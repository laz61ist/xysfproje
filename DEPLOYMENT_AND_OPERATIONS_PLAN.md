# Deployment and Operations Plan (Dağıtım ve Operasyon Planı)

**Document Version:** 1.1
**Last Updated:** 18.08.2025

## 1. Amaç

Bu doküman, `Sinefil Radarı` projesinin yerel geliştirme ortamından (Windows) canlı üretim ortamına (Linux sunucu) dağıtım (deployment) sürecini, sunucu gereksinimlerini ve platform yayına alındıktan sonraki operasyonel bakım ve izleme prosedürlerini detaylandırır. Amaç, sorunsuz, tekrarlanabilir ve güvenli bir dağıtım süreci oluşturmaktır.

## 2. Sunucu Ortamı Gereksinimleri (Production Server)

-   **İşletim Sistemi:** Linux dağıtımı (Ubuntu 22.04 LTS veya CentOS Stream önerilir).
-   **Web Sunucusu:** Apache 2.4 veya Nginx. Güvenlik ve performans için Nginx tercih sebebidir.
-   **PHP:** Sürüm 8.1 veya üstü. Gerekli PHP eklentileri (`php-curl`, `php-pgsql`, `php-mbstring` vb.) kurulmalıdır.
-   **Paket Yöneticisi:** Composer 2.x.
-   **Veritabanı:** Gerekli değil. Veritabanı ve kullanıcı yönetimi tamamen **Supabase** tarafından yönetilecektir. Sunucunun Supabase'e HTTPS (port 443) üzerinden erişimi olmalıdır.
-   **Güvenlik:**
    -   UFW (Uncomplicated Firewall) veya benzeri bir güvenlik duvarı aktif edilmeli, sadece gerekli portlar (80, 443, 22) açık bırakılmalıdır.
    -   Let's Encrypt kullanılarak ücretsiz SSL sertifikası kurulmalı ve tüm trafik HTTPS üzerinden zorunlu kılınmalıdır.
    -   SSH erişimi şifre yerine SSH anahtarı ile sağlanmalıdır.

## 3. Dağıtım Süreci (Deployment Process)

Dağıtım, manuel dosya transferi yerine, tekrarlanabilir ve daha az hataya açık olan Git tabanlı bir yaklaşımla yapılacaktır.

### Adım 1: Sunucu Hazırlığı (İlk Kurulum)
1.  Sunucuda bir sistem kullanıcısı oluşturulur (örn: `sinefil`).
2.  Web sunucusu (Nginx/Apache), PHP ve Git kurulur.
3.  Proje için bir dizin oluşturulur (örn: `/var/www/sinefilradari`).
4.  Bu dizinde, `git init --bare` komutu ile "çıplak" bir Git deposu oluşturulur.
5.  Bu depoda, `post-receive` adında bir Git hook betiği oluşturulur.

### Adım 2: Otomatik Dağıtım Betiği (`post-receive` hook)
```bash
#!/bin/bash
# post-receive hook

TARGET="/var/www/sinefilradari/live"
GIT_DIR="/var/www/sinefilradari/repo.git"
BRANCH="main"

mkdir -p $TARGET
git --work-tree=$TARGET --git-dir=$GIT_DIR checkout $BRANCH -f

echo "Running composer install..."
cd $TARGET
composer install --no-dev --optimize-autoloader

# ... (gerekirse dosya izinlerini ayarlama komutları) ...

echo "Deployment successful!"
```

### Adım 3: Yerel Makineden Dağıtım Yapma
1.  Yerel Git projesine sunucu bir "remote" olarak eklenir:
    ```bash
    git remote add production sinefil@YOUR_SERVER_IP:/var/www/sinefilradari/repo.git
    ```
2.  Canlıya atmak istenen kod hazır olduğunda, tek bir komut ile dağıtım yapılır:
    ```bash
    git push production main
    ```

### Adım 4: Ortam Değişkenlerini Yönetme
-   Sunucudaki proje dizininde (`/var/www/sinefilradari/live`), **ASLA** Git'e dahil edilmeyecek bir `.env` dosyası manuel olarak oluşturulur.
-   Bu dosyaya, canlı sunucuya özel değerler girilir (`APP_ENV=production`, `BASE_URL=https://www.sinefilradari.com`, production API anahtarları vb.).

## 4. Lansman Öncesi Prosedürler (Pre-Launch Procedures) - (YENİ BÖLÜM)

-   **Amaç:** Sitenin ilk ziyaretçilerine en iyi deneyimi sunmak için "Soğuk Başlangıç" (Cold Start) problemini ortadan kaldırmak.
-   **Adım 1: Veritabanını "Isıtma" Betiğini Çalıştırma**
    -   **Eylem:** Projenin canlıya alınmasından **hemen önce**, sunucuda SSH üzerinden `php warmup.php` gibi özel bir komut dosyası çalıştırılacaktır.
    -   **Betiğin Görevleri:**
        1.  TMDB API'sinden tüm film türlerini (`genres`) çeker ve veritabanına kaydeder.
        2.  TMDB API'sinden en popüler ~250 filmi çeker.
        3.  Her bir film için:
            -   Tüm detaylarını (oyuncular, fragman vb.) çeker ve `movies` tablomuza kaydeder.
            -   Filmi ilgili türlerle (`movie_genres` tablosu) ilişkilendirir.
            -   `GemmaService`'i tetikleyerek film için yapay zeka analizini oluşturur ve `movie_analyses` tablomuza kaydeder.
            -   Yapılan her AI çağrısını `ai_usage_logs` tablosuna kaydeder.
    -   **Sonuç:** Bu işlem, sitenin en çok ziyaret edilecek sayfalarının ve verilerinin lansman anında süper hızlı olmasını ve AI analizlerinin hazır bulunmasını sağlar.

## 5. Operasyonel Görevler ve Bakım

### 5.1. Zamanlanmış Görevler (Cron Jobs)
-   Linux sunucunun `crontab` özelliği, platformumuzun periyodik görevlerini otomatikleştirmek için kullanılacaktır.
-   **Örnek Cron Job'lar:**
    -   **Haftalık Keşif Radarı Oluşturma:** `0 5 * * 1 php /var/www/sinefilradari/live/artisan.php generate:weekly-discovery`
    -   **Topluluk Yorumlarını Özetleme:** `0 3 * * * php /var/www/sinefilradari/live/artisan.php summarize:reviews`

### 5.2. Yedekleme (Backup)
-   **Uygulama Kodu:** Git'te versiyonlandığı için güvendedir.
-   **Kullanıcı Tarafından Yüklenen Dosyalar:** Supabase Storage kullanıldığı için, dosyaların yedeklenmesi Supabase'in sorumluluğundadır.
-   **Veritabanı:** Supabase, veritabanını otomatik olarak yedekler. Admin panelinden düzenli olarak manuel yedeklemeler alınması önerilir.

### 5.3. İzleme ve Loglama (Monitoring & Logging)
-   **Uygulama Hataları:** PHP, `production` modundayken hataları sunucudaki bir log dosyasına yazacak şekilde yapılandırılmalıdır.
-   **Sunucu Sağlığı:** Sunucunun CPU, RAM ve disk kullanımı gibi temel metrikleri izlenmelidir.
-   **API Kullanımı:** Google AI, TMDB gibi platformların kendi panelleri üzerinden API kullanım limitleri ve maliyetler düzenli olarak takip edilmelidir.
