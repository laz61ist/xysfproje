# Third-Party API Contracts (Üçüncü Parti API Sözleşmeleri)

**Document Version:** 1.2
**Last Updated:** 18.08.2025

## 1. Genel Bakış ve Amaç

Bu doküman, Sinefil Radarı platformunun işlevselliği için hayati öneme sahip olan üçüncü parti (harici) API'lerle olan etkileşimini, "sözleşme" niteliğinde tanımlar. Amaç, hangi API'nin ne için kullanılacağını, hangi uç noktaların (endpoints) çağrılacağını, veri limitlerini ve hata yönetimi stratejilerini net bir şekilde ortaya koyarak geliştirme sürecini standartlaştırmaktır. Tüm API etkileşimleri, sunucu tarafındaki (PHP backend) özel "Servis" sınıfları (`TMDBService`, `GemmaService` vb.) içinde soyutlanacaktır.

## 2. API Entegrasyonları

### 2.1. The Movie Database (TMDB)

-   **Rolü:** Platformun ana film meta veri kaynağı. Afişler, özetler, oyuncu kadroları, puanlar, fragmanlar, **türler (kategoriler)** ve benzer filmler gibi bilgiler için birincil kaynaktır.
-   **Kimlik Doğrulama:** Tüm isteklerde `.env` dosyasında saklanan `TMDB_API_KEY`'i bir query parametresi olarak (`?api_key=...`) gönderilecektir.
-   **Temel Uç Noktalar (Endpoints):**
    -   **Film Arama:** `GET /search/movie`
        -   **Kullanım:** Kullanıcı arama çubuğuna bir film adı yazdığında tetiklenir.
    -   **Film Detayları:** `GET /movie/{movie_id}`
        -   **Kullanım:** Bir filmin ana sayfasını oluşturmak için gerekli tüm temel bilgileri tek seferde almak için kullanılır.
        -   **Parametre:** `append_to_response=credits,keywords,videos` parametresi, temel film bilgileriyle birlikte; oyuncu kadrosu, anahtar kelimeler ve video (fragman) verilerini tek bir API çağrısında verimli bir şekilde almak için **mutlaka kullanılacaktır.**
    -   **Tür Listesi (Genre List) - (YENİ):** `GET /genre/movie/list`
        -   **Kullanım:** Lansman öncesi `warmup.php` betiği tarafından, sistemdeki tüm olası film türlerini çekmek ve `genres` tablomuza kaydetmek için **bir kez** kullanılır.
    -   **Benzer Filmler:** `GET /movie/{movie_id}/similar`
        -   **Kullanım:** "Haftalık Keşif Radarı" için aday havuzu oluşturmak ve film detay sayfalarında benzer öneriler sunmak için kullanılır.
    -   **İzleme Sağlayıcıları:** `GET /movie/{movie_id}/watch/providers`
        -   **Kullanım:** "Nerede İzleyebilirim?" motoru için bir filmin hangi platformlarda mevcut olduğunu ülke bazında (`watch_region=TR`) öğrenmek için kullanılır.

-   **Arama Sayfası Fragman Önizleme Stratejisi - (YENİ):**
    -   Arama sonuçları sayfasının (`/search`) ilk yükleme hızını korumak için, fragman bilgileri sunucu tarafından değil, **istemci (JavaScript) tarafından talep üzerine** getirilir.
    -   Sayfa yüklendikten sonra, listelenen her film için `GET /movie/{movie_id}/videos` uç noktasına arka planda asenkron bir API çağrısı yapılır. Bu çağrıdan gelen YouTube anahtarı (`key`), "Hover-to-Preview" özelliğini beslemek için kullanılır.

-   **Hız Limiti ve Önbellekleme (Rate Limiting & Caching):**
    -   TMDB API'sinin istek limitleri bulunmaktadır. Bu limitleri aşmamak için, API'den alınan ve sık değişmeyen veriler (film detayları, oyuncu kadrosu, fragman anahtarı, türler vb.) kendi veritabanımızda **önbelleklenecektir**.
    -   Bir film detayına istek geldiğinde, sistem önce kendi veritabanını kontrol edecek, veri yoksa veya çok eskiyse TMDB API'sine istek atacaktır.

### 2.2. Google AI - Gemma 3

-   **Rolü:** Platformun yapay zeka beyni. Özgün içerik üretme, kişiselleştirme ve veri analizi için kullanılır.
-   **Kimlik Doğrulama:** Tüm isteklerde `x-goog-api-key` HTTP başlığında, `.env` dosyasında saklanan `GEMMA_API_KEY` gönderilecektir.
-   **Temel Uç Nokta (Endpoint):**
    -   **İçerik Üretme:** `POST /v1beta/models/{model_name}:generateContent`
-   **İstek Yapısı ve Veri Formatı:**
    -   Tüm istekler, içeriğin JSON formatında olmasını talep eden `responseMimeType: application/json` parametresini içerecektir.
-   **Hız Limiti ve Önbellekleme:**
    -   Gemma API çağrıları hem maliyetli hem de yavaştır. Bu nedenle, üretilen her yapay zeka içeriği istisnasız olarak veritabanımızda (`movie_analyses`) önbelleklenecektir.

### 2.3. JustWatch (Alternatif/Gelecek)

-   **Rolü:** TMDB'nin izleme sağlayıcıları verisinin yetersiz kaldığı durumlarda, daha kapsamlı "Nerede İzleyebilirim?" verisi sağlamak için potansiyel bir alternatif.
-   **Entegrasyon Durumu:** v1.0 için kapsam dışı, ancak gelecek sürümler için değerlendirilecek.

## 4. Hata Yönetimi (Error Handling)

Harici API'ler her zaman kesintiye uğrayabilir. Servis sınıflarımız bu durumlara karşı hazırlıklı olmalıdır.

-   **Zaman Aşımı (Timeout):** Tüm API isteklerinde makul bir zaman aşımı süresi (örn: 10-15 saniye) belirlenmelidir.
-   **HTTP Hata Kodları:** `401`, `403`, `404`, `429` (Too Many Requests), `5xx` (Server Error) gibi standart hata kodları yakalanmalı ve uygun şekilde yönetilmelidir (loglama, yeniden deneme mekanizması vb.).
-   **Beklenmedik Veri Formatı:** API'den beklenen formatta bir yanıt gelmezse, bu durum da bir hata olarak kabul edilmeli ve loglanmalıdır.
