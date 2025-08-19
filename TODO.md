# TODO - Faz 1: Çekirdek Deneyim ve MVP (Profesyonel İş Akışıyla)

**Document Version:** 2.1
**Last Updated:** 18.08.2025

Bu doküman, `PROJECT_ROADMAP.md`'de tanımlanan **Faz 1**'i hayata geçirmek için gereken tüm teknik görevleri, **profesyonel Git iş akışını içerecek şekilde** detaylı, sıralı ve birbirine bağımlı bir şekilde listeler.

---

## **Milestone 1.1: Proje Temelleri ve Altyapı**
*(**Hedef:** Geliştirme ortamını kurmak, projenin iskeletini oluşturmak ve ilk versiyon kontrolünü sağlamak.)*

-   [ ] **1.1.1. Ortam Kurulumu**
    -   [ ] `[Infra]` Supabase projesini oluştur ve API anahtarlarını (`URL`, `ANON_KEY`, `SERVICE_ROLE_KEY`) güvenli bir yere kaydet.
    -   [ ] `[DevOps]` Proje için bir GitHub/GitLab deposu (private) oluştur ve yerel Windows geliştirme ortamına klonla.
    -   [ ] `[DevOps]` Yerel Windows (XAMPP/WAMP) ortamında Apache Virtual Host ayarını yaparak proje için temiz bir URL (`http://sinefil-radari.test` gibi) tanımla.

-   [ ] **1.1.2. Proje Mimarisi**
    -   [ ] `[Backend]` `ARCHITECTURE_OVERVIEW.md`'de tanımlanan klasör yapısını oluştur (`app/Controllers`, `app/Models`, `app/Views`, `public`, `config` vb.).
    -   [ ] `[Backend]` Composer'ı başlat (`composer init`) ve `vlucas/phpdotenv` kütüphanesini yükle.
    -   [ ] `[Backend]` `.gitignore` dosyasına `.env` ve `/vendor/` dizinini ekle.
    -   [ ] `[Backend]` `.env.example` dosyasını temel değişkenlerle oluştur.
    -   [ ] `[Backend]` Gelen tüm istekleri yakalayan bir `public/index.php` (Front Controller) dosyası oluştur.
    -   [ ] `[Backend]` Basit bir `app/Router.php` sınıfı ve `baseUrl()` yardımcı fonksiyonunu oluştur.

-   [ ] **1.1.3. İlk Commit ve Push (Versiyon Kontrolünü Başlatma)**
    -   [ ] `[Git]` Proje klasöründeki tüm yeni dosyaları `git add .` komutu ile ekle.
    -   [ ] `[Git]` `git commit -m "Milestone 1.1: Initial project structure and setup complete"` komutu ile ilk anlamlı kaydı oluştur.
    -   [ ] `[Git]` `git push origin main` komutu ile kodunun ilk yedeğini ve versiyonunu **GitHub/GitLab'e gönder.**

---

## **Milestone 1.2: Veritabanı ve Kullanıcı Sistemi**
*(**Hedef:** Veri yapısını kurmak ve kullanıcıların sisteme kaydolup giriş yapabilmesini sağlamak.)*

-   [ ] **1.2.1. Veritabanı Şeması (Supabase)**
    -   [ ] `[DB]` `users`, `movies`, ve `movie_analyses` tablolarını Supabase'de oluştur.
    -   [ ] `[DB]` `users` tablosu için RLS politikalarını ve `handle_new_user` PostgreSQL trigger'ını oluştur.
-   [ ] **1.2.2. Kullanıcı Kimlik Doğrulama (Backend & Frontend)**
    -   [ ] `[Backend]` `AuthController.php` ve `User.php` modelini oluşturarak kayıt/giriş mantığını implemente et.
    -   [ ] `[Frontend]` Kayıt (`views/auth/register.php`) ve Giriş (`views/auth/login.php`) için temel HTML formlarını oluştur.
    -   [ ] `[Frontend]` Header'ı, kullanıcının oturum durumuna göre dinamik hale getir.
-   [ ] **1.2.3. İlerlemeyi Kaydet (Commit & Push Progress)**
    -   [ ] `[Git]` `git add .`
    -   [ ] `[Git]` `git commit -m "Milestone 1.2: Implement user authentication and database schema"`
    -   [ ] `[Git]` `git push origin main` **(Kodunu GitHub'da güvene al.)**

---

## **Milestone 1.3: Harici Servis Entegrasyonları**
*(**Hedef:** Projemizin TMDB ve Gemma 3 ile konuşabilmesini sağlayan servisleri oluşturmak.)*

-   [ ] **1.3.1. TMDB ve Gemma 3 Servisleri**
    -   [ ] `[Backend]` `TMDBService.php` sınıfını ve ilgili metodlarını (`getMovieDetails` vb.) oluştur. Film verilerini `movies` tablosuna kaydeden önbellekleme mantığını ekle.
    -   [ ] `[Backend]` `GemmaService.php` sınıfını ve `analyzeMovieContent` metodunu oluştur. Gelen analizi `movie_analyses` tablosuna kaydeden mantığı ekle.
-   [ ] **1.3.2. İlerlemeyi Kaydet (Commit & Push Progress)**
    -   [ ] `[Git]` `git add .`
    -   [ ] `[Git]` `git commit -m "Milestone 1.3: Integrate TMDB and Gemma 3 services"`
    -   [ ] `[Git]` `git push origin main` **(Kodunu GitHub'da güvene al.)**

---

## **Milestone 1.4: Çekirdek Özelliklerin Hayata Geçirilmesi**
*(**Hedef:** Tüm parçaları birleştirerek kullanıcıların göreceği ana sayfaları ve özellikleri oluşturmak.)*

-   [ ] **1.4.1. Ana Sayfa ve Film Detay Sayfası**
    -   [ ] `[Backend]` `HomeController` ve `MovieController` sınıflarını, servisleri kullanarak veri çekecek ve view'lere gönderecek şekilde tamamla.
    -   [ ] `[Frontend]` Ana Sayfa ve Film Detay Sayfası'nın HTML/CSS/JS'ini tamamla (Fragman modal'ı dahil).
-   [ ] **1.4.2. Profil Sayfası (Temel)**
    -   [ ] `[Backend/Frontend]` Temel profil sayfasını (`ProfileController` ve `show.php` view) oluştur.
-   [ ] **1.4.3. İlerlemeyi Kaydet (Commit & Push Progress)**
    -   [ ] `[Git]` `git add .`
    -   [ ] `[Git]` `git commit -m "Milestone 1.4: Complete core pages and features for MVP"`
    -   [ ] `[Git]` `git push origin main` **(Kodunu GitHub'da güvene al.)**

---

## **Milestone 1.5: Dağıtım ve Lansman**
*(**Hedef:** MVP'yi canlıya almak ve operasyonel hale getirmek.)*

-   [ ] **1.5.1. Sunucu Hazırlığı ve Otomatik Dağıtım**
    -   [ ] `[DevOps]` Canlı Linux sunucusunu hazırla (Nginx/Apache, PHP, SSL vb.).
    -   [ ] `[DevOps]` `git push production main` komutuyla çalışacak olan otomatik dağıtım (`post-receive` hook) sistemini kur.
-   [ ] **1.5.2. Son Kontroller ve Canlıya Geçiş**
    -   [ ] `[DevOps]` Canlı sunucuda, production anahtarlarını içeren `.env` dosyasını oluştur.
    -   [ ] `[Test]` Geliştirme branch'ini ana branch (`main`) ile birleştir ve son bir kez `git push origin main` ile GitHub'a gönder.
    -   [ ] `[DevOps]` **`git push production main` komutu ile projeyi ilk kez canlıya gönder.**
-   [ ] **1.5.3. Lansman Sonrası Test**
    -   [ ] `[Test]` Canlı site URL'i üzerinden tüm ana kullanıcı akışlarını baştan sona test et.
    -   [ ] **FAZ 1 TAMAMLANDI - MVP YAYINDA!**

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

# TODO - Faz 3: Kişiselleştirme ve Oyunlaştırma

Bu doküman, `PROJECT_ROADMAP.md`'de tanımlanan **Faz 3**'ü hayata geçirmek için gereken tüm teknik görevleri detaylı ve sıralı bir şekilde listeler.

---

## **▶️ Faz 3: Kişiselleştirme ve Oyunlaştırma**
*(**Hedef:** Platformu her kullanıcı için benzersiz, "akıllı" ve bağımlılık yaratan bir deneyime dönüştürmek. Kullanıcıların platformda geçirdiği zamanı ödüllendirerek uzun vadeli bağlılıklarını artırmak.)*

### **Milestone 3.1: Oyunlaştırma Sistemi Altyapısı**
*(**Amaç:** Kullanıcı etkileşimlerini ödüllendirecek puan ve rozet sisteminin temelini atmak.)*

-   [ ] **1. Veritabanı Şemasını Güncelle**
    -   [ ] `[DB]` `users` tablosuna `cine_points` adında, varsayılan değeri `0` olan bir `integer` sütunu ekle.
    -   [ ] `[DB]` `DATABASE_SCHEMA.md`'yi referans alarak `badges` (tüm olası rozetlerin tanımı) ve `user_badges` (kullanıcıların kazandığı rozetler) tablolarını oluştur.
    -   [ ] `[DB]` `badges` tablosuna, `GAMIFICATION_SYSTEM.md`'de listelenen en az 10 başlangıç rozetini (örn: "First Critic", "Curator", "Horror Aficionado") manuel olarak ekle.
    -   [ ] `[Referans]` `5. DATABASE_SCHEMA.md`, `13. GAMIFICATION_SYSTEM.md`

-   [ ] **2. Oyunlaştırma Servisini Geliştir (Backend)**
    -   [ ] `[Backend]` `app/Services/GamificationService.php` adında yeni bir servis sınıfı oluştur.
    -   [ ] `[Backend]` Bu servisin içinde, belirli bir eylem gerçekleştiğinde (örn: "comment_created") kullanıcının toplam puanını ve rozet kazanma durumunu kontrol eden `checkAndAwardBadges($userId, $action)` gibi bir metod oluştur.
    -   [ ] `[Backend]` `InteractionController` ve `CommentController` gibi mevcut controller'ları, başarılı bir işlemden sonra (örn: yeni yorum eklendiğinde) `GamificationService`'i çağıracak şekilde güncelle.
    -   [ ] `[Referans]` `13. GAMIFICATION_SYSTEM.md`

### **Milestone 3.2: Kişiselleştirme Motoru (Backend)**
*(**Amaç:** Kullanıcı verilerini analiz ederek kişisel öneriler üreten "akıllı" sistemleri kurmak.)*

-   [ ] **1. Haftalık Keşif Radarı (Discover Weekly) Cron Job'unu Oluştur**
    -   [ ] `[Backend]` `app/Commands/GenerateWeeklyRecommendations.php` adında bir komut dosyası oluştur.
    -   [ ] `[Backend]` Bu komutun, `PERSONALIZATION_ALGORITHMS.md`'de tanımlanan algoritma akışını (kullanıcının zevk profilini çıkarma, aday havuzu oluşturma, daha önce izlediklerini filtreleme) izlemesini sağla.
    -   [ ] `[Backend]` `GemmaService`'e, `PROMPT_ENGINEERING_GUIDE.md`'deki `generate_weekly_discovery` şablonunu kullanarak kişiselleştirilmiş öneriler üretecek yeni bir metod ekle.
    -   [ ] `[DB]` Sonuçları saklamak için `weekly_recommendations` (`user_id`, `recommendations_json`, `week_date`) adında bir tablo oluştur.
    -   [ ] `[DevOps]` Bu komut dosyasını her Pazartesi sabahı çalıştıracak bir cron job'u sunucuya ekle.
    -   [ ] `[Referans]` `12. PERSONALIZATION_ALGORITHMS.md`, `8. PROMPT_ENGINEERING_GUIDE.md`, `19. DEPLOYMENT_AND_OPERATIONS_PLAN.md`

-   [ ] **2. Moduna Göre Listeler (Mood Mixes) Mantığını Geliştir**
    -   [ ] `[Backend]` `HomeController` veya yeni bir `PersonalizationController` içinde, giriş yapmış bir kullanıcının Zevk Profili'ni analiz ederek en popüler "Akıllı Etiketlerini" (`smart_tags`) bulan bir mantık geliştir.
    -   [ ] `[Backend]` Bu etiketlere dayanarak, kullanıcıya özel dinamik film listeleri (örn: "Sana Özel Gerilim Filmleri") oluşturan bir fonksiyon yaz.
    -   [ ] `[Referans]` `12. PERSONALIZATION_ALGORITHMS.md`

-   [ ] **3. Topluluk Görüşü Özeti (Community Consensus) AI Görevini Oluştur**
    -   [ ] `[Backend]` `GemmaService`'e, `PROMPT_ENGINEERING_GUIDE.md`'deki `summarize_community_reviews` şablonunu kullanarak bir filme ait yorumları özetleyecek bir metod ekle.
    -   [ ] `[Backend]` Belirli bir yoruma ulaşan (örn: 20 yorum) filmler için bu özeti otomatik olarak oluşturup `movie_analyses` tablosuna kaydeden bir zamanlanmış görev veya trigger mekanizması kur.
    -   [ ] `[Referans]` `8. PROMPT_ENGINEERING_GUIDE.md`, `12. PERSONALIZATION_ALGORITHMS.md`

### **Milestone 3.3: Kişiselleştirme ve Oyunlaştırma Arayüzü (Frontend)**
*(**Amaç:** Geliştirilen akıllı özellikleri ve oyunlaştırma unsurlarını kullanıcıya sunmak.)*

-   [ ] **1. Oyunlaştırma Öğelerini Arayüze Ekle**
    -   [ ] `[Frontend]` Profil sayfasında (`views/profile/show.php`), kullanıcının toplam Sine-Puanını ve kazandığı tüm rozetleri sergileyen bir bölüm oluştur.
    -   [ ] `[Frontend]` `/leaderboards` URL'inde çalışacak ve en yüksek puanlı kullanıcıları listeleyecek `views/leaderboard/index.php` sayfasını oluştur.
    -   [ ] `[Referans]` `13. GAMIFICATION_SYSTEM.md`

-   [ ] **2. Kişiselleştirilmiş Ana Sayfayı Geliştir**
    -   [ ] `[Frontend]` Ana sayfayı (`views/home.php`), giriş yapmış kullanıcılar için "Haftalık Keşif Radarı" ve "Moduna Göre Listeler" bölümlerini gösterecek şekilde, dinamik bloklar halinde yeniden tasarla.
    -   [ ] `[Referans]` `12. PERSONALIZATION_ALGORITHMS.md`, `18. UI_UX_FLOW_AND_WIREFRAMES.md`

-   [ ] **3. Gelişmiş Topluluk Özelliklerini Tamamla**
    -   [ ] `[DB]` `comments` tablosuna `parent_comment_id` sütununu ekle ve `comment_likes` tablosunu oluştur.
    -   [ ] `[Backend/Frontend]` Yorumlara yanıt verme (iç içe yorumlar) ve yorumları beğenme işlevlerini tam olarak geliştir ve arayüze entegre et.
    -   [ ] `[Frontend]` Film Detay Sayfasına, `movie_analyses` tablosundan gelen "Topluluk Görüşü Özeti" metnini gösteren bir bölüm ekle.
    -   [ ] `[Referans]` `14. COMMUNITY_FEATURES_AND_MODERATION.md`

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

# TODO - Faz 5: Genişleme ve Gelecek Vizyonu

Bu doküman, `PROJECT_ROADMAP.md`'de tanımlanan **Faz 5 ve Sonrası**'nı hayata geçirmek için gereken stratejik ve teknik görevleri listeler. Bu faz, projenin yeni içerik türlerine ve platformlara açılarak pazarındaki etkisini artırmasını hedefler.

---

## **▶️ Faz 5: Genişleme ve Gelecek Vizyonu**
*(**Hedef:** Platformu, sinema dikeyinde lider bir konuma taşımak, hizmet kapsamını genişletmek ve kullanıcı deneyimini yeni platformlara taşımak.)*

### **Milestone 5.1: Dizi ve TV Şovları Entegrasyonu (Content Expansion)**
*(**Amaç:** Platformun en çok talep edilen özelliklerinden birini ekleyerek kullanıcı tabanını ve etkileşimi önemli ölçüde artırmak.)*

-   [ ] **1. Veritabanı Mimarisi Değişikliği**
    -   [ ] `[DB]` `movies` tablosunu daha genel bir yapıya dönüştürmek veya yeni tablolar eklemek için veritabanını yeniden tasarla. Önerilen yapı:
        -   `tv_shows` (`id`, `tmdb_id`, `title`, `overview`, `poster_path`, vb.)
        -   `tv_seasons` (`id`, `tv_show_id`, `season_number`, `air_date`, vb.)
        -   `tv_episodes` (`id`, `season_id`, `episode_number`, `title`, `overview`, vb.)
    -   [ ] `[DB]` `movie_interactions`, `comments` gibi etkileşim tablolarını, hem filmleri hem de dizileri/sezonları/bölümleri referans alabilecek şekilde (`content_type`, `content_id` sütunları ile) **polimorfik** bir yapıya dönüştür. Bu, büyük bir veritabanı refaktörüdür.

-   [ ] **2. Backend Entegrasyonu**
    -   [ ] `[Backend]` `TMDBService`'i, diziler (`/tv/{tv_id}`), sezonlar ve bölümler için TMDB API uç noktalarını destekleyecek şekilde genişlet.
    -   [ ] `[Backend]` Yeni `TVShowController`, `SeasonController` ve `EpisodeController` sınıflarını oluştur.
    -   [ ] `[Backend]` Mevcut tüm etkileşim servislerini (puanlama, yorumlama, listeleme vb.) yeni polimorfik veritabanı yapısıyla çalışacak şekilde refaktör et.

-   [ ] **3. Frontend Geliştirmesi**
    -   [ ] `[Frontend]` Dizi detay sayfaları için yeni bir arayüz tasarla (sezon ve bölüm listelerini içerecek şekilde).
    -   [ ] `[Frontend]` Sezon ve bölüm detay sayfaları için arayüzler oluştur.
    -   [ ] `[Frontend]` Ana arama fonksiyonunu, sonuçlarda hem filmleri hem de dizileri gösterecek şekilde güncelle.
    -   [ ] `[Frontend]` Profil ve liste sayfalarını, dizi etkileşimlerini de gösterecek şekilde güncelle.

### **Milestone 5.2: Gelişmiş Sosyal Özellikler (Social Deepening)**
*(**Amaç:** Kullanıcılar arası etkileşimi artırarak platformu daha "yapışkan" (sticky) hale getirmek.)*

-   [ ] **1. Takip Sistemi**
    -   [ ] `[DB]` Kullanıcılar arası takip ilişkisini saklamak için bir `followers` (`follower_id`, `following_id`) ara tablosu oluştur.
    -   [ ] `[Backend]` Kullanıcıların birbirini takip etmesini ve takipten çıkarmasını yönetecek bir `FollowController` oluştur.
    -   [ ] `[Frontend]` Kullanıcı profillerine "Takip Et" / "Takipten Çık" butonlarını ve takipçi/takip edilen sayılarını ekle.

-   [ ] **2. Kişisel Aktivite Akışı (Activity Feed)**
    -   [ ] `[DB]` Takip edilen kullanıcıların önemli eylemlerini (yeni bir filme 5 yıldız vermesi, yeni bir liste oluşturması vb.) saklamak için bir `activity_feed_items` tablosu tasarla.
    -   [ ] `[Backend]` Kullanıcı bir eylem gerçekleştirdiğinde, onu takip edenlerin akışına yeni bir öğe ekleyecek bir `ActivityFeedService` oluştur.
    -   [ ] `[Frontend]` Giriş yapmış kullanıcılar için, takip ettikleri kişilerin aktivitelerini gösteren, Facebook/X akışına benzer bir `/dashboard` veya `/feed` sayfası oluştur. Bu, giriş sonrası ana sayfa olabilir.

### **Milestone 5.3: Derinlemesine Yapay Zeka Entegrasyonları (AI Advancement)**
*(**Amaç:** Gemma 3'ün yeteneklerini, başka hiçbir platformda bulunmayan benzersiz ve "sihirli" özellikler sunmak için kullanmak.)*

-   [ ] **1. Film Karşılaştırma Asistanı**
    -   [ ] `[AI/Backend]` `GemmaService`'e, iki filmin verilerini alıp bu filmleri tematik, sinematografik ve hikaye anlatımı açısından karşılaştıran bir metin üretecek `compareMovies(movieA, movieB)` metodu ekle.
    -   [ ] `[Frontend]` Kullanıcıların iki film seçip karşılaştırma sonucunu görebileceği yeni bir `/compare` arayüzü oluştur.

-   [ ] **2. Gelişmiş Semantik Arama**
    -   [ ] `[AI/Backend]` Gemma 3'ün "function calling" veya "tool use" yeteneklerini araştır. Kullanıcının "Se7en gibi karanlık ama başrolü kadın olan bir film" gibi karmaşık, doğal dil sorgularını, sistemin anlayabileceği yapılandırılmış filtrelere (`genre: thriller`, `theme: dark`, `protagonist_gender: female`) dönüştürecek bir sistem tasarla.
    -   [ ] `[Frontend]` Ana arama çubuğunu veya özel bir "AI Asistanı" sayfasını, bu tür gelişmiş sorguları kabul edecek şekilde güncelle.

### **Milestone 5.4: Mobil Uygulama Altyapısı ve API Geliştirme (Mobile Groundwork)**
*(**Amaç:** Gelecekteki native mobil uygulamaları desteklemek için projenin mimarisini API odaklı bir yapıya dönüştürmek.)*

-   [ ] **1. Kapsamlı RESTful API Tasarımı**
    -   [ ] `[API]` Platformun tüm özelliklerini (kimlik doğrulama, film/dizi listeleme, etkileşimler, listeler vb.) dışa açacak bir RESTful API tasarla.
    -   [ ] `[API]` Tüm API uç noktalarını, istek/yanıt formatlarını ve veri modellerini **OpenAPI 3.0 (Swagger)** formatında belgele. Bu, hem mobil geliştiriciler için bir rehber olacak hem de API'yi test etmeyi kolaylaştıracaktır. *(Referans: Daha önceki hafıza notunuz)*
-   [ ] **2. Backend Refaktörü (API Odaklı)**
    -   [ ] `[Backend]` Mevcut Controller'ları, gelen isteğin türüne göre (`Accept` başlığı) ya bir HTML view render edecek ya da saf JSON yanıtı dönecek şekilde refaktör et. Bu, web ve mobil için tek bir kod tabanı kullanılmasını sağlar.
-   [ ] **3. Mobil Geliştirme Ön Hazırlığı**
    -   [ ] `[Mobile]` Hedef mobil platformlar için teknoloji yığınını araştır ve seç (React Native, Flutter, veya native Swift/Kotlin).
    -   [ ] `[Mobile]` Seçilen teknolojiye göre temel bir başlangıç projesi oluştur ve tasarlanan API'nin kimlik doğrulama (`login`) ucuna başarılı bir şekilde bağlanabildiğini test et.

# TODO - Faz 6: Native Mobil Uygulamalar (iOS & Android)

Bu doküman, `PROJECT_ROADMAP.md`'de tanımlanan **Faz 6**'yı hayata geçirmek için gereken stratejik ve teknik görevleri listeler. Bu faz, Sinefil Radarı deneyimini App Store ve Google Play'de yayınlanacak, yüksek performanslı ve platforma özgü native uygulamalara taşımayı hedefler.

---

## **▶️ Faz 6: Native Mobil Uygulamalar (iOS & Android)**
*(**Hedef:** Sinefil Radarı'nın web'deki temel ve en sevilen özelliklerini, iOS ve Android kullanıcıları için optimize edilmiş, hızlı ve sezgisel bir mobil deneyim olarak sunmak.)*

### **Milestone 6.1: Ön Hazırlık ve Strateji**
*(**Amaç:** Mobil geliştirme sürecine başlamadan önce teknoloji, tasarım ve ekip yapısını netleştirmek.)*

-   [ ] **1. Teknoloji Yığınını Sonlandırma**
    -   [ ] `[Strategy]` **Native (Swift/Kotlin) vs Cross-Platform (React Native/Flutter)** kararını kesinleştir.
        -   **Native (Swift/Kotlin):** En yüksek performans, en iyi platform entegrasyonu. İki ayrı kod tabanı ve ekip gerektirir.
        -   **Cross-Platform:** Tek kod tabanı, daha hızlı geliştirme. Performans ve platforma özgü özelliklerde potansiyel kısıtlamalar.
    -   [ ] `[DevOps]` Seçilen teknolojiye uygun olarak geliştirme ortamlarını (Xcode için macOS, Android Studio) ve bağımlılıkları (CocoaPods, Gradle) kur.

-   [ ] **2. Mobil API Katmanını Güçlendirme**
    -   [ ] `[Backend/API]` Faz 5'te tasarlanan **OpenAPI (Swagger)** dokümantasyonunu son haline getir. Tüm mobil uygulama özelliklerinin ihtiyaç duyacağı API uç noktalarının (`endpoints`) tanımlı ve test edilebilir olduğundan emin ol.
    -   [ ] `[Backend/API]` Mobil uygulamalar için daha verimli veri alışverişi sağlamak amacıyla API yanıtlarını optimize et (gereksiz verileri kaldırma, sayfalama - pagination).

-   [ ] **3. Mobil UI/UX Tasarımı**
    -   [ ] `[Design]` Web sitesindeki kullanıcı akışlarını mobile uyarlayan, dokunmatik ekranlar için optimize edilmiş **wireframe'ler ve prototipler** (Figma, Sketch vb. araçlarla) oluştur.
    -   [ ] `[Design]` Apple'ın **Human Interface Guidelines** ve Google'ın **Material Design** prensiplerine uygun, platforma özgü tasarım desenlerini (navigation drawers, tab bars, native alert'ler vb.) tanımla.
    -   [ ] `[Design]` Uygulama ikonunu, açılış ekranını (splash screen) ve App Store/Google Play için gerekli pazarlama görsellerini tasarla.

### **Milestone 6.2: Çekirdek Uygulama Geliştirme (MVP for Mobile)**
*(**Amaç:** Uygulamanın ilk yayınlanabilir versiyonu için en kritik özellikleri geliştirmek.)*

-   [ ] **1. Proje Yapısı ve Kimlik Doğrulama**
    -   [ ] `[Mobile/iOS]` Xcode'da yeni bir Swift projesi oluştur. Gerekli ağ (networking) ve JSON işleme kütüphanelerini (örn: Alamofire, SwiftyJSON) entegre et.
    -   [ ] `[Mobile/Android]` Android Studio'da yeni bir Kotlin projesi oluştur. Gerekli ağ ve JSON işleme kütüphanelerini (örn: Retrofit, Moshi/Gson) entegre et.
    -   [ ] `[Mobile/Both]` Supabase'in native mobil client kütüphanelerini (eğer mevcutsa) veya tasarlanan RESTful API'yi kullanarak **Kullanıcı Kaydı, Girişi ve Oturum Yönetimini** implemente et. Güvenli token depolama (Keychain/Keystore) kullanılmalı.

-   [ ] **2. Ana Ekranlar ve Gezinme (Navigation)**
    -   [ ] `[Mobile/Both]` Uygulama için ana gezinme yapısını kur (örn: iOS'ta Tab Bar, Android'de Bottom Navigation Bar).
    -   [ ] `[Mobile/Both]` **Ana Sayfa:** Popüler filmleri ve dizileri listeleyen ekranı geliştir. Sonsuz kaydırma (infinite scrolling) ile daha fazla içerik yükleme özelliği ekle.
    -   [ ] `[Mobile/Both]` **Arama Ekranı:** Kullanıcıların film/dizi araması yapmasını ve sonuçları görmesini sağlayan ekranı geliştir.
    -   [ ] `[Mobile/Both]` **Film/Dizi Detay Ekranı:** API'den gelen tüm verileri (afiş, özet, oyuncular, fragman oynatıcı, yorumlar) gösteren detay ekranını geliştir.

-   [ ] **3. Çekirdek Etkileşimler**
    -   [ ] `[Mobile/Both]` Film/Dizi Detay ekranından **Puanlama, Yorum Yazma, Favorilere Ekleme ve Listelere Ekleme** işlevlerini, API'ye istek göndererek çalışacak şekilde implemente et.
    -   [ ] `[Mobile/Both]` **Profil Ekranı:** Kullanıcının temel bilgilerini, listelerini ve son aktivitelerini gösteren sekmeli profil sayfasını geliştir.

### **Milestone 6.3: Gelişmiş Özellikler ve Yayınlama**
*(**Amaç:** Uygulamayı zenginleştirmek, test etmek ve marketlerde yayınlamak.)*

-   [ ] **1. Kişiselleştirme ve Bildirimler**
    -   [ ] `[Mobile/Both]` Giriş yapmış kullanıcılar için ana sayfada **"Haftalık Keşif Radarı"** gibi kişiselleştirilmiş bölümleri göster.
    -   [ ] `[Backend]` Firebase Cloud Messaging (FCM) veya Apple Push Notification Service (APNS) ile entegrasyon kur.
    -   [ ] `[Mobile/Both]` Kullanıcıya özel bildirimleri (örn: "Takip ettiğiniz bir kullanıcı yeni bir liste oluşturdu") alıp gösterecek altyapıyı (Push Notifications) kur.

-   [ ] **2. Test ve Optimizasyon**
    -   [ ] `[QA]` Uygulamanın farklı cihaz boyutları ve işletim sistemi versiyonlarında (iOS 16+, Android 10+) doğru çalıştığını test et.
    -   [ ] `[QA]` API hataları, internet bağlantısı kopması gibi durumlar için hata yönetimi ve kullanıcıya gösterilecek mesajları test et.
    -   [ ] `[Performance]` Uygulama başlangıç süresini, ağ isteklerini ve bellek kullanımını optimize et.

-   [ ] **3. Yayınlama Süreci (Deployment)**
    -   [ ] `[Legal]` Uygulama için bir Gizlilik Politikası ve Kullanım Koşulları metni hazırla.
    -   [ ] `[DevOps]` Apple Developer ve Google Play Console hesaplarını oluştur ve yapılandır.
    -   [ ] `[DevOps]` Uygulamanın marketlere gönderilecek sürümlerini (build) oluştur, imzala ve tüm gerekli bilgileri (ekran görüntüleri, açıklamalar, ikonlar) yükle.
    -   [ ] `[DevOps]` Uygulamaları App Store ve Google Play'e inceleme (review) için gönder.
    -   [ ] `[Marketing]` Uygulamalar onaylandıktan sonra, web sitesinde ve sosyal medyada lansmanı duyur.
