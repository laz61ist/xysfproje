# Admin Dashboard Specification (Yönetici Paneli Spesifikasyonu)

**Document Version:** 1.1
**Last Updated:** 18.08.2025

## 1. Genel Bakış ve Amaç

Bu doküman, Sinefil Radarı platformunun tüm operasyonel, içeriksel, topluluk ve finansal yönlerinin yönetileceği Yönetici Paneli'nin (Admin Dashboard) fonksiyonel gereksinimlerini ve modüllerini tanımlar. Admin Paneli, güvenli, sezgisel ve verimli bir arayüz sunarak site yöneticilerinin ve moderatörlerin platformu sağlıklı bir şekilde yönetmesini sağlamayı hedefler. Panel, ana siteden ayrı bir URL altında (örn: `/admin`) yer alacak ve özel bir yetkilendirme katmanına sahip olacaktır.

## 2. Erişim ve Güvenlik

-   **Yetkilendirme:** Sadece belirli rollere sahip kullanıcılar panele erişebilir. `users` tablosuna `role` (text) adında bir sütun eklenecektir. Olası roller: `user`, `moderator`, `admin`.
-   **Giriş:** Admin paneline erişim, ana siteyle aynı Supabase Auth sistemini kullanır, ancak giriş yapıldıktan sonra kullanıcının `role` bilgisi kontrol edilir. Rolü `user` olanlar bu alana yönlendirilmez.
-   **Güvenlik:** Tüm admin eylemleri, sunucu tarafında yetki kontrolünden geçirilmeli ve kimin hangi eylemi ne zaman yaptığına dair bir denetim kaydı (audit log) tutulmalıdır.

## 3. Dashboard Modülleri

Admin paneli, sol tarafta bir menü ile gezilebilen aşağıdaki ana modüllerden oluşacaktır.

### 3.1. Ana Panel (Dashboard Home)

-   **Amaç:** Platformun genel sağlığını bir bakışta gösteren özet bir ekran.
-   **Gösterilecek Metrikler (Widgets):**
    -   Son 24 saatte kayıt olan yeni kullanıcı sayısı.
    -   Onay bekleyen rapor sayısı.
    -   **Bu Ayki Toplam AI Maliyeti (USD).**
    -   **Bugünkü Toplam Token Kullanımı.**
    -   Son 7 günde eklenen yorum sayısı.
    -   Toplam kullanıcı, film ve liste sayısı.

### 3.2. Kullanıcı Yönetimi (User Management)

-   **Amaç:** Kullanıcıları görüntülemek, aramak ve yönetmek.
-   **Özellikler:**
    -   Tüm kullanıcıları listeleyen, arama ve filtreleme yapılabilen bir tablo.
    -   **Kullanıcı Detay Sayfası:**
        -   Kullanıcı bilgilerini görüntüleme ve düzenleme.
        -   Kullanıcı rolünü değiştirme (`user` <-> `moderator`).
        -   Kullanıcı statüsünü değiştirme (`is_supporter`).
        -   Kullanıcıya uyarı gönderme, susturma (mute) veya yasaklama (ban).

### 3.3. İçerik Yönetimi (Content Management)

-   **Filmler (Movies):**
    -   Sistemdeki filmleri listeleme ve arama.
    -   TMDB ID'si ile manuel olarak yeni bir film ekleme.
    -   Bir film için Gemma 3 analizini yeniden tetikleme.
-   **Yorumlar (Comments) & Listeler (Lists):**
    -   Tüm yorumları/listeleri arama, filtreleme ve görüntüleme.
    -   Uygunsuz bulunan yorumları/listeleri düzenleme veya silme.

### 3.4. Topluluk ve Moderasyon (Community & Moderation)

-   **Amaç:** Platformun sosyal sağlığını korumak.
-   **Rapor Kuyruğu (Report Queue):**
    -   Kullanıcılar tarafından gönderilen tüm raporların listelendiği ana moderasyon ekranı.
    -   Hızlı aksiyon butonları: "Raporu Reddet", "İçeriği Sil", "Kullanıcıyı Uyar".
-   **Kelime Filtresi (Word Filter):**
    -   Yasaklı kelime listesini (blacklist) yönetmek için bir arayüz.

### 3.5. AI Maliyet Merkezi (AI Cost Center) - (YENİ)

-   **Amaç:** Yapay zeka kullanımını ve maliyetlerini şeffaf bir şekilde izlemek ve kontrol etmek.
-   **Özet Ekranı:**
    -   Son 30 gün içindeki günlük AI maliyetini gösteren bir çizgi grafiği.
    -   Toplam maliyetin görevlere göre dağılımını (`analyze_movie`, `weekly_discovery` vb.) gösteren bir pasta grafiği.
-   **Detaylı Log Tablosu:**
    -   `ai_usage_logs` tablosundaki tüm kayıtların listelendiği, filtrelenebilir ve sıralanabilir bir tablo (en maliyetli çağrılar, en çok token harcayanlar vb.).

### 3.6. Gelir Modelleri Yönetimi (Monetization Management)

-   **Amaç:** Gelir getiren sistemleri yapılandırmak ve yönetmek.
-   **Yönlendirme Partnerleri (Affiliate Partners):**
    -   Affiliate anlaşması yapılan platformları eklemek/düzenlemek için bir arayüz (`partner_name`, `affiliate_tag`).
-   **Reklam Alanları (Ad Slots):**
    -   Sitedeki reklam alanlarını yönetmek ve Google AdSense kodlarını yapıştırmak için bir arayüz.
-   **Destekçiler (Supporters):**
    -   Platforma destek olan tüm kullanıcıların listesi ve ödeme webhook logları.

### 3.7. Ayarlar (Settings)

-   **Amaç:** Platformun genel davranışlarını ve entegrasyonlarını yönetmek.
-   **API Anahtarları:**
    -   `GEMMA_API_KEY`, `TMDB_API_KEY` gibi anahtarları güvenli bir şekilde girmek ve güncellemek için bir form.
-   **AI Maliyet Yapılandırması:**
    -   `config/ai.php` dosyasındaki token başına maliyet oranlarını doğrudan panel üzerinden güncelleyebilmek için bir arayüz.
-   **Önbellek Yönetimi (Cache Management):**
    -   Sitenin çeşitli bölümlerindeki önbelleği manuel olarak temizlemek için bir buton.
