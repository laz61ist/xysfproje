# 22. Utility Scripts Specification (Yardımcı Betiklerin Spesifikasyonu)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Amaç

Bu doküman, Sinefil Radarı projesinin web sunucusu üzerinden değil, doğrudan komut satırı arayüzünden (CLI) çalıştırılan yardımcı betiklerin (utility scripts) teknik detaylarını, mantığını ve kullanımını tanımlar. Bu betikler, genellikle tek seferlik veri doldurma, periyodik bakım veya zamanlanmış görevler için kullanılır.

---

## **2. Veritabanı Isıtma Betiği (`warmup.php`)**

### 2.1. Hedef

Bu betiğin temel hedefi, projenin lansmanından hemen önce çalıştırılarak "Soğuk Başlangıç" (Cold Start) problemini çözmektir. Veritabanını, yeni ziyaretçilerin ilk olarak karşılaşacağı en popüler içeriklerle önceden doldurarak, ilk kullanıcı deneyimini maksimum hıza ve kaliteye ulaştırır.

### 2.2. Çalıştırma Ortamı

-   **Tür:** Komut Satırı Betiği (CLI Script).
-   **Komut:** `php warmup.php`
-   **Zamanlama:** Sadece bir kez, projenin ilk lansmanından önce manuel olarak çalıştırılır.

### 2.3. Bağımlılıklar

Bu betik, çalışabilmek için projenin ana uygulama mantığına erişmelidir. Betiğin en başında, uygulama "bootstrap" dosyası çağrılmalıdır. Bu dosya:
-   `.env` dosyasındaki ortam değişkenlerini yükler.
-   Gerekli servis sınıflarını (`TMDBService`, `GemmaService`) ve veritabanı bağlantısını hazır hale getirir.

### 2.4. Adım Adım İşleyiş Mantığı

1.  **Başlatma ve Bilgilendirme:**
    -   Betiği çalıştıran geliştiriciye sürecin başladığını belirten bir mesaj gösterir. (Örn: `echo "Sinefil Radarı Veritabanı Isıtma Betiği Başlatılıyor..."`).
    -   Uzun sürecek bir işlem olduğu için her adımda ilerleme durumu hakkında bilgi verir.

2.  **Görev 1: Türleri (Genres) Çek ve Kaydet**
    -   `TMDBService`'in `getGenreList()` metodunu çağırır (Bu metod `GET /genre/movie/list` uç noktasına istek atar).
    -   Gelen her bir tür için:
        -   `genres` tablosunda bu ID'nin olup olmadığını kontrol eder.
        -   Yoksa, `id`, `name` ve addan oluşturulmuş `slug` ile birlikte `genres` tablosuna kaydeder.
    -   İşlem tamamlandığında bilgi verir (Örn: `echo "Tüm film türleri veritabanına eklendi."`).

3.  **Görev 2: Popüler Filmleri Çek ve İşle**
    -   Bir döngü başlatarak `TMDBService`'in `getPopularMovies($page)` metodunu çağırır. Döngü, yaklaşık 250 filme ulaşana kadar (örn: ilk 13 sayfa) devam eder.
    -   Her bir film için aşağıdaki alt adımları uygular:
        a.  **Var mı Kontrolü:** Filmin `tmdb_id`'sinin bizim `movies` tablomuzda olup olmadığını kontrol eder. Varsa, bu filmi atlar ve bir sonraki filme geçer (bu, betiğin yarıda kesilmesi durumunda tekrar çalıştırılabilir olmasını sağlar - idempotency).
        b.  **Detayları Çek:** `TMDBService`'in `getMovieDetails($tmdb_id)` metodunu çağırarak filmin tüm detaylarını (oyuncular, fragmanlar, anahtar kelimeler ve ait olduğu türlerin listesi dahil) alır.
        c.  **Filmi Kaydet:** Gelen ana film verilerini `movies` tablomuza kaydeder.
        d.  **Türleri İlişkilendir:** Gelen tür listesindeki her bir tür ID'si için, `movie_genres` ara tablomuza bir `(movie_id, genre_id)` kaydı ekler.
        e.  **AI ile Zenginleştir:** `GemmaService`'in `analyzeMovieContent()` metodunu çağırarak film için yapay zeka analizini oluşturur.
        f.  **Analizi Kaydet:** Gelen JSON analizini `movie_analyses` tablomuza kaydeder.
        g.  **Maliyeti Logla:** Bu AI çağrısının token ve maliyet bilgilerini `ai_usage_logs` tablomuza kaydeder.
        h.  **İlerleme Bildir:** İşlenen her filmden sonra ekrana bir ilerleme mesajı basar (Örn: `echo "İşlendi: (5/250) The Dark Knight"`).

4.  **Hata Yönetimi:**
    -   Döngü içindeki her bir film işleme adımı, bir `try...catch` bloğu içinde olmalıdır. Eğer tek bir filmde (örn: TMDB API hatası) sorun yaşanırsa, betik çökmeyip hatayı ekrana basmalı (`echo "HATA: Inception işlenemedi. Sebep: ..."` ) ve bir sonraki filmle devam etmelidir.

5.  **Tamamlama:**
    -   Tüm filmler işlendikten sonra, betik toplam süreyi ve işlenen film sayısını belirten bir başarı mesajı ile sonlanır. (Örn: `echo "Isıtma işlemi tamamlandı! Toplam 250 film 15 dakika içinde işlendi."`).

### 2.5. `warmup.php` Kavramsal Kodu (Pseudocode)

```php
<?php

// 1. Uygulamayı başlat (config, servisler vb.)
require __DIR__ . '/bootstrap.php';

// Gerekli servisleri oluştur
$tmdbService = new TMDBService();
$gemmaService = new GemmaService();
$db = new Database(); // Veritabanı işlemleri için

echo "--- Isıtma Betiği Başlatıldı ---\n";

// 2. Görev 1: Türleri çek
echo "Türler çekiliyor...\n";
$genres = $tmdbService->getGenreList();
foreach ($genres as $genre) {
    $db->table('genres')->upsert($genre);
}
echo "Türler eklendi.\n";

// 3. Görev 2: Popüler filmleri çek
echo "Popüler filmler işleniyor...\n";
$movieCount = 0;
$page = 1;
define('MOVIE_LIMIT', 250);

while ($movieCount < MOVIE_LIMIT) {
    $popularMovies = $tmdbService->getPopularMovies($page);
    
    foreach ($popularMovies as $popularMovie) {
        if ($movieCount >= MOVIE_LIMIT) break;

        try {
            // a. Var mı kontrolü
            if ($db->table('movies')->exists(['tmdb_id' => $popularMovie['tmdb_id']])) {
                echo "Atlandı (zaten var): " . $popularMovie['title'] . "\n";
                continue;
            }

            // b, c, d. Detayları çek, kaydet, türleri ilişkilendir
            $movie = $tmdbService->getMovieDetails($popularMovie['tmdb_id']);
            $db->saveMovieWithGenres($movie);

            // e, f, g. AI ile zenginleştir, kaydet, logla
            $analysis = $gemmaService->analyzeMovieContent($movie);
            $db->saveMovieAnalysis($movie['id'], $analysis);
            
            $movieCount++;
            echo "İşlendi ($movieCount/" . MOVIE_LIMIT . "): " . $movie['title'] . "\n";

        } catch (Exception $e) {
            echo "HATA: " . $popularMovie['title'] . " işlenemedi. Sebep: " . $e->getMessage() . "\n";
        }
    }
    $page++;
}

echo "--- Isıtma Betiği Tamamlandı ---\n";
```
