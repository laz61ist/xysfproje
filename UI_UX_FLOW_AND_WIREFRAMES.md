# UI/UX Flow and Wireframes Specification (Kullanıcı Arayüzü/Deneyimi Akışı ve Taslakları Spesifikasyonu)

**Document Version:** 1.1
**Last Updated:** 18.08.2025

## 1. Tasarım Felsefesi ve Prensipleri

Sinefil Radarı'nın kullanıcı arayüzü ve deneyimi, aşağıdaki temel prensipler üzerine inşa edilecektir:

-   **İçerik Odaklı (Content-First):** Tasarım, filmlerin afişlerini, kullanıcıların yorumlarını ve yapay zekanın analizlerini öne çıkararak dikkat dağıtıcı unsurları en aza indirmelidir.
-   **Sezgisel ve Zahmetsiz (Intuitive & Effortless):** Kullanıcılar, aradıkları bilgiye veya yapmak istedikleri eyleme en az tıklama ile ulaşabilmelidir.
-   **Mobil Öncelikli (Mobile-First):** Tüm sayfa düzenleri, öncelikle mobil cihazlar için tasarlanacak, ardından tablet ve masaüstü ekranlarına uyarlanacaktır (Responsive Design).
-   **Erişilebilirlik (Accessibility):** Renk kontrastları, yazı tipi boyutları ve interaktif elemanlar, WCAG (Web Content Accessibility Guidelines) standartlarına uygun olarak tasarlanmalıdır.

## 2. Temel Kullanıcı Akışları (Core User Flows)

### Akış 1: Yeni Kullanıcının Film Keşfi ve İzlemeye Yönlenmesi
1.  **Ana Sayfa:** Kullanıcı, "Ruh halini yaz, filmini bul..." gibi davetkar bir metinle karşılayan büyük, doğal dil arama çubuğunu görür. Altında "Popüler Listeler", **"Türlere Göz At"** ve "Bu Hafta Trend Olanlar" gibi kürasyonlar yer alır.
2.  **Arama:** Arama çubuğuna "a space movie that feels realistic" yazar.
3.  **Arama Sonuçları Sayfası:** Arama sonuçları listelenir. Kullanıcı, ilgisini çeken bir filmin **afişinin üzerine fareyle gelerek sessiz fragman önizlemesini** izler.
4.  **Film Detay Sayfası:** Bir filme tıklar. Sayfada ilk olarak büyük film afişi, başlık ve "Fragmanı İzle" butonu dikkat çeker.
5.  **Keşif:** Sayfayı aşağı kaydırdıkça, önce Gemma 3'ün analizini, ardından "Nerede İzleyebilirim?" bloğunu ve en son topluluk yorumlarını görür.
6.  **Eylem:** "Nerede İzleyebilirim?" bloğundaki "Netflix'te İzle" butonuna tıklar ve platformdan ayrılır.

## 3. Sayfa Taslakları (Page Wireframes)

### 3.1. Genel Bileşenler (Global Components)
-   **Header (Üst Kısım):**
    -   Solda: Site Logosu.
    -   Ortada: Genel Arama Çubuğu.
    -   Sağda (Giriş Yapmamış): "Sign In", "Sign Up" butonları.
    -   Sağda (Giriş Yapmış): **Bildirim Zili (🔔)**, Kullanıcı Avatarı (tıklandığında menüyü açar).
-   **Footer (Alt Kısım):**
    -   "Hakkında", "Topluluk Kuralları", "Gizlilik Politikası", "İletişim".
    -   **"İstek & Öneri Gönder"** linki.

### 3.2. Ana Sayfa (`/`)
-   **Hero Bölümü:** Platformun ana sloganı ve büyük doğal dil arama çubuğu.
-   **Kişiselleştirilmiş Bölüm (Sadece Giriş Yapmış Kullanıcılar İçin):**
    -   "Senin İçin Seçtiklerimiz: Haftalık Keşif Radarı".
    -   "Moduna Göre: Beyin Yakan Filmler".
-   **Genel Bölümler:**
    -   **Türlere Göz At (Browse by Genre):** "Aksiyon", "Komedi", "Bilim Kurgu" gibi tıklanabilir kategori etiketleri/kartları.
    -   "Popüler Filmler".
    -   "Topluluktan Öne Çıkan Listeler".

### 3.3. Arama Sonuçları Sayfası (`/search`)
-   **Yapı:** Her bir filmin kart olarak listelendiği bir grid veya liste görünümü.
-   **Film Kartı Verileri:** Film Afişi, Film Adı, Yayın Yılı, Ortalama Puan, Kısa Özet (ilk ~150 karakter).
-   **İnteraktif Davranışlar (YENİ):**
    -   **Fareyle Üzerine Gelince Önizleme (Hover-to-Preview):** Kullanıcı fareyi bir film kartının üzerine getirdiğinde, afişin yerini filmin sessiz ve otomatik oynayan fragmanı alır. Fare çekildiğinde afiş geri döner.

### 3.4. Tür (Kategori) Sayfası (`/genre/{slug}`) - (YENİ)
-   **Başlık:** Türün adı (örn: "Aksiyon Filmleri").
-   **İçerik:** O türe ait filmlerin, popülerliğe göre sıralandığı ve sayfalama (pagination) içeren bir liste.
-   **Filtreleme:** Kullanıcıların sonuçları "En Yüksek Puanlı", "En Yeni" gibi kriterlere göre sıralayabilmesi için basit filtreleme seçenekleri.

### 3.5. Film Detay Sayfası (`/movie/{slug}`)
-   **İki Sütunlu Yapı (Masaüstü):**
    -   **Sol Sütun (Dar):**
        -   Film Afişi.
        -   **"Fragmanı İzle" Butonu** (Tıklandığında YouTube videosunu bir modal içinde açar).
        -   Etkileşim Butonları (Yıldızlar, Favori, Beğen, Listeye Ekle).
        -   "Nerede İzleyebilirim?" Bloğu.
    -   **Sağ Sütun (Geniş):**
        -   `Film Başlığı (Yıl)`
        -   `Türler (tıklanabilir linkler) | Süre | Yaş Sınırı`
        -   Gemma 3 Analiz Bloğu.
        -   Resmi Film Özeti.
        -   Topluluk Bloğu ve Yorumlar.

### 3.6. Yeni Topluluk Sayfaları
-   **Duyuru Panosu (`/whats-new`):**
    -   **Tetiklenme:** Header'daki bildirim zili (🔔) üzerinde okunmamış bir duyuru olduğunda kırmızı bir nokta belirir.
    -   **Arayüz:** Zile tıklandığında, son duyuruları gösteren bir dropdown açılır. "Tümünü Gör" linki, `/whats-new` sayfasına yönlendirir. Bu sayfada, admin tarafından eklenen tüm yenilikler ters kronolojik sırada listelenir.
-   **İstek ve Öneri Sayfası (`/feedback`):**
    -   **Tetiklenme:** Footer'daki "İstek & Öneri Gönder" linkine tıklanır.
    -   **Arayüz:** Kullanıcının konu seçip (Özellik İsteği, Hata Bildirimi vb.) mesajını yazabileceği basit bir form içerir.
