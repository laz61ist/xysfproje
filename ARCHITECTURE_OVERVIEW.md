# Architecture Overview (Mimari Genel Bakış)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Felsefe ve Yaklaşım

Sinefil Radarı'nın mimarisi, **hızlı geliştirme**, **ölçeklenebilirlik**, **güvenlik** ve **düşük bakım maliyeti** prensipleri üzerine kurulmuştur. Bu hedeflere ulaşmak için, geleneksel monolitik bir yapı yerine, güçlü bir **Backend-as-a-Service (BaaS)** platformu (Supabase) ile entegre çalışan, hafif bir sunucu tarafı uygulaması (Framework'süz PHP) benimsenmiştir. Bu hibrit yaklaşım, bize hem Supabase'in hazır çözümlerinin hızını hem de kendi iş mantığımızı uygulama esnekliğini sunar.

## 2. Mimari Şeması

```
+------------------+      +---------------------+      +------------------------+
|   Kullanıcı      |      |     Web Sunucusu    |      | Harici Servisler       |
| (Tarayıcı)       |      |    (Linux/Apache)   |      | (3rd Party APIs)       |
+------------------+      +----------+----------+      +-----------+------------+
        |                        ^     |                           ^
        |                        |     |                           |
        | HTTP(S) İstekleri      |     | API İstekleri             |
        | (örn: /movie/dune)     |     | (PHP cURL/Guzzle)         |
        |                        |     |                           |
        v                        |     v                           |
+------------------+      +----------+----------+      +-----------+------------+
|   Frontend       |      |   Backend (PHP)     |----->|   - TMDB API           |
| (HTML, CSS, JS)  |      |  (MVC-benzeri yapı) |      |   - JustWatch API      |
+------------------+      +----------+----------+      |   - Gemma 3 API        |
        ^                        ^     |               +------------------------+
        |                        |     |
        | HTML Cevapları         |     | Güvenli API Çağrıları
        |                        |     | (Service Role Key ile)
        |                        |     |
        v                        |     v
+------------------------------------+------------------------------------------+
|                                                                                |
|                           Supabase (Backend-as-a-Service)                      |
|                                                                                |
|  +---------------------+  +---------------------+  +-------------------------+ |
|  |    Authentication   |  |   PostgreSQL DB     |  |   Storage               | |
|  | (Kullanıcı Yönetimi)|  | (Auto-generated API)|  | (Profil Resimleri vb.)  | |
|  +---------------------+  +---------------------+  +-------------------------+ |
|                                                                                |
+--------------------------------------------------------------------------------+
```

## 3. Bileşenlerin Açıklaması

### 3.1. Frontend (Kullanıcı Arayüzü)
- **Teknoloji:** Standart HTML5, CSS3, ve Vanilla JavaScript.
- **Sorumlulukları:**
    - Sunucudan (PHP) gelen dinamik HTML'i render etmek.
    - Kullanıcı etkileşimlerini (buton tıklamaları, form gönderimleri) yönetmek.
    - Asenkron istekler (örn: bir filmi favorilere ekleme) için basit AJAX/Fetch API çağrıları yapmak.
    - Arayüz tamamen sunucu tarafında oluşturulur (Server-Side Rendering), bu da SEO uyumluluğunu ve ilk sayfa yükleme hızını artırır.

### 3.2. Backend (Sunucu Uygulaması)
- **Teknoloji:** Framework'süz PHP 8.x, Composer (paket yönetimi için).
- **Mimari Yapı:** MVC (Model-View-Controller) prensiplerinden esinlenilmiş, organize bir klasör yapısı.
    - **Controllers:** HTTP isteklerini karşılar, gerekli servisleri/modelleri çağırır ve sonucu View'e gönderir.
    - **Models:** İş mantığını ve veri erişimini içerir. Supabase ve diğer harici API'lerle iletişimden sorumludur. **Doğrudan SQL sorgusu yazmaz.** Bunun yerine Supabase'in REST API'sini veya PHP client kütüphanesini kullanır.
    - **Views:** HTML şablonlarını içerir. Controller'dan gelen verileri kullanarak son kullanıcıya gösterilecek HTML'i oluşturur.
- **Sorumlulukları:**
    - Gelen URL'leri yönlendirmek (Routing).
    - Kullanıcı oturumlarını yönetmek.
    - Supabase ve diğer harici API'lerden gelen verileri birleştirmek ve işlemek.
    - Gemma 3 API'sine **güvenli ve yapılandırılmış prompt'lar** göndermek ve cevaplarını işlemek.
    - Nihai HTML çıktısını oluşturup kullanıcıya göndermek.

### 3.3. Supabase (Backend-as-a-Service)
Projemizin bel kemiğidir. Birçok karmaşık backend işlevini bizim için yönetir.
- **Authentication:**
    - E-posta/şifre, sosyal medya (Google vb.) ile kullanıcı kayıt ve giriş işlemlerini tamamen yönetir.
    - Güvenli JWT (JSON Web Tokens) tabanlı oturum yönetimi sağlar.
- **PostgreSQL Veritabanı:**
    - Tüm uygulama verilerimiz (kullanıcılar, filmler, listeler, yorumlar vb.) burada saklanır.
    - En güçlü özelliği: Tablolarımız için otomatik olarak **RESTful API uç noktaları** oluşturur.
    - Veritabanı erişim güvenliği **Row Level Security (RLS)** politikaları ile sağlanır. (Örn: "Bir kullanıcı sadece kendi özel listelerini görebilir.").
- **Storage:**
    - Kullanıcıların profil resimleri, liste kapakları gibi statik dosyaları güvenli bir şekilde saklamak için kullanılır.

### 3.4. Harici Servisler (3rd Party APIs)
- **Google Gemma 3 API:**
    - Projemizin yapay zeka beynidir.
    - **Sadece Backend (PHP) üzerinden çağrılır.** API anahtarları asla istemci tarafına (tarayıcıya) gönderilmez.
    - Kişiselleştirilmiş öneriler, metin analizleri ve özetler oluşturmak için kullanılır.
- **The Movie Database (TMDB) API:**
    - Film meta verileri (afişler, özetler, oyuncu kadrosu, puanlar vb.) için ana kaynağımızdır.
- **JustWatch API (veya alternatifi):**
    - Filmlerin hangi platformlarda izlenebileceği bilgisini almak için kullanılır.

## 4. Veri Akış Örneği: "Bir Filme Yorum Yapma"

1.  **Kullanıcı** `sinefilradari.com/movie/dune` sayfasındaki yorum formunu doldurur ve "Gönder" butonuna tıklar.
2.  **Frontend**, form verilerini AJAX ile `POST /comment/add` URL'ine gönderir. Bu istek, kullanıcının Supabase'den aldığı JWT token'ını da içerir.
3.  **Backend (PHP)**, `/comment/add` isteğini karşılar. `CommentController` devreye girer.
4.  **Controller**, gelen JWT token'ını Supabase'e göndererek kullanıcının kimliğini doğrular.
5.  **Controller**, `CommentModel`'i çağırır.
6.  **CommentModel**, Supabase'in PHP client'ını kullanarak `comments` tablosuna yeni bir kayıt eklemek için Supabase API'sine bir istek gönderir. Bu istek, **Backend'in güvenli service_role key'i** ile yapılır.
7.  **Supabase**, veritabanına kaydı ekler ve başarılı olduğuna dair bir yanıt döner.
8.  **Backend**, başarılı yanıtını JSON formatında **Frontend**'e geri gönderir.
9.  **Frontend**, başarılı yanıtı alınca sayfayı yenilemeden yeni yorumu yorumlar listesine dinamik olarak ekler.

Bu yapı, her bileşenin sorumluluğunu net bir şekilde ayırarak geliştirme sürecini basitleştirir ve güvenliği en üst düzeye çıkarır.
