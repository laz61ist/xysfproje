# Personalization Algorithms Specification (Kişiselleştirme Algoritmaları Spesifikasyonu)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Felsefe ve Amaç

Bu doküman, Sinefil Radarı'nı statik bir film kataloğundan, her kullanıcı için yaşayan, nefes alan ve kişisel bir deneyime dönüştüren algoritmik sistemlerin mantığını ve işleyişini tanımlar. Temel felsefemiz, kullanıcının platformda bıraktığı her izi (puan, favori, liste, yorum) öğrenme fırsatı olarak kullanarak, ona sadece seveceği içerikleri sunmak değil, aynı zamanda sinema zevkini genişletecek sürpriz keşifler yaptırmaktır. Bu sistemlerin kalbinde, veri analizi ve Google Gemma 3 yapay zekasının yaratıcı metin üretme yetenekleri yer alır.

## 2. Veri Kaynakları (User Taste Profile)

Tüm kişiselleştirme algoritmaları, her kullanıcı için dinamik olarak oluşturulan bir "Zevk Profili"nden beslenir. Bu profil aşağıdaki sinyalleri içerir:

-   **Açık Sinyaller (Explicit Signals - En Yüksek Ağırlık):**
    -   Yüksek Puan Verilen Filmler (`rating >= 4`).
    -   Favorilere Eklenen Filmler (`is_favorite = true`).
    -   Kullanıcının Oluşturduğu Listelere Eklenen Filmler.
-   **Dolaylı Sinyaller (Implicit Signals - Orta Ağırlık):**
    -   Beğenilen Filmler (`is_liked = true`).
    -   Yorum Yapılan Filmler.
    -   Detaylı olarak incelenen (sayfasında uzun süre kalınan) filmlerin türleri ve anahtar kelimeleri.
-   **Negatif Sinyaller (Negative Signals - Dışlama İçin):**
    -   Düşük Puan Verilen Filmler (`rating <= 2`).

## 3. Algoritmik Özellikler

### 3.1. Haftalık Keşif Radarı (Discover Weekly Radar)

-   **Amaç:** Kullanıcıya her hafta, yüksek ihtimalle seveceği ancak muhtemelen daha önce duymadığı veya izlemediği filmlerden oluşan bir liste sunmak. Bu, platformun "imza" özelliği olacaktır.
-   **Tetiklenme:** Haftada bir (örneğin Pazartesi sabahı) her kullanıcı için çalışan, zamanlanmış bir görev (Cron Job).
-   **Algoritma Akışı:**
    1.  **Profil Çıkarma:** Sistem, kullanıcının "Zevk Profili"ni yukarıdaki veri kaynaklarından oluşturur. Özellikle en sevdiği 5-10 film, favori türleri ve yönetmenleri belirlenir.
    2.  **Aday Havuzu Oluşturma:**
        -   Kullanıcının en sevdiği filmlere "benzer" olan filmler (TMDB "similar movies" API'si veya kendi etiket sistemimiz üzerinden) bulunur.
        -   Kullanıcının favori yönetmenlerinin veya oyuncularının diğer filmleri havuza eklenir.
    3.  **Filtreleme:** Aday havuzundan, kullanıcının daha önce puanladığı, yorum yaptığı veya herhangi bir listesine eklediği filmler **çıkarılır**.
    4.  **Gemma 3 ile Kürasyon:** Kalan filtrelenmiş aday listesi (örneğin 20-30 film), kullanıcının zevk profili ile birlikte `PROMPT_ENGINEERING_GUIDE.md`'de tanımlanan `generate_weekly_discovery` prompt şablonu kullanılarak Gemma 3'e gönderilir.
    5.  **Sonuç:** Gemma 3, bu havuzdan en uygun 5-10 filmi seçer ve **her biri için neden önerildiğine dair kişiselleştirilmiş bir metin** üretir.
    6.  **Kaydetme:** Bu sonuç, o haftanın "Keşif Radarı" olarak kullanıcıya özel bir tabloda saklanır ve ana sayfasında gösterilir.

### 3.2. Moduna Göre Listeler (Mood Mixes)

-   **Amaç:** Kullanıcının ana sayfasında, o anki ruh haline veya spesifik ilgi alanlarına yönelik, dinamik olarak oluşturulmuş film koleksiyonları sunmak.
-   **Tetiklenme:** Kullanıcı ana sayfayı her ziyaret ettiğinde (sonuçlar bir süre önbelleklenebilir).
-   **Algoritma Akışı:**
    1.  **Etiket Analizi:** Sistem, kullanıcının Zevk Profili'ndeki filmlerin "Akıllı Etiketlerini" (`smart_tags` - Gemma 3 tarafından üretilen) analiz eder. En sık tekrar eden etiketler belirlenir (örn: "dystopian-future", "character-driven", "slow-burn").
    2.  **Liste Oluşturma:** Sistem, bu popüler etiketlere dayalı olarak listeler oluşturur. Örneğin:
        -   **"Beyin Yakan Filmler":** İçinde "complex-plot", "mind-bending", "sci-fi" etiketleri geçen ve kullanıcının sevdiği filmler ile onlara benzer yeni keşifleri bir araya getirir.
        -   **"Hafta Sonu Gerilimi":** İçinde "thriller", "mystery", "suspense" etiketleri geçen filmleri birleştirir.
    3.  **Sunum:** Bu dinamik listeler, "Sadece Sana Özel" başlığı altında ana sayfada gösterilir.

### 3.3. Sinefil Karnesi (Cinephile Wrapped)

-   **Amaç:** Her yıl sonunda, kullanıcının yıllık izleme alışkanlıklarını özetleyen, görsel olarak çekici, eğlenceli ve sosyal medyada paylaşılmaya teşvik eden bir deneyim sunmak.
-   **Tetiklenme:** Her yılın sonunda (örneğin Aralık ayında) çalışan özel bir betik (script).
-   **Hesaplanacak Metrikler:**
    1.  **Toplam İzleme Süresi:** Yıl içinde puanlanan filmlerin toplam süresi.
    2.  **En Çok İzlenen Tür:** Puanlanan filmlerin tür dağılımı.
    3.  **En Çok İzlenen Yönetmen/Oyuncu:** Puanlanan filmlerin yönetmen ve oyuncu istatistikleri.
    4.  **En Yüksek Puan Verilen Ay:** Aylara göre film puanlama aktivitesi.
    5.  **"Gizli Hazine" Keşfi:** Kullanıcının yüksek puan verdiği ancak platformda genel olarak az popüler olan bir film.
-   **Gemma 3 ile Kişiselleştirme:** Bu istatistiksel veriler, Gemma 3'e gönderilerek her metrik için esprili ve kişisel bir metin üretmesi istenir.
    -   **Örnek Prompt:** "Kullanıcının en çok izlediği tür 'Romantik Komedi'. Bu veriye dayanarak ona 'Görünüşe göre 2025'te kalbinin sesini dinlemişsin!' gibi esprili bir metin yaz."
-   **Sunum:** Sonuçlar, animasyonlu, tam sayfa bir web deneyimi olarak sunulur ve her slayt için bir "Paylaş" butonu içerir. Bu sayfa, SEO ve Google Ads için **kapsam dışı** olacaktır; amacı tamamen kullanıcı etkileşimi ve viral pazarlamadır.
