# Environment Config and URL Strategy (Ortam Yapılandırması ve URL Stratejisi)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Amaç

Bu dokümanın temel amacı, `Sinefil Radarı` projesinin farklı çalışma ortamlarında (örneğin, bir geliştiricinin yerel Windows makinesi, test sunucusu, canlı Linux sunucusu) hiçbir kod değişikliği yapmadan sorunsuzca çalışmasını sağlayacak bir yapılandırma stratejisi tanımlamaktır. Bu, özellikle URL oluşturma, veritabanı bağlantıları ve harici API anahtarlarının yönetimi için kritik öneme sahiptir.

## 2. Temel Prensip: "Kodda Değil, Ortamda Yapılandır"

Uygulama kodunun içine asla ortama özgü (environment-specific) değerler yazılmayacaktır. Tüm yapılandırma değerleri, projenin dışında tutulan ve sürümlenmeyen (versiyon kontrolüne eklenmeyen) bir `.env` dosyası aracılığıyla yönetilecektir.

## 3. `.env` Dosya Yapısı

Projenin kök dizininde iki adet dosya bulunacaktır:

1.  **`.env.example`:** Bu dosya, projede kullanılan tüm ortam değişkenlerinin bir listesini ve açıklamalarını içerir. **Bu dosya Git repomuza commit edilecektir.** Yeni bir geliştirici projeyi klonladığında, hangi ayarlara ihtiyacı olduğunu bu dosyaya bakarak görecektir.
2.  **`.env`:** Bu dosya, `.env.example` dosyasının bir kopyasıdır ve geliştiricinin kendi yerel ortamına veya sunucuya özel gerçek değerleri içerir. **Bu dosya `.gitignore` içinde yer alacak ve ASLA Git repomuza commit edilmeyecektir.** Bu, hassas bilgilerin (API anahtarları, şifreler) güvende kalmasını sağlar.

### Örnek `.env.example` Dosyası

```dotenv
# ----------------------------------
# Uygulama Temel Ayarları
# ----------------------------------
# Uygulamanın çalışma modunu belirtir. Olası değerler: development, production
# 'development' modunda detaylı hata mesajları gösterilirken, 'production' modunda gizlenir.
APP_ENV=development

# Uygulamanın kök URL'ini belirtir. CSS, JS ve linklerin doğru oluşturulması için kritiktir.
# ASLA sonunda / (slash) karakteri bulunmamalıdır.
# Örnek Yerel (XAMPP): BASE_URL=http://localhost/sinefil-radari/public
# Örnek Sunucu: BASE_URL=https://www.sinefilradari.com
BASE_URL=

# ----------------------------------
# Supabase Entegrasyon Ayarları
# ----------------------------------
# Supabase proje panelinden alınan URL.
SUPABASE_URL=""

# Supabase proje panelinden alınan, tarayıcı tarafında güvenle kullanılabilen anonim (public) anahtar.
SUPABASE_ANON_KEY=""

# Supabase proje panelinden alınan, SADECE backend (PHP) tarafında kullanılacak olan servis rolü (gizli) anahtarı.
SUPABASE_SERVICE_ROLE_KEY=""

# ----------------------------------
# Harici API Servis Ayarları
# ----------------------------------
# The Movie Database (TMDB) API v3 anahtarınız.
TMDB_API_KEY=""

# Google AI Studio'dan alınan Gemma API anahtarınız.
GEMMA_API_KEY=""

# JustWatch API anahtarınız (eğer kullanılıyorsa).
JUSTWATCH_API_KEY=""
```

## 4. `BASE_URL` Stratejisi

Doğru ve esnek bir `BASE_URL` yönetimi, projenin taşınabilirliği için hayati önem taşır.

-   **Neden Gerekli?** Uygulama içindeki tüm linkler (menü linkleri, yönlendirmeler) ve statik dosya yolları (CSS, JS, resimler) mutlak (absolute) yollarla oluşturulmalıdır. Bu, `http://localhost/proje/public/css/style.css` gibi bir yolun, sunucuda otomatik olarak `https://site.com/css/style.css` haline gelmesini sağlar.
-   **Uygulama:** PHP tarafında, bu `.env` değişkenini okuyacak ve tüm URL'leri oluştururken kullanacak bir yardımcı fonksiyon (helper function) oluşturulacaktır.

    **Örnek PHP Helper Fonksiyonu:**

    ```php
    // helpers.php veya benzeri bir dosyada
    function baseUrl(string $path = ''): string
    {
        // .env'den BASE_URL'i oku
        $baseUrl = $_ENV['BASE_URL'];
        
        // Gelen yolun başında / varsa temizle
        $path = ltrim($path, '/');
        
        return $baseUrl . '/' . $path;
    }
    ```

    **Örnek Kullanım (View Dosyasında):**

    ```html
    <!-- CSS dosyasını çağırma -->
    <link rel="stylesheet" href="<?php echo baseUrl('css/style.css'); ?>">

    <!-- Menü linki oluşturma -->
    <a href="<?php echo baseUrl('movie/dune'); ?>">Dune Film Detayları</a>
    ```

## 5. Uygulama Akışı

1.  **Geliştirici Projeyi Klonlar:** `git clone ...`
2.  **`.env` Dosyasını Oluşturur:** `cp .env.example .env` komutu ile `.env` dosyasını oluşturur.
3.  **`.env` Dosyasını Düzenler:** Kendi yerel Windows XAMPP ortamına göre `BASE_URL`, Supabase ve diğer API anahtarlarını `.env` dosyasına girer.
4.  **Uygulamayı Çalıştırır:** Proje, bu `.env` dosyasındaki ayarlarla yerel ortamda sorunsuzca çalışır.
5.  **Sunucuya Yükleme:** Proje dosyaları sunucuya yüklendiğinde, sunucu üzerinde yeni bir `.env` dosyası oluşturulur ve bu sefer sunucunun `BASE_URL`'i (`https://www.sinefilradari.com`) ve varsa production veritabanı bilgileri girilir.
6.  **Sonuç:** Kod tabanında hiçbir değişiklik yapmadan, proje hem yerel geliştirme ortamında hem de canlı sunucuda doğru URL'ler ve yapılandırma ile çalışır.

Bu strateji, projenin başından sonuna kadar temiz, güvenli ve taşınabilir bir yapılandırma yönetimi sağlar.
