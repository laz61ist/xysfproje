# Project Roadmap (Proje Yol Haritası)

**Document Version:** 1.1
**Last Updated:** 18.08.2025

## 1. Vizyon ve Amaç

Bu yol haritası, `Sinefil Radarı` projesinin gelişimini mantıksal fazlara ayırarak, özelliklerin önceliklendirilmesini ve projenin aşamalı olarak hayata geçirilmesini planlar. Amaç, her fazın sonunda kendi başına değerli ve kullanılabilir bir ürün (Minimum Viable Product - MVP) ortaya çıkarmak ve her yeni fazda bu temel üzerine inşa ederek platformu zenginleştirmektir.

---

## **Faz 1: Çekirdek Deneyim ve MVP (Core Experience & MVP)**

-   **Hedef:** Platformun temel film keşfetme ve bilgi edinme işlevlerini hayata geçirmek. Bu fazın sonunda, kullanıcılar siteye gelip filmleri arayabilir, detaylarını görebilir, fragmanlarını izleyebilir ve yapay zeka analizlerini okuyabilirler.
-   **Süre Tahmini:** 4-6 Hafta
-   **Özellikler:**
    -   **[Temel Altyapı]**
        -   Linux sunucu kurulumu ve temel konfigürasyon.
        -   Framework'süz PHP için MVC benzeri klasör yapısının oluşturulması.
        -   Supabase projesinin oluşturulması ve veritabanı şemasının (Faz 1 için gerekli tablolar) kurulması.
        -   `.env` tabanlı ortam yapılandırmasının hayata geçirilmesi.
    -   **[Film Keşfi]**
        -   TMDB API entegrasyonu (Film arama, film detayları, fragmanlar).
        -   Ana Sayfa (Popüler filmleri listeleyen).
        -   Film Detay Sayfası (Afiş, özet, oyuncular, fragman).
    -   **[Yapay Zeka Entegrasyonu - v1]**
        -   Gemma 3 API entegrasyonu (Merkezi `GemmaService`).
        -   Film Detay Sayfası için "Bu Film Kimin İçin?" ve "Beklentini Ayarla" analizlerinin otomatik üretilip önbelleklenmesi.
    -   **[Kullanıcı Sistemi - Temel]**
        -   Supabase Auth ile E-posta/Şifre ile Kullanıcı Kaydı ve Girişi.
        -   Temel Profil Sayfası (sadece username gösteren).
    -   **[Admin Paneli - v1]**
        -   Temel admin girişi ve kullanıcı listeleme.

---

## **Faz 2: Topluluk ve Etkileşim (Community & Engagement)**

-   **Hedef:** Kullanıcıyı pasif bir tüketiciden, platforma katkıda bulunan aktif bir üyeye dönüştürmek. Bu faz, kullanıcı tarafından oluşturulan içeriğin (UGC) temelini atar.
-   **Süre Tahmini:** 4-5 Hafta
-   **Özellikler:**
    -   **[Etkileşim Araçları]**
        -   Filmlere Puan Verme (1-5 yıldız).
        -   Filmlere Yorum Yazma (Spoiler korumalı).
        -   Filmleri Favorilere Ekleme ve Beğenme.
    -   **[Liste Yönetim Sistemi]**
        -   Kullanıcıların Public/Private listeler oluşturması.
        -   Listelere film ekleme/çıkarma.
        -   SEO dostu public liste sayfaları.
    -   **[Gelişmiş Profil]**
        -   Profil sayfalarında kullanıcının son yorumlarının ve public listelerinin gösterilmesi.
        -   Profil düzenleme (bio, avatar yükleme).
    -   **[Admin Paneli - v2]**
        -   Yorum ve liste yönetimi araçları.

---

## **Faz 3: Kişiselleştirme ve Oyunlaştırma (Personalization & Gamification)**

-   **Hedef:** Platformu her kullanıcı için benzersiz bir deneyime dönüştürmek ve uzun vadeli bağlılığı artırmak.
-   **Süre Tahmini:** 5-7 Hafta
-   **Özellikler:**
    -   **[Kişiselleştirme Motoru]**
        -   **Haftalık Keşif Radarı (Discover Weekly Radar)** algoritmasının geliştirilmesi ve otomatize edilmesi.
        -   Ana sayfada "Moduna Göre Listeler"in dinamik olarak gösterilmesi.
    -   **[Oyunlaştırma Sistemi]**
        -   Sine-Puan sisteminin entegrasyonu.
        -   Rozetler ve Başarımlar sisteminin kurulması.
        -   Liderlik Tabloları sayfası.
    -   **[Gelişmiş Topluluk]**
        -   Yorumlara yanıt verme ve yorumları beğenme.
    -   **[Yapay Zeka Entegrasyonu - v2]**
        -   Topluluk yorumlarını özetleyen "Community Consensus" özelliğinin geliştirilmesi.
        -   Doğal Dil Arama özelliğinin ilk versiyonu.

---

## **Faz 4: Monetizasyon ve Operasyonel Mükemmellik (Monetization & Operational Excellence)**

-   **Hedef:** Platformun finansal sürdürülebilirliğini sağlamak, operasyonel kontrolü artırmak ve viral büyümeyi teşvik edecek özellikler eklemek.
-   **Süre Tahmini:** 5-7 Hafta
-   **Özellikler:**
    -   **[Gelir Modelleri]**
        -   **"Nerede İzleyebilirim?"** motoru ve Affiliate link entegrasyonu.
        -   **Gönüllü Destekçi Modeli** (Buy Me a Coffee/Stripe entegrasyonu, reklamsız deneyim).
        -   Stratejik **Google AdSense** reklam alanlarının yerleştirilmesi.
    -   **[Operasyonel Kontrol] - (YENİ)**
        -   **AI Maliyet Takip Sistemi:** Her Gemma 3 API çağrısının token kullanımını ve maliyetini loglayan sistemin kurulması.
        -   **Admin Panelinde AI Maliyet Merkezi:** Maliyetleri görselleştiren (grafikler, tablolar) yeni admin modülünün geliştirilmesi.
    -   **[Büyüme Araçları]**
        -   **Sinefil Karnesi (Cinephile Wrapped)** yıllık özet özelliğinin geliştirilmesi.
        -   Tartışma Kulüpleri / Forumlar sisteminin ilk versiyonu.
        -   **İstek & Öneri** sistemi ve **Duyuru Panosu**.
    -   **[Admin Paneli - v3]**
        -   Gelir modelleri yönetim paneli.
        -   Kullanıcı raporlama ve moderasyon kuyruğu.

---

## **Gelecek (Future Roadmap)**

-   Dizi ve TV şovları için destek (Faz 5).
-   Gelişmiş sosyal özellikler (takip sistemi, aktivite akışı) (Faz 5).
-   Native Mobil Uygulamalar (iOS/Android) (Faz 6).
-   Daha derin Gemma 3 entegrasyonları (film karşılaştırma asistanı).
