# Core Film Features Specification (Temel Film Özellikleri Spesifikasyonu)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Genel Bakış

Bu doküman, kullanıcıların Sinefil Radarı platformunda filmlerle etkileşime geçmesini sağlayan temel özellikleri detaylandırmaktadır. Bu özellikler; Puanlama (Rating), Yorumlama (Commenting), Favorilere Ekleme (Favoriting), Beğenme (Liking) ve Paylaşma (Sharing)'dır. Bu etkileşimler, hem topluluk katılımını artırmanın hem de kişiselleştirme algoritmalarımız için değerli veri toplamanın temelini oluşturur.

## 2. Veri Modeli Referansı

Bu özelliklerin tamamı, `DATABASE_SCHEMA.md` dosyasında tanımlanan aşağıdaki iki ana tablo etrafında şekillenir:

-   `movie_interactions`: Bir kullanıcının bir filmle olan **tüm** durum-bazlı etkileşimlerini (puan, favori, beğeni) tek bir satırda tutar. Bu, "UPSERT" (UPDATE or INSERT) mantığı ile çalışarak veri tekrarını önler ve sorguları basitleştirir.
-   `comments`: Kullanıcıların metin tabanlı yorumlarını saklar.

## 3. Özelliklerin Detayları

### 3.1. Puanlama (Rating)

-   **Arayüz:** Her film detay sayfasında, 1'den 5'e kadar seçilebilen interaktif bir yıldız sistemi bulunur.
-   **Kullanıcı Akışı:**
    1.  Giriş yapmış bir kullanıcı, fareyi yıldızların üzerine getirerek puanını seçer ve tıklar.
    2.  Kullanıcının daha önce verdiği bir puan varsa, yıldızlar o puanı dolu olarak gösterir. Tekrar tıklayarak puanını değiştirebilir.
    3.  Puan silme seçeneği (örneğin, aktif puana tekrar tıklayarak) sunulmalıdır.
-   **Teknik Akış (Frontend):**
    1.  Kullanıcı bir yıldıza tıkladığında, istemci tarafı JavaScript, o anki kullanıcı ID'si ve film ID'si ile birlikte seçilen puanı (örn: `4`) içeren bir isteği backend API'mize (`/api/interaction/rate`) gönderir.
-   **Teknik Akış (Backend):**
    1.  Backend, isteği alır ve kullanıcının kimliğini doğrular.
    2.  `movie_interactions` tablosunda bu `user_id` ve `movie_id` kombinasyonu için bir satır olup olmadığını kontrol eder.
        -   **Varsa (UPDATE):** Mevcut satırın `rating` sütununu yeni puanla günceller.
        -   **Yoksa (INSERT):** Yeni bir satır oluşturur ve `rating` sütununu gelen puanla doldurur.
    3.  Bu işlem için Supabase'in `upsert()` fonksiyonu idealdir.
    4.  Başarılı işlem sonrası, filmin yeni ortalama puanı yeniden hesaplanıp önbelleğe alınabilir.

### 3.2. Yorumlama (Commenting)

-   **Arayüz:** Film detay sayfasında, yorumları listeleyen bir bölüm ve yeni yorum yazmak için bir metin kutusu bulunur. Metin kutusunun yanında bir "Spoiler İçerir" onay kutusu (checkbox) yer alır.
-   **Kullanıcı Akışı:**
    1.  Giriş yapmış kullanıcı yorumunu yazar.
    2.  Eğer yorumu filmin sürprizlerini açığa vuruyorsa, "Spoiler İçerir" kutusunu işaretler.
    3.  "Gönder" butonuna tıklar.
    4.  Yazdığı yorum, sayfa yenilenmeden listenin en üstünde görünür.
-   **Teknik Akış (Backend):**
    1.  Backend, `comments` tablosuna `user_id`, `movie_id`, yorum metni (`content`) ve `has_spoiler` durumu ile yeni bir kayıt ekler.
    2.  RLS politikaları, sadece giriş yapmış kullanıcıların bu tabloya `INSERT` yapabilmesini garanti eder.
-   **Gösterim Mantığı:**
    -   `has_spoiler` değeri `true` olan yorumlar, içeriği gizlenmiş (bulanık veya "Spoiler'ı Göster" butonu ile) bir şekilde gösterilir.

### 3.3. Favorilere Ekleme (Favoriting) & Beğenme (Liking)

Bu iki özellik, kullanıcı arayüzünde farklı ikonlarla (örn: Kalp ❤️ Favori için, Başparmak 👍 Beğeni için) temsil edilse de, teknik altyapıları neredeyse aynıdır.

-   **Arayüz:** Her film sayfasında tıklanabilir bir "Favorilere Ekle" ve "Beğen" butonu bulunur. Butonlar, kullanıcının mevcut durumuna göre aktif veya pasif görünür.
-   **Kullanıcı Akışı:**
    1.  Kullanıcı "Favorilere Ekle" butonuna tıklar. Butonun ikonu dolar ve aktif hale gelir.
    2.  Tekrar tıkladığında, favorilerden çıkarılır ve ikon eski haline döner.
    3.  Aynı mantık "Beğen" butonu için de geçerlidir.
-   **Teknik Akış:**
    1.  Kullanıcı butona tıkladığında, frontend backend'e bir istek gönderir.
    2.  Backend, Puanlama özelliğindeki gibi `upsert()` mantığını kullanarak `movie_interactions` tablosunda ilgili kullanıcı/film satırını bulur.
    3.  Mevcut `is_favorite` (veya `is_liked`) boolean değerini tersine çevirir (`true` ise `false`, `false` ise `true` yapar).
-   **Önemi:**
    -   **Favori:** Kullanıcının en çok sevdiği filmleri belirtir ve kişisel profilinde özel bir listede gösterilir. "Haftalık Keşif" algoritması için çok güçlü bir sinyaldir.
    -   **Beğeni:** Daha düşük bir bağlılık seviyesini temsil eder. "Beğendiğiniz filmlere benzer" gibi anlık öneriler için kullanılabilir.

### 3.4. Paylaşma (Sharing)

-   **Arayüz:** Her film detay sayfasında, "Paylaş" ikonu bulunur.
-   **Kullanıcı Akışı:**
    1.  Kullanıcı "Paylaş" ikonuna tıklar.
    2.  Bir modal veya popup açılarak aşağıdaki seçenekler sunulur:
        -   **URL'i Kopyala:** Filmin sayfa linkini panoya kopyalar.
        -   **Sosyal Medya Butonları:** X (Twitter), Facebook, WhatsApp gibi platformlarda paylaşmak için hazır linkler sunar.
-   **Teknik Akış (Frontend):**
    -   Bu özellik tamamen istemci tarafında çalışır.
    -   Paylaşım linkleri oluşturulurken, sosyal medya platformlarının standart paylaşım URL formatları kullanılır. Örneğin, X için: `https://twitter.com/intent/tweet?url=FILM_URL&text=FILM_ADI`.
    -   URL oluşturulurken `ENVIRONMENT_CONFIG_AND_URL_STRATEGY.md`'de tanımlanan `baseUrl()` yardımcı fonksiyonu kullanılır.
-   **Open Graph Meta Etiketleri:** Paylaşıldığında sosyal medyada zengin bir önizleme (resim, başlık, açıklama) görünmesi için, tüm film detay sayfalarının `<head>` bölümünde Open Graph (OG) meta etiketleri dinamik olarak oluşturulmalıdır.
    ```html
    <meta property="og:title" content="Film Adı - Sinefil Radarı" />
    <meta property="og:description" content="Filmin kısa özeti..." />
    <meta property="og:image" content="FILM_AFIS_URL" />
    <meta property="og:url" content="FILM_SAYFA_URL" />
    <meta property="og:type" content="video.movie" />
    ```

Bu temel özellikler, kullanıcıların platformla anlamlı bir bağ kurmasını sağlayacak ve Sinefil Radarı'nı statik bir bilgi sitesinden dinamik bir topluluk merkezine dönüştürecektir.
