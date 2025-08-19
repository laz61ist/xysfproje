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
# TODO - Faz 4: Monetizasyon ve Operasyonel Mükemmellik

Bu doküman, `PROJECT_ROADMAP.md`'de tanımlanan **Faz 4**'ü hayata geçirmek için gereken tüm teknik görevleri detaylı ve sıralı bir şekilde listeler.

---

## **▶️ Faz 4: Monetizasyon ve Operasyonel Mükemmellik**
*(**Hedef:** Platformun finansal sürdürülebilirliğini sağlamak, operasyonel kontrolü artırmak ve viral büyümeyi teşvik edecek özellikler eklemek.)*

### **Milestone 4.1: Gelir Modelleri Altyapısı**
*(**Amaç:** Platform için sürdürülebilir gelir akışlarının teknik temelini kurmak.)*

-   [ ] **1. Yönlendirme Ortaklığı (Affiliate) Sistemini Kur**
    -   [ ] `[Admin/DB]` `affiliate_partners` (`partner_name`, `base_url`, `affiliate_tag`) adında yeni bir veritabanı tablosu oluştur.
    -   [ ] `[Admin/Backend]` Admin Panelinde, bu `affiliate_partners` tablosunu yönetmek için bir CRUD (Create, Read, Update, Delete) arayüzü oluştur.
    -   [ ] `[Backend]` "Nerede İzleyebilirim?" verisini işleyen servisi, `affiliate_partners` tablosundaki bilgilere göre yönlendirme linklerine affiliate etiketini otomatik olarak ekleyecek şekilde güncelle.
    -   [ ] `[Referans]` `15. MONETIZATION_STRATEGY.md`, `16. ADMIN_DASHBOARD_SPECS.md`

-   [ ] **2. Gönüllü Destekçi Modelini (Supporter Model) Entegre Et**
    -   [ ] `[Entegrasyon]` "Buy Me a Coffee" veya Stripe üzerinde bir hesap oluştur ve API anahtarlarını al.
    -   [ ] `[DB]` `users` tablosuna `is_supporter` (boolean, default `false`) sütununu ekle.
    -   [ ] `[Backend]` Ödeme platformundan gelen webhook (başarılı ödeme bildirimi) isteklerini yakalayacak güvenli bir API ucu (`/api/webhooks/payment`) oluştur. Bu uç nokta, ilgili kullanıcının `is_supporter` statüsünü `true` olarak güncellemeli.
    -   [ ] `[Frontend]` Sitenin çeşitli yerlerine (header, footer) "Destek Ol" butonunu/linkini ekle.
    -   [ ] `[Referans]` `15. MONETIZATION_STRATEGY.md`

-   [ ] **3. Stratejik Reklam Alanlarını Yerleştir**
    -   [ ] `[Admin/Backend]` Admin Panelinde, reklam alanlarını (`header_banner`, `sidebar_ad` vb.) ve bu alanlara eklenecek Google AdSense kodlarını yönetmek için bir arayüz ve veritabanı tablosu oluştur.
    -   [ ] `[Backend]` View'leri render eden ana mekanizmayı, `!currentUser.is_supporter` kontrolü yapacak şekilde güncelle. Eğer kullanıcı destekçi değilse, veritabanından ilgili reklam kodunu çekip HTML'e yerleştir.
    -   [ ] `[Frontend]` Reklamların `UI_UX_FLOW_AND_WIREFRAMES.md`'de belirtilen yerlerde, içeriği bozmayacak şekilde göründüğünü doğrula.
    -   [ ] `[Referans]` `15. MONETIZATION_STRATEGY.md`

### **Milestone 4.2: AI Maliyet Takip Sisteminin Kurulması**
*(**Amaç:** Yapay zeka kullanımını ve maliyetlerini şeffaf bir şekilde izlemek ve kontrol etmek.)*

-   [ ] **1. Veritabanı ve Yapılandırma**
    -   [ ] `[DB]` `DATABASE_SCHEMA.md`'de tanımlanan `ai_usage_logs` tablosunu Supabase'de oluştur.
    -   [ ] `[Backend]` Yapay zeka maliyet oranlarını saklamak için bir `config/ai.php` dosyası oluştur ve değerleri gir.
    -   [ ] `[Referans]` `5. DATABASE_SCHEMA.md` (v1.1)

-   [ ] **2. Loglama Mekanizmasını Geliştir (Backend)**
    -   [ ] `[Backend]` `GemmaService.php` sınıfını, her API çağrısından sonra `usageMetadata`'yı işleyip `ai_usage_logs` tablosuna kayıt atacak `logAiUsage` fonksiyonu ile refaktör et.
    -   [ ] `[Referans]` `7. GEMMA3_INTEGRATION_BLUEPRINT.md` (v1.2)

-   [ ] **3. Maliyet Merkezi Arayüzünü Oluştur (Admin Panel)**
    -   [ ] `[Admin/Backend]` Admin paneli için `ai_usage_logs` tablosundan veri çeken (toplam maliyet, günlük kullanım vb.) ve raporlama için hazırlayan yeni metodlar oluştur.
    -   [ ] `[Admin/Frontend]` Admin paneline "AI Maliyet Merkezi" adında yeni bir bölüm ekle.
    -   [ ] `[Admin/Frontend]` Bu bölümde, backend'den gelen verileri gösteren özet widget'larını, zaman serisi grafiğini ve detaylı log tablosunu geliştir.
    -   [ ] `[Referans]` `16. ADMIN_DASHBOARD_SPECS.md` (v1.1)

### **Milestone 4.3: Büyüme ve İletişim Araçları**
*(**Amaç:** Kullanıcılarla çift yönlü iletişimi güçlendirmek ve viral büyümeyi teşvik etmek.)*

-   [ ] **1. Sinefil Karnesi (Cinephile Wrapped) Geliştir**
    -   [ ] `[Backend]` Belirli bir kullanıcının o yıl içindeki tüm etkileşimlerini analiz edip `PERSONALIZATION_ALGORITHMS.md`'de belirtilen metrikleri hesaplayan bir `WrappedService` veya komut dosyası oluştur.
    -   [ ] `[AI]` `GemmaService`'e, bu istatistiksel verileri alıp kişiselleştirilmiş ve esprili metinler üretecek yeni bir metod ekle.
    -   [ ] `[Frontend]` Her yıl sonunda erişilebilir olacak, `/wrapped/{year}` URL'inde çalışan, animasyonlu, görsel olarak zengin ve paylaşılabilir bir sayfa tasarla ve geliştir.
    -   [ ] `[Referans]` `12. PERSONALIZATION_ALGORITHMS.md`

-   [ ] **2. İletişim Kanallarını Kur**
    -   [ ] `[DB]` `feedback` ve `announcements` tablolarını oluştur.
    -   [ ] `[Frontend]` `/feedback` sayfasını ve "İstek & Öneri" formunu oluştur.
    -   [ ] `[Admin]` Admin Panelinde gelen geri bildirimleri okumak ve yeni duyurular oluşturmak için arayüzler ekle.
    -   [ ] `[Frontend]` Header'a bildirim zilini (🔔) ve `/whats-new` duyuru sayfasını ekle.
    -   [ ] `[Referans]` `18. UI_UX_FLOW_AND_WIREFRAMES.md`

-   [ ] **3. Gelişmiş Moderasyon Araçları**
    -   [ ] `[DB]` `reports` tablosunu oluştur.
    -   [ ] `[Frontend]` Tüm kullanıcı içeriklerinin (yorum, liste vb.) yanına "Raporla" butonu ve modal formunu ekle.
    -   [ ] `[Admin]` Admin Panelinde, gelen raporları listeleyen ve hızlı aksiyon almayı sağlayan bir "Rapor Kuyruğu" arayüzü geliştir.
    -   [ ] `[Referans]` `14. COMMUNITY_FEATURES_AND_MODERATION.md`

-   [ ] **4. Tartışma Kulüpleri (İlk Versiyon)**
    -   [ ] `[DB]` `forums`, `threads`, ve `posts` tablolarını oluştur.
    -   [ ] `[Backend/Frontend]` Kullanıcıların forumları listeleyebileceği, yeni tartışma başlıkları açabileceği ve başlıklara yanıt yazabileceği temel arayüzü ve mantığı geliştir.
    -   [ ] `[Referans]` `14. COMMUNITY_FEATURES_AND_MODERATION.md`
