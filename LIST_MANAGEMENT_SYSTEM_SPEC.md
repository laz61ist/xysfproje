# List Management System Specification (Liste Yönetim Sistemi Spesifikasyonu)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Genel Bakış

Liste Yönetim Sistemi, Sinefil Radarı'nın en önemli topluluk ve içerik üretme özelliklerinden biridir. Bu sistem, kullanıcılara filmleri kişisel zevklerine göre gruplandırma, kürasyon yapma ve bu koleksiyonları ister özel olarak saklama isterlerse de tüm dünya ile paylaşma imkanı sunar. Bu özellik, kullanıcı etkileşimini artırmanın, platformda geçirilen süreyi uzatmanın ve arama motorları için değerli, özgün içerik (User-Generated Content) oluşturmanın temelini oluşturur.

## 2. Veri Modeli Referansı

Bu sistemin işleyişi, `DATABASE_SCHEMA.md` dosyasında tanımlanan aşağıdaki iki ana tabloya dayanmaktadır:
-   `lists`: Her listenin ana bilgilerini (adı, sahibi, gizlilik durumu vb.) saklar.
-   `list_items`: Hangi filmin hangi listeye ait olduğunu ve listedeki sırasını belirten ara tablodur.

Tüm erişim kontrolleri, `SECURITY_AND_ACCESS_CONTROL.md` dosyasında belirtilen RLS (Row Level Security) politikaları ile veritabanı seviyesinde garanti altına alınır.

## 3. Temel Fonksiyonlar

### 3.1. Liste Oluşturma (Create List)
-   **Arayüz:** Kullanıcının profil sayfasında veya gezinti menüsünde bulunan "Yeni Liste Oluştur" butonu ile erişilen bir form.
-   **Form Alanları:**
    -   `name` (Liste Adı): Zorunlu, metin alanı.
    -   `description` (Açıklama): İsteğe bağlı, daha büyük bir metin alanı (textarea).
    -   `is_public` (Herkese Açık mı?): Varsayılan olarak `true` (işaretli) olan bir onay kutusu (checkbox). İşareti kaldırıldığında liste özel olur.
-   **Akış:** Kullanıcı formu doldurup kaydettiğinde, `lists` tablosuna yeni bir kayıt eklenir.

### 3.2. Listeye Film Ekleme/Çıkarma (Add/Remove Movies)
-   **Arayüz:** Her film detay sayfasında bir "Listeye Ekle" (+ ikonu) butonu bulunur.
-   **Akış:**
    1.  Kullanıcı "Listeye Ekle" butonuna tıklar.
    2.  Kullanıcının mevcut tüm listelerini gösteren bir modal (popup) açılır.
    3.  Kullanıcı filmi eklemek istediği listenin/listelerin yanındaki onay kutusunu işaretler. Bu modal içinde "Yeni Liste Oluştur" seçeneği de bulunabilir.
    4.  Seçim yapıldığında, `list_items` tablosuna ilgili `list_id` ve `movie_id` ile yeni bir kayıt eklenir. Bu işlem sayfa yenilenmeden, asenkron olarak gerçekleşir.
    5.  Bir filmi listeden çıkarmak için, liste detay sayfasında filmin yanındaki "Kaldır" (X ikonu) butonuna basılır ve ilgili kayıt `list_items` tablosundan silinir.

### 3.3. Liste Detaylarını Düzenleme (Edit List)
-   Kullanıcılar **sadece kendi** listelerinin detay sayfasında "Düzenle" butonunu görürler.
-   Bu buton, liste oluşturma formuna benzer bir arayüz açarak `name`, `description` ve `is_public` alanlarının güncellenmesine olanak tanır.

### 3.4. Liste İçeriğini Sıralama (Reorder List)
-   Liste detay sayfasında, liste sahibi olan kullanıcı için filmleri sürükle-bırak (drag-and-drop) ile yeniden sıralama özelliği sunulmalıdır.
-   Bir film yeniden sıralandığında, `list_items` tablosundaki `order` sütunu, listenin yeni sırasını yansıtacak şekilde güncellenir.

### 3.5. Liste Silme (Delete List)
-   Kullanıcılar **sadece kendi** listelerini silebilir.
-   Silme işlemi öncesinde, "Bu listeyi silmek istediğinizden emin misiniz? Bu işlem geri alınamaz." şeklinde bir onay kutusu gösterilmelidir.
-   Onaylandığında, `lists` tablosundan ilgili kayıt silinir. Veritabanı ilişkisindeki `ON DELETE CASCADE` kuralı sayesinde, bu listeye ait tüm kayıtlar `list_items` tablosundan da otomatik olarak silinir.

## 4. Paylaşım, Görünürlük ve SEO Stratejisi

### 4.1. URL Yapısı
-   Tüm **Herkese Açık (Public)** listeler, SEO dostu, temiz ve paylaşılabilir bir URL yapısına sahip olmalıdır.
-   **URL Formatı:** `https://www.sinefilradari.com/profile/{username}/list/{list-slug}`
    -   `{list-slug}`: Liste adından otomatik olarak oluşturulan, `en-iyi-bilim-kurgu-filmleri` gibi bir yapıdır.

### 4.2. SEO (Arama Motoru Optimizasyonu)
-   **Dinamik Meta Etiketleri:** Her public liste sayfası, arama motorları için dinamik olarak oluşturulmuş meta etiketlerine sahip olmalıdır:
    -   **`<title>`:** `Liste Adı - {username} tarafından oluşturulan bir Sinefil Radarı listesi`
    -   **`<meta name="description">`:** Listenin `description` alanındaki metin. Eğer boşsa, listedeki ilk birkaç filmin adından oluşan bir özet.
-   **Yapılandırılmış Veri (Structured Data):** Google'ın içeriği daha iyi anlaması için her public liste sayfasına **Schema.org `ItemList`** mikrodatası eklenmelidir. Bu, listedeki her filmi (`itemListElement`) ve listenin kendisini tanımlar.
-   **İndekslenebilirlik:** Özel (`Private`) listelerin sayfaları, arama motorları tarafından indekslenmemesi için `noindex` meta etiketini içermelidir.

## 5. Monetizasyon Stratejisi (Google Ads)

-   **Reklam Alanları:** Kullanıcı tarafından oluşturulan liste sayfaları, yüksek etkileşim alan ve özgün içerik barındıran sayfalardır. Bu sayfalar, reklam yerleşimi için idealdir.
    -   **Potansiyel Yerleşimler:**
        1.  Listenin başlığının altında/üstünde bir adet banner reklam (Leaderboard).
        2.  Listedeki her 5 veya 10 filmden sonra bir adet "in-feed" reklam.
-   **İçerik Politikaları:** Google Ads politikalarına uyum sağlamak için, liste adları ve açıklamalarında uygunsuz veya yasa dışı içerik kullanımını engellemek amacıyla bir **içerik denetleme (moderation)** mekanizması (örn: küfür filtresi, raporlama butonu) düşünülmelidir.
-   **Kullanıcı Deneyimi:** "Destekçi" statüsündeki kullanıcılar için bu sayfalardaki reklamlar otomatik olarak gizlenmelidir.
