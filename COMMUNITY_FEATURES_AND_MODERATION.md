# Community Features and Moderation Specification (Topluluk Özellikleri ve Denetleme Spesifikasyonu)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Felsefe ve Amaç

Bu doküman, Sinefil Radarı'nda derinlemesine tartışma ve etkileşimi teşvik edecek topluluk özelliklerini ve bu etkileşimlerin saygılı, yapıcı ve güvenli bir ortamda kalmasını sağlayacak denetleme (moderasyon) stratejilerini tanımlar. Amacımız, kullanıcıların sadece filmler hakkında değil, aynı zamanda birbirleriyle de anlamlı bağlar kurabildiği, toksik davranışlardan arındırılmış bir "dijital sinema kulübü" yaratmaktır.

## 2. Topluluk Özellikleri

### 2.1. Gelişmiş Yorum Sistemi

-   **Özellik: Yorumlara Yanıt Verme (Reply to Comments)**
    -   **Açıklama:** Kullanıcılar, başka bir kullanıcının yorumuna doğrudan yanıt verebilmelidir. Yanıtlar, ana yorumun altında iç içe (nested) bir yapıda gösterilerek diyalog akışının takip edilmesini kolaylaştırmalıdır.
    -   **Teknik Uygulama:** `comments` tablosuna `parent_comment_id` adında, yine aynı tabloya referans veren (self-referencing) bir `uuid` sütunu eklenecektir. Bu sütun, bir yorumun hangi yoruma yanıt olarak yazıldığını belirtir.

-   **Özellik: Yorumları Beğenme (Like Comments)**
    -   **Açıklama:** Kullanıcılar, faydalı, zekice veya katıldıkları yorumları bir "beğen" butonu ile işaretleyebilmelidir. Bu, kaliteli yorumların öne çıkmasına yardımcı olur.
    -   **Teknik Uygulama:** `comment_likes` adında yeni bir ara tablo (`user_id`, `comment_id`) oluşturulacaktır. Film detay sayfasında yorumlar, en çok beğeni alanlara göre sıralanabilir bir seçenek sunmalıdır.

### 2.2. Tartışma Kulüpleri (Discussion Clubs)

-   **Amaç:** Genel film yorumlarının ötesinde, belirli temalar, yönetmenler veya film serileri etrafında odaklanmış, kalıcı tartışma alanları oluşturmak.
-   **Arayüz:** Platformda `/clubs` veya `/discussions` adında özel bir bölüm bulunacaktır.
-   **Örnek Kulüpler/Forumlar:**
    -   `The Nolanverse`: Christopher Nolan'ın filmleri üzerine teoriler ve analizler.
    -   `Horror Corner`: Haftanın korku filmi önerileri ve tartışmaları.
    -   `Movie Theories`: Filmlerin sonları veya gizli anlamları üzerine teorilerin paylaşıldığı genel bir alan.
-   **Teknik Uygulama:**
    -   `forums` (`id`, `title`, `description`, `slug`) adında yeni bir tablo oluşturulacaktır.
    -   `threads` (`id`, `forum_id`, `user_id`, `title`, `created_at`) adında, her bir tartışma başlığını temsil eden bir tablo oluşturulacaktır.
    -   `posts` (`id`, `thread_id`, `user_id`, `content`, `created_at`) adında, bir başlığa yazılan her bir mesajı/yanıtı içeren bir tablo oluşturulacaktır.

## 3. İçerik Denetleme (Moderation) Stratejisi

Sağlıklı bir topluluk, etkili ve adil bir denetleme sistemi gerektirir. Stratejimiz, reaktif (şikayet bazlı) ve proaktif (otomatik) sistemlerin bir kombinasyonuna dayanacaktır.

### 3.1. Kullanıcı Tabanlı Raporlama (User-Based Reporting)

-   **Özellik: Raporla Butonu**
    -   **Açıklama:** Her kullanıcı tarafından oluşturulmuş içeriğin (yorumlar, liste adları/açıklamaları, forum gönderileri, kullanıcı profilleri) yanında bir "Raporla" butonu bulunmalıdır.
    -   **Akış:**
        1.  Kullanıcı "Raporla" butonuna tıklar.
        2.  Bir modal açılarak raporlama nedeni sorulur (örn: "Spam", "Nefret Söylemi", "Taciz", "Spoiler").
        3.  Gönderilen rapor, admin panelinde incelenmek üzere bir "Rapor Kuyruğu"na düşer.
    -   **Teknik Uygulama:** `reports` (`id`, `reporter_user_id`, `reported_content_id`, `content_type`, `reason`, `status`) adında yeni bir tablo oluşturulacaktır.

### 3.2. Otomatik Filtreleme (Proactive Filtering)

-   **Özellik: Kelime Filtresi (Word Filter)**
    -   **Açıklama:** Küfür, hakaret ve nefret söylemi içeren kelimelerin bir listesi (blacklist) tutulacaktır.
    -   **Uygulama:** Kullanıcı bir yorum veya metin gönderdiğinde, backend bu metni kara listedeki kelimelere karşı kontrol eder.
    -   **Aksiyon:** Eşleşme bulunursa, kelime `****` gibi karakterlerle sansürlenir veya gönderi otomatik olarak moderasyon onayına düşürülür.

### 3.3. Admin Paneli Moderasyon Araçları

Admin paneli, moderatörlerin verimli bir şekilde çalışabilmesi için aşağıdaki araçları içermelidir:

-   **Rapor Kuyruğu:** Tüm kullanıcı raporlarının listelendiği, rapora konu olan içeriğe doğrudan link veren ve "Onayla", "Reddet", "Kullanıcıyı Uyar" gibi hızlı aksiyon butonları içeren bir arayüz.
-   **Kullanıcı Yönetimi:**
    -   **Uyarı Gönderme:** Bir kullanıcıya kural ihlali hakkında özel bir uyarı mesajı gönderme.
    -   **Geçici Olarak Susturma (Mute):** Bir kullanıcının belirli bir süre (örn: 3 gün) boyunca yorum yapmasını veya içerik oluşturmasını engelleme.
    -   **Kalıcı Olarak Yasaklama (Ban):** Tekrarlanan veya ciddi kural ihlallerinde kullanıcının hesabını kalıcı olarak askıya alma.
-   **İçerik Düzenleme/Silme:** Moderatörler, platformdaki herhangi bir kullanıcı tarafından oluşturulmuş içeriği düzenleme veya silme yetkisine sahip olmalıdır. Yapılan her moderatör eylemi, şeffaflık için bir log kaydı olarak tutulmalıdır.

### 3.4. Topluluk Kuralları (Community Guidelines)

-   Sitede, herkesin kolayca erişebileceği, ne tür davranışların kabul edilebilir olduğunu ve nelerin kural ihlali sayılacağını açıkça belirten bir `/guidelines` sayfası bulunmalıdır. Bu sayfa, raporlama ve denetleme kararları için bir referans noktası olacaktır.

Bu kapsamlı topluluk ve denetleme yapısı, Sinefil Radarı'nın sadece bir film sitesi değil, aynı zamanda sinemaseverler için güvenli ve davetkar bir buluşma noktası olmasını sağlayacaktır.
