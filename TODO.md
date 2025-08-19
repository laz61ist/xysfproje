### **Güncellenmiş TODO - Faz 1: Çekirdek Deneyim ve MVP**
*(Bu, `warmup.php` betiğini içeren son ve eksiksiz halidir.)*

````markdown
# TODO - Faz 1: Çekirdek Deneyim ve MVP (Lansman Kalitesiyle)

Bu doküman, `PROJECT_ROADMAP.md`'de tanımlanan **Faz 1**'i hayata geçirmek için gereken, lansman kalitesini artırıcı adımları da içeren tüm teknik görevleri listeler.

---

## **Milestone 1.1 - 1.4:** (Önceki versiyonlarla aynı, değişiklik yok)
*(Proje Kurulumu, Veritabanı, Kullanıcı Sistemi, Servis Entegrasyonları ve Çekirdek Sayfaların Geliştirilmesi)*

---

## **Milestone 1.5: Dağıtım ve Lansman**
*(**Hedef:** MVP'yi canlıya almak ve operasyonel hale getirmek.)*

-   [ ] **1.5.1. Sunucu Hazırlığı**
    -   [ ] `[DevOps]` Canlı Linux sunucusunu hazırla (Nginx/Apache, PHP, Git vb.).
    -   [ ] `[DevOps]` Let's Encrypt ile SSL sertifikasını kur ve HTTPS'i zorunlu kıl.
-   [ ] **1.5.2. Otomatik Dağıtım (CI/CD)**
    -   [ ] `[DevOps]` Git tabanlı otomatik dağıtım için sunucuda `post-receive` hook betiğini oluştur ve yapılandır.
    -   [ ] `[DevOps]` Yerel makineden `git remote add production ...` komutu ile sunucuyu ekle.
-   [ ] **1.5.3. Lansman Öncesi Veritabanını "Isıtma" (Warm-up) - (YENİ GÖREV)**
    -   [ ] `[DB]` `DATABASE_SCHEMA.md` v1.2'ye göre `genres` ve `movie_genres` tablolarını Supabase'de oluştur.
    -   [ ] `[Backend]` Projenin kök dizininde, sunucuda komut satırından çalıştırılacak bir `warmup.php` betiği oluştur.
    -   [ ] `[Backend]` Bu betiğin, `DEPLOYMENT_AND_OPERATIONS_PLAN.md` v1.1'de tanımlanan adımları (türleri çekme, popüler 250 filmi çekme, her filmi DB'ye ve AI analizlerini `movie_analyses` tablosuna kaydetme) uygulamasını sağla.
    -   [ ] `[DevOps]` Projeyi canlıya göndermeden **hemen önce**, bu `warmup.php` betiğini sunucuda **bir kez** çalıştır.
    -   [ ] `[Referans]` `19. DEPLOYMENT_AND_OPERATIONS_PLAN.md`, `5. DATABASE_SCHEMA.md`
-   [ ] **1.5.4. Canlıya Geçiş**
    -   [ ] `[DevOps]` Canlı sunucuda, production anahtarlarını içeren `.env` dosyasını oluştur.
    -   [ ] `[DevOps]` `git push production main` komutu ile projeyi ilk kez canlıya gönder.
-   [ ] **1.5.5. Son Kontroller**
    -   [ ] `[Test]` Canlı site URL'i üzerinden tüm ana kullanıcı akışlarını baştan sona test et.
    -   [ ] **FAZ 1 TAMAMLANDI - MVP YAYINDA!**
````

---
---

### **Güncellenmiş TODO - Faz 2: Topluluk ve Gelişmiş Keşif**
*(Bu, Fragman Önizlemesi ve Kategori Sayfaları özelliklerini içeren son ve eksiksiz halidir.)*

````markdown
# TODO - Faz 2: Topluluk ve Gelişmiş Keşif

Bu doküman, `PROJECT_ROADMAP.md`'de tanımlanan **Faz 2**'yi hayata geçirmek için gereken tüm teknik görevleri detaylı ve sıralı bir şekilde listeler.

---

## **Milestone 2.1 - 2.4:** (Önceki versiyonlarla aynı, değişiklik yok)
*(Etkileşim Veritabanı, Etkileşim Arayüzü, Liste Yönetim Sistemi ve Profil Geliştirmeleri)*

---

## **Milestone 2.5: Gelişmiş Keşif Özellikleri (Advanced Discovery)**
*(**Amaç:** Kullanıcıların film keşfetme yollarını zenginleştirmek ve arama deneyimini daha interaktif hale getirmek.)*

-   [ ] **1. Kategori (Tür) Sayfalarını Oluştur**
    -   [ ] `[Backend]` `app/Controllers/GenreController.php` adında yeni bir controller oluştur.
    -   [ ] `[Backend]` Bu controller içinde, belirli bir türe (`slug` ile) ait olan filmleri veritabanından (`movie_genres` ara tablosunu kullanarak) çeken ve sayfalandıran (`pagination`) bir `show()` metodu oluştur.
    -   [ ] `[Frontend]` `/genre/{slug}` URL'inde çalışacak, gelen film listesini bir grid yapısında gösteren `views/genre/show.php` adında bir view dosyası oluştur.
    -   [ ] `[Frontend]` Ana sayfaya ve film detay sayfalarındaki tür etiketlerine, bu yeni kategori sayfalarına giden linkleri ekle.
    -   [ ] `[Referans]` `18. UI_UX_FLOW_AND_WIREFRAMES.md`, `5. DATABASE_SCHEMA.md`

-   [ ] **2. Aramada Fragman Önizlemesi Özelliğini Geliştir**
    -   [ ] `[Frontend/JS]` Arama sonuçları sayfası için özel bir JavaScript dosyası oluştur.
    -   [ ] `[Frontend/JS]` Bu JS dosyasının içinde, `THIRD_PARTY_API_CONTRACTS.md` v1.2'de tanımlandığı gibi, listelenen her film için arka planda TMDB'nin `/movie/{movie_id}/videos` uç noktasına asenkron bir AJAX isteği atarak fragman anahtarını (`key`) alan bir fonksiyon yaz.
    -   [ ] `[Frontend/JS]` Kullanıcı bir film kartının üzerine fareyle geldiğinde (`mouseenter`) ve ayrıldığında (`mouseleave`) tetiklenecek event listener'lar oluştur.
    -   [ ] `[Frontend/JS]` `mouseenter` olayında, ilgili filmin fragman anahtarını kullanarak sessiz ve otomatik oynayan bir YouTube `iframe`'ini dinamik olarak oluşturan ve film afişinin üzerine yerleştiren; `mouseleave` olayında ise bu `iframe`'i kaldıran mantığı implemente et.
    -   [ ] `[Referans]` `17. THIRD_PARTY_API_CONTRACTS.md`, `18. UI_UX_FLOW_AND_WIREFRAMES.md`
````
