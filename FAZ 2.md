# TODO - Faz 2: Topluluk ve Gelişmiş Keşif

Bu doküman, `PROJECT_ROADMAP.md`'de tanımlanan **Faz 2**'yi hayata geçirmek için gereken tüm teknik görevleri detaylı ve sıralı bir şekilde listeler.

---

## **▶️ Faz 2: Topluluk ve Gelişmiş Keşif**
*(**Hedef:** Kullanıcıyı pasif bir tüketiciden, platforma aktif olarak katkıda bulunan bir üyeye dönüştürmek ve film keşfetme yollarını zenginleştirmek.)*

### **Milestone 2.1: Etkileşim Veritabanı ve Backend**
*(**Amaç:** Kullanıcıların filmlerle etkileşime (puan, favori, yorum) girmesini sağlayacak altyapıyı kurmak.)*

-   [ ] **1. Veritabanı Şemasını Genişlet**
    -   [ ] `[DB]` Supabase'de, `DATABASE_SCHEMA.md` dokümanını referans alarak aşağıdaki tabloları oluştur:
        -   `movie_interactions` (`id`, `user_id`, `movie_id`, `rating`, `is_favorite`, `is_liked`, `created_at`, `updated_at`)
        -   `comments` (`id`, `user_id`, `movie_id`, `content`, `has_spoiler`, `created_at`, `updated_at`)
    -   [ ] `[Referans]` `5. DATABASE_SCHEMA.md`, `10. CORE_FILM_FEATURES_SPEC.md`

-   [ ] **2. Erişim Kontrol Politikalarını (RLS) Tanımla**
    -   [ ] `[DB]` Supabase SQL editöründe, `SECURITY_AND_ACCESS_CONTROL.md` dokümanını referans alarak `movie_interactions` ve `comments` tabloları için Row Level Security politikalarını oluştur.
    -   [ ] `[Referans]` `6. SECURITY_AND_ACCESS_CONTROL.md`

-   [ ] **3. Backend Controller'larını Oluştur**
    -   [ ] `[Backend]` `app/Controllers/InteractionController.php` sınıfını oluştur. İçerisinde, `movie_interactions` tablosu üzerinde `upsert` mantığıyla çalışacak `update()` adında bir metod oluştur.
    -   [ ] `[Backend]` `app/Controllers/CommentController.php` sınıfını oluştur. İçerisinde, `store()` (yeni yorum ekleme), `update()` (yorumu düzenleme) ve `destroy()` (yorumu silme) metodlarını implemente et.
    -   [ ] `[Referans]` `10. CORE_FILM_FEATURES_SPEC.md`

### **Milestone 2.2: Etkileşim Arayüzü (Frontend)**
*(**Amaç:** Kullanıcıların film detay sayfasında etkileşimde bulunabilmesini sağlamak.)*

-   [ ] **1. Film Detay Sayfasını Güncelle**
    -   [ ] `[Frontend]` `views/movie/detail.php` sayfasına, interaktif bir 5-yıldız puanlama bileşeni ekle.
    -   [ ] `[Frontend]` Aynı sayfaya, "Favorilere Ekle" (Kalp ikonu) ve "Beğen" (Başparmak ikonu) butonlarını ekle.
    -   [ ] `[Frontend]` Yorumları listeleyen bir bölüm ve yeni yorum yazmak için bir form ekle. Formda "Spoiler İçerir" onay kutusu bulunmalı.
    -   [ ] `[Referans]` `18. UI_UX_FLOW_AND_WIREFRAMES.md`

-   [ ] **2. Asenkron Etkileşimleri Geliştir (JavaScript)**
    -   [ ] `[Frontend]` Puanlama, favorileme ve beğenme butonlarına tıklandığında, sayfayı yenilemeden backend'e AJAX isteği gönderen bir JavaScript kodu yaz.
    -   [ ] `[Frontend]` Yorum gönderme formunu, AJAX ile çalışacak şekilde geliştir. Başarılı yanıta göre, yeni yorumu sayfa yenilenmeden yorum listesine ekle.
    -   [ ] `[Referans]` `10. CORE_FILM_FEATURES_SPEC.md`

### **Milestone 2.3: Liste Yönetim Sistemi**
*(**Amaç:** Kullanıcıların kendi film koleksiyonlarını oluşturup yönetebilmesini sağlamak.)*

-   [ ] **1. Liste Yönetimi Veritabanı ve Backend**
    -   [ ] `[DB]` `DATABASE_SCHEMA.md`'ye göre `lists` ve `list_items` tablolarını ve ilgili RLS politikalarını oluştur.
    -   [ ] `[Backend]` `app/Controllers/ListController.php` sınıfını oluştur (liste oluşturma, düzenleme, silme, film ekleme/çıkarma).
    -   [ ] `[Referans]` `5. DATABASE_SCHEMA.md`, `11. LIST_MANAGEMENT_SYSTEM_SPEC.md`

-   [ ] **2. Liste Yönetimi Arayüzü**
    -   [ ] `[Frontend]` Profil sayfasında "Yeni Liste Oluştur" formunu ve kullanıcının mevcut listelerini gösteren bir bölüm oluştur.
    -   [ ] `[Frontend]` Film Detay Sayfasındaki "Listeye Ekle" butonunu, tıklandığında kullanıcının listelerini gösteren bir modal açacak şekilde geliştir.
    -   [ ] `[Frontend]` Herkese açık listeler için SEO dostu URL'lere (`/profile/{username}/list/{slug}`) sahip, `views/list/show.php` adında bir detay sayfası oluştur.
    -   [ ] `[Referans]` `11. LIST_MANAGEMENT_SYSTEM_SPEC.md`, `18. UI_UX_FLOW_AND_WIREFRAMES.md`

### **Milestone 2.4: Profil Geliştirmeleri**
*(**Amaç:** Kullanıcı profillerini, yaptıkları katkıları sergileyen zengin sayfalara dönüştürmek.)*

-   [ ] **1. Profil Sayfasını Zenginleştir**
    -   [ ] `[Backend]` `ProfileController`'ı, kullanıcının son yorumlarını ve herkese açık listelerini de çekecek şekilde güncelle.
    -   [ ] `[Frontend]` `views/profile/show.php` sayfasını, gelen bu yeni verileri (Yorumlar, Listeler) sekmeli bir yapıda gösterecek şekilde yeniden tasarla.
    -   [ ] `[Referans]` `18. UI_UX_FLOW_AND_WIREFRAMES.md`

-   [ ] **2. Profil Düzenleme ve Avatar Yükleme**
    -   [ ] `[Backend]` Profil bilgilerini güncelleyecek ve Supabase Storage'a avatar yükleyip URL'ini `users` tablosuna kaydedecek mantığı `ProfileController`'a ekle.
    -   [ ] `[Frontend]` `/settings/profile` URL'inde çalışacak, kullanıcıların profil bilgilerini düzenleyebileceği ve yeni bir profil resmi yükleyebileceği bir form (`views/profile/edit.php`) oluştur.
    -   [ ] `[Referans]` `9. USER_AUTHENTICATION_AND_PROFILES.md`

### **Milestone 2.5: Gelişmiş Keşif Özellikleri (Advanced Discovery)**
*(**Amaç:** Kullanıcıların film keşfetme yollarını zenginleştirmek ve arama deneyimini daha interaktif hale getirmek.)*

-   [ ] **1. Kategori (Tür) Sayfalarını Oluştur**
    -   [ ] `[Backend]` `app/Controllers/GenreController.php` adında yeni bir controller oluştur.
    -   [ ] `[Backend]` Bu controller içinde, belirli bir türe ait olan filmleri veritabanından çeken ve sayfalandıran (`pagination`) bir `show()` metodu oluştur.
    -   [ ] `[Frontend]` `/genre/{slug}` URL'inde çalışacak, gelen film listesini bir grid yapısında gösteren `views/genre/show.php` adında bir view dosyası oluştur.
    -   [ ] `[Frontend]` Ana sayfaya ve film detay sayfalarındaki tür etiketlerine, bu yeni kategori sayfalarına giden linkleri ekle.
    -   [ ] `[Referans]` `18. UI_UX_FLOW_AND_WIREFRAMES.md`, `5. DATABASE_SCHEMA.md`

-   [ ] **2. Aramada Fragman Önizlemesi Özelliğini Geliştir**
    -   [ ] `[Frontend/JS]` Arama sonuçları sayfasında, listelenen her film için arka planda TMDB'nin `/movie/{movie_id}/videos` uç noktasına asenkron bir AJAX isteği atarak fragman anahtarını (`key`) alan bir fonksiyon yaz.
    -   [ ] `[Frontend/JS]` Kullanıcı bir film kartının üzerine fareyle geldiğinde (`mouseenter`), ilgili filmin fragmanını sessiz ve otomatik oynatan bir `iframe`'i dinamik olarak oluşturan mantığı implemente et.
    -   [ ] `[Referans]` `17. THIRD_PARTY_API_CONTRACTS.md`, `18. UI_UX_FLOW_AND_WIREFRAMES.md`
