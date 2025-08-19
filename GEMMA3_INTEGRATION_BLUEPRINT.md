# Gemma 3 Integration Blueprint (Gemma 3 Entegrasyon Planı)

**Document Version:** 1.2
**Last Updated:** 18.08.2025

## 1. Felsefe ve Strateji

Google Gemma 3, Sinefil Radarı platformunun sadece bir "eklenti"si değil, temel bir "bileşeni"dir. Amacımız, Gemma 3'ü kullanıcı deneyiminin her aşamasına entegre ederek, platformumuzu statik bir veritabanından, kullanıcıyı anlayan ve ona özel dinamik içerikler sunan akıllı bir asistana dönüştürmektir.

**Temel Stratejiler:**

1.  **Güvenlik Önceliklidir:** Tüm Gemma 3 API çağrıları, **yalnızca sunucu tarafından (PHP backend)** yapılacaktır. `GEMMA_API_KEY`, istemci tarafına (tarayıcı) asla gönderilmeyecektir.
2.  **Maliyet ve Performans Optimizasyonu:** API çağrıları maliyetli ve zaman alıcı olabilir. Bu nedenle, Gemma 3 tarafından üretilen ve sıkça erişilen içerikler (örn: bir filmin analizi) veritabanımızda **önbelleklenecek (cached)**.
3.  **Yapılandırılmış Çıktı (Structured Output):** Gemma 3'ten gelen cevapların serbest metin yerine, işlenmesi kolay olan **JSON formatında** olması hedeflenecektir. Bu, prompt mühendisliği ile sağlanacaktır.
4.  **Ayrılmış Servis Mimarisi:** Gemma 3 ile ilgili tüm mantık, PHP kod tabanında `app/Services/GemmaService.php` gibi özel bir servis sınıfı içinde toplanacaktır.
5.  **Maliyet Takibi (Cost Tracking):** Her bir API çağrısının token kullanımı ve tahmini maliyeti, operasyonel şeffaflık için **istisnasız olarak loglanacaktır**.

## 2. Entegrasyon Mimarisi

```
+----------------+      +-----------------------+      +-------------------+
|   PHP Backend  |----->|   GemmaService.php    |----->|   Google AI API   |
| (Controller'lar|      | (Merkezi Servis Sınıfı)|      |   (Gemma 3)       |
|  veya Cron Job)|      +-----------+-----------+      +-------------------+
+----------------+                  |
                                    | Caching & Logging
                                    v
                         +-----------------------------+
                         |     Supabase Veritabanı     |
                         | - `movie_analyses` (Cache)  |
                         | - `ai_usage_logs` (Log)     |
                         +-----------------------------+
```

## 3. API Çağrı Yapısı ve Model Seçimi

-   **API Uç Noktası (Endpoint):** `https://generativelanguage.googleapis.com/v1beta/models/{model_name}:generateContent`
-   **Model Adı (`model_name`):** Hedef modelimiz `gemma-27b-it`'nin API'deki resmi adı (örn: `gemma-27b-it`) olacaktır. Bu, `.env` dosyasında yapılandırılabilir bir değişken olacaktır.
-   **Yetkilendirme:** `x-goog-api-key` HTTP başlığında, `.env` dosyasında saklanan `GEMMA_API_KEY` gönderilecektir.
-   **İstek Yapısı:**
    ```json
    {
      "contents": [
        { "parts": [ { "text": "Your detailed prompt text..." } ] }
      ],
      "generationConfig": {
        "responseMimeType": "application/json"
      }
    }
    ```
-   **Yanıt Yapısı (Response Structure):** Başarılı bir yanıttan iki ana bilgi alınacaktır:
    1.  **`candidates`**: Modelin ürettiği asıl içerik (bizim durumumuzda JSON formatında metin).
    2.  **`usageMetadata`**: Çağrının token bilgilerini (`promptTokenCount`, `candidatesTokenCount`) içeren, maliyet takibi için **kritik öneme sahip** nesne.

## 4. Maliyet Takip ve Loglama Mekanizması

-   **Sorumluluk:** `GemmaService.php` sınıfı, Gemma 3 API'sine yaptığı her çağrıdan sonra, API yanıtından gelen `usageMetadata`'yı işlemekle ve `ai_usage_logs` tablosuna bir kayıt eklemekle yükümlüdür.
-   **Süreç:**
    1.  API çağrısı yapılır.
    2.  `usageMetadata`'dan `promptTokenCount` ve `candidatesTokenCount` alınır.
    3.  `config/ai.php` dosyasında tanımlanan, o anki model için geçerli olan token başına maliyet oranları okunur.
    4.  `estimated_cost_usd` hesaplanır.
    5.  Tüm bu veriler (`task_name`, `model_name`, token sayıları, maliyet vb.) `ai_usage_logs` tablosuna kaydedilir.
-   Bu mekanizma, Admin Panelindeki "AI Maliyet Merkezi" modülünü besleyecektir.

## 5. Kullanım Alanları (Use Cases)

Gemma 3, platformda aşağıdaki ana görevleri yerine getirmek için kullanılacaktır. Her görev, yukarıdaki loglama mekanizmasını tetikleyecektir.

### 5.1. Film İçerik Analizi ve Zenginleştirme
-   **Görev:** Her film için özgün ve derinlemesine analizler üretmek.
-   **`task_name` (Log için):** `analyze_movie_content`
-   **Süreç:** Film verileri (başlık, özet vb.) `PROMPT_ENGINEERING_GUIDE.md`'deki ilgili şablonla birleştirilir, API'ye gönderilir. Gelen JSON yanıtı `movie_analyses` tablosunda önbelleklenir. Çağrı loglanır.

### 5.2. Kişiselleştirilmiş Öneriler
-   **Görev:** "Haftalık Keşif Radarı" gibi kişisel film öneri listeleri oluşturmak.
-   **`task_name` (Log için):** `generate_weekly_discovery`
-   **Süreç:** Kullanıcının zevk profili, ilgili prompt şablonuyla birleştirilir, API'ye gönderilir. Gelen öneriler `weekly_recommendations` tablosuna kaydedilir. Çağrı loglanır.

### 5.3. Topluluk İçeriği Özeti
-   **Görev:** Bir filme yazılmış yorumları analiz edip, genel kanıyı özetlemek.
-   **`task_name` (Log için):** `summarize_community_reviews`
-   **Süreç:** Bir filme ait tüm yorum metinleri birleştirilip ilgili prompt ile API'ye gönderilir. Gelen özet metni `movie_analyses` tablosunda önbelleklenir. Çağrı loglanır.

### 5.4. Doğal Dil Arama
-   **Görev:** Kullanıcının serbest metin aramasını yapılandırılmış filtrelere dönüştürmek.
-   **`task_name` (Log için):** `natural_language_search`
-   **Süreç:** Kullanıcının arama sorgusu, ilgili prompt ile API'ye gönderilir. Gelen yapılandırılmış JSON, veritabanı sorgusunu oluşturmak için kullanılır. Çağrı loglanır.

Bu yapı, Gemma 3'ün gücünü projemizin her köşesine yayarken, aynı zamanda operasyonel kontrolü ve finansal öngörülebilirliği en üst düzeyde tutmamızı sağlar.
