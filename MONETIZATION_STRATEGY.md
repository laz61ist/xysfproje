# Monetization Strategy Specification (Gelir Modeli Stratejisi Spesifikasyonu)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Felsefe ve Amaç

Bu doküman, Sinefil Radarı platformunun finansal sürdürülebilirliğini sağlamak için kullanılacak gelir modellerini ve bu modellerin teknik entegrasyonunu tanımlar. Temel felsefemiz, **kullanıcı deneyimini önceliklendiren, zorlayıcı olmayan ve platformun sunduğu değere paralel** gelir akışları oluşturmaktır. Agresif ve rahatsız edici reklam modellerinden kaçınarak, geliri platformun doğal akışına ve kullanıcıların gönüllü desteğine dayandıracağız.

## 2. Gelir Modelleri

Stratejimiz üç ana gelir modelinden oluşmaktadır:

1.  **Yönlendirme Ortaklığı (Affiliate Marketing):** Kullanıcıları filmleri izleyebilecekleri platformlara yönlendirerek komisyon kazanma. (Ana Gelir Kaynağı)
2.  **Gönüllü Destekçi Modeli (Supporter Model):** Kullanıcılara reklamsız bir deneyim ve ek avantajlar sunarak doğrudan desteklerini alma. (Topluluk ve Gelir Kaynağı)
3.  **Stratejik Reklam Yerleşimi (Strategic Advertising):** Destekçi olmayan kullanıcılara, deneyimi en az bozacak şekilde reklam gösterme. (İkincil Gelir Kaynağı)

## 3. Modellerin Detayları

### 3.1. Yönlendirme Ortaklığı (Affiliate Marketing)

-   **Amaç:** "Nerede İzleyebilirim?" özelliğini, kullanıcı için bir kolaylık aracından pasif bir gelir kaynağına dönüştürmek.
-   **İşleyiş:**
    1.  Apple TV, Amazon Prime Video gibi platformların yönlendirme ortaklığı programlarına kaydolunur.
    2.  Her partnerden, bize özel bir kimlik içeren (örn: `?tag=sinefilradari-21`) bir "affiliate tag" alınır.
    3.  "Nerede İzleyebilirim?" motoru, JustWatch veya TMDB API'sinden aldığı orijinal izleme linklerine, bizim veritabanımızda saklanan bu "affiliate tag"i otomatik olarak ekler.
    4.  Kullanıcı bu linke tıklayıp yönlendirildiği platformda bir film kiraladığında, satın aldığında veya yeni bir abonelik başlattığında, Sinefil Radarı belirli bir oranda komisyon kazanır.
-   **Teknik Uygulama:**
    -   Admin panelinde, `affiliate_partners` adında bir yönetim bölümü oluşturulacaktır.
    -   Bu bölümde her partner için `partner_name`, `base_url`, ve `affiliate_tag` gibi bilgiler saklanacaktır.
    -   Backend'deki servis, API'den gelen linkin hangi partnere ait olduğunu tespit edip, o partnere ait doğru `affiliate_tag`i linke ekleyecektir.
-   **Şeffaflık:** Kullanıcı güvenini korumak için, sitenin altbilgisinde (footer) veya "Hakkında" sayfasında, platformun bu tür yönlendirme linklerinden gelir elde edebileceğini belirten şeffaf bir açıklama yer almalıdır.

### 3.2. Gönüllü Destekçi Modeli (Supporter Model)

-   **Amaç:** Platformu seven ve gelişimini desteklemek isteyen en sadık kullanıcılardan, abonelik baskısı olmadan, doğrudan gelir elde etmek.
-   **Platform Entegrasyonu:** "Buy Me a Coffee" veya Stripe gibi ödeme platformları kullanılacaktır. "Buy Me a Coffee" başlangıç için daha basit ve samimi bir yaklaşım sunar.
-   **Arayüz:** Sitenin çeşitli yerlerinde (gezinti menüsü, sayfa altı vb.) "Sinefil Radarı'na Destek Ol" veya "Bize Bir Kahve Ismarla" gibi davetkar butonlar bulunacaktır.
-   **Destekçi Avantajları (Supporter Perks):** Tek seferlik veya aylık destek yapan kullanıcılar "Destekçi" statüsü kazanır ve aşağıdaki avantajlara sahip olur:
    1.  **Tamamen Reklamsız Deneyim:** Platformdaki tüm Google Ads reklam alanları, destekçi statüsüne sahip kullanıcılar için otomatik olarak gizlenir.
    2.  **Özel Profil Rozeti:** Kullanıcı profillerinde, onları onurlandıran "Destekçi" veya "Patron" gibi özel bir görsel rozet belirir.
    3.  **Gelecekteki Premium Özelliklere Erken Erişim:** Geliştirilecek yeni beta özelliklerini ilk deneyenler arasında olma imkanı.
-   **Teknik Uygulama:**
    -   `users` tablosuna `is_supporter` (boolean) ve `supporter_tier` (text) gibi sütunlar eklenecektir.
    -   Buy Me a Coffee/Stripe, bir ödeme başarılı olduğunda sistemimize bir "webhook" (otomatik bildirim) gönderir.
    -   Bu webhook'u dinleyen bir backend API ucu (`/api/webhook/payment-success`), ödemeyi yapan kullanıcının `is_supporter` statüsünü `true` olarak günceller.
    -   Frontend ve backend, bir içeriği göstermeden önce kullanıcının bu statüsünü kontrol ederek reklamları gösterip göstermemeye karar verir.

### 3.3. Stratejik Reklam Yerleşimi (Strategic Advertising)

-   **Amaç:** Destekçi olmayan kullanıcılardan, kullanıcı deneyimini minimum düzeyde etkileyerek ikincil bir gelir elde etmek.
-   **Reklam Ağı:** **Google AdSense**, başlangıç için en uygun ve yönetimi en kolay reklam ağıdır.
-   **Google Ads Politikaları ve SEO Uyumluluğu:**
    -   Reklamlar asla içeriğin önüne geçmemeli veya kullanıcıyı yanıltıcı olmamalıdır. Pop-up veya sayfa içeriğini iten (layout-shifting) reklamlardan kaçınılacaktır.
    -   Kullanıcı tarafından oluşturulan içeriklerin (listeler, yorumlar) denetlenmesi, Google'ın "sakıncalı içerik" politikalarına uyum için kritik öneme sahiptir.
    -   SEO için oluşturulan özgün ve değerli içerik (Gemma 3 analizleri, kullanıcı listeleri), Google'ın siteyi "düşük kaliteli" olarak görmesini engelleyecek ve reklam gelirini olumlu etkileyecektir.
-   **Reklam Alanları (Ad Slots):** Admin panelinden yönetilebilen, belirli ve stratejik alanlar tanımlanacaktır:
    -   **Header Altı Banner (Leaderboard Ad):** Sayfanın üst kısmında, ana içeriği etkilemeden.
    -   **Sidebar Reklamı (Skyscraper Ad):** Masaüstü görünümünde, sayfa kenarında.
    -   **İçerik Arası Reklam (In-Feed Ad):** Uzun listelerde (örn: film listeleri, yorumlar) her 5-10 içerik öğesinden sonra bir adet.
-   **Teknik Uygulama:**
    -   Admin panelinde, her reklam alanı için bir metin kutusu bulunacaktır. Buraya Google AdSense'ten alınan `<script>` kodları yapıştırılır.
    -   View (görünüm) dosyaları, bu reklam kodlarını veritabanından çeker ve sayfaya yerleştirir.
    -   Reklam kodunu içeren ana PHP fonksiyonu, her zaman önce `if (!currentUser.is_supporter)` gibi bir kontrol yaparak, destekçi kullanıcılar için bu kodların render edilmesini engelleyecektir.

Bu üçlü gelir modeli, platformun hem finansal olarak sağlıklı kalmasını hem de kullanıcı odaklı felsefesini korumasını sağlayacak dengeli bir yapı sunar.
