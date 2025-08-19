# Sinefil Radarı

**Kişisel Sinema Asistanınız: Akıllı Filtrelerle Film Keşfet, Tartış ve Paylaş.**

Sinefil Radarı, standart film veritabanlarının ötesine geçerek, Google Gemma 3 yapay zeka modelinin gücünü kullanan, kullanıcı odaklı, kişiselleştirilmiş bir film keşif ve topluluk platformudur. Amacımız, "bu akşam ne izlesem?" sorusuna en kişisel ve isabetli cevapları vermek ve sinemaseverleri bir araya getiren yaşayan bir ekosistem oluşturmaktır.

## ✨ Temel Özellikler

-   **Yapay Zeka Destekli Keşif:** Doğal dilde arama, kişiselleştirilmiş "Haftalık Keşif" listeleri ve her film için özgün AI analizleri.
-   **Derinlemesine Etkileşim:** Puanlama, spoiler korumalı yorumlar, favorileme ve beğenme.
-   **Kürasyon ve Paylaşım:** Herkese açık veya özel, sınırsız sayıda film listesi oluşturma ve sosyal medyada paylaşma.
-   **Oyunlaştırma:** Platformdaki aktivitelerle "Sine-Puan" ve "Rozetler" kazanma, liderlik tablolarında yükselme.
-   **Anında İzleme:** "Nerede İzleyebilirim?" motoru ile filmleri yasal platformlarda anında bulma ve izlemeye başlama.
-   **Topluluk:** Tartışma kulüpleri, kullanıcı geri bildirimleri ve yenilikleri duyuran bir pano.

## 🛠️ Teknik Yapı (Tech Stack)

-   **Backend:** Framework'süz PHP (Modern, MVC benzeri yapılandırılmış yaklaşım)
-   **Veritabanı & BaaS:** **Supabase** (PostgreSQL, Auth, Storage)
-   **Yapay Zeka (AI) Çekirdeği:** **Google Gemma 3**
-   **Harici API'lar:** The Movie Database (TMDB)
-   **Deployment:** Git tabanlı otomatik dağıtım (CI/CD) ile Linux Sunucu.

---

## 📂 Proje Dokümantasyonu

Bu proje hakkında daha derinlemesine bilgi edinmek için lütfen aşağıdaki dokümanları inceleyin. Bu dokümanlar, projenin "anayasası" niteliğindedir.

### **I. Ürün ve Vizyon**
1.  **[Proje Vizyonu ve Gereksinimleri](PROJECT_VISION_AND_REQUIREMENTS.md):** Neyi, neden ve kimin için inşa ediyoruz?
2.  **[Proje Yol Haritası](PROJECT_ROADMAP.md):** Nereye gidiyoruz ve hangi adımlarla? (Faz 1'den 6'ya)
3.  **[Görev Listeleri (TODOs)](TODO_FAZ1.md):** Projeyi hayata geçirmek için adım adım görevler.

### **II. Mimari ve Teknik Tasarım**
4.  **[Mimari Genel Bakış](ARCHITECTURE_OVERVIEW.md):** Sistemimizin parçaları birbiriyle nasıl konuşuyor?
5.  **[Veritabanı Şeması](DATABASE_SCHEMA.md):** Verilerimizi nasıl yapılandırıyoruz?
6.  **[Ortam Yapılandırması ve URL Stratejisi](ENVIRONMENT_CONFIG_AND_URL_STRATEGY.md):** Farklı ortamlarda (yerel/canlı) nasıl sorunsuz çalışıyoruz?
7.  **[Yardımcı Betikler (Utility Scripts)](UTILITY_SCRIPTS.md):** `warmup.php` gibi komut satırı araçlarımız nasıl çalışıyor?

### **III. Çekirdek Entegrasyonlar ve Güvenlik**
8.  **[Üçüncü Parti API Sözleşmeleri](THIRD_PARTY_API_CONTRACTS.md):** TMDB ve diğer dış servislerle olan "sözleşmemiz".
9.  **[Gemma 3 Entegrasyon Planı](GEMMA3_INTEGRATION_BLUEPRINT.md):** Yapay zeka beynimiz nasıl çalışıyor?
10. **[Prompt Mühendisliği Rehberi](PROMPT_ENGINEERING_GUIDE.md):** Yapay zeka ile nasıl "konuşuyoruz"?
11. **[Güvenlik ve Erişim Kontrolü](SECURITY_AND_ACCESS_CONTROL.md):** Platformu ve kullanıcılarımızı (RLS ile) nasıl koruyoruz?

### **IV. Özellik Spesifikasyonları**
12. **[Kullanıcı Kimlik Doğrulama ve Profilleri](USER_AUTHENTICATION_AND_PROFILES.md):** Üyelik sistemi nasıl işliyor?
13. **[Temel Film Özellikleri](CORE_FILM_FEATURES_SPEC.md):** Puanlama, yorum, favori vb.
14. **[Liste Yönetim Sistemi](LIST_MANAGEMENT_SYSTEM_SPEC.md):** Kullanıcı listeleri nasıl çalışıyor?
15. **[Kişiselleştirme Algoritmaları](PERSONALIZATION_ALGORITHMS.md):** "Haftalık Keşif" ve diğer akıllı özellikler.
16. **[Oyunlaştırma Sistemi](GAMIFICATION_SYSTEM.md):** Puanlar, rozetler ve liderlik tabloları.
17. **[Topluluk Özellikleri ve Denetleme](COMMUNITY_FEATURES_AND_MODERATION.md):** Forumlar ve raporlama sistemi.
18. **[Gelir Modeli Stratejisi](MONETIZATION_STRATEGY.md):** Proje nasıl sürdürülebilir olacak?

### **V. Operasyonel Planlar**
19. **[Kullanıcı Arayüzü/Deneyimi Akışı ve Taslakları](UI_UX_FLOW_AND_WIREFRAMES.md):** Site nasıl görünecek ve hissettirecek?
20. **[Yönetici Paneli Spesifikasyonu](ADMIN_DASHBOARD_SPECS.md):** Projeyi nasıl yöneteceğiz?
21. **[Dağıtım ve Operasyon Planı](DEPLOYMENT_AND_OPERATIONS_PLAN.md):** Projeyi nasıl canlıya alıp bakımını yapacağız?

---

## 🚀 Yerel Kurulum (Lokal Windows Ortamı)

Projeyi yerel makinenizde çalıştırmak için aşağıdaki adımları izleyin.

### Gereksinimler
-   [XAMPP](https://www.apachefriends.org/tr/index.html) veya benzeri bir Apache/PHP/MySQL ortamı.
-   [Composer](https://getcomposer.org/) (PHP paket yöneticisi).
-   [Git](https://git-scm.com/).

### Kurulum Adımları

1.  **Projeyi Klonlayın:**
    ```bash
    git clone https://github.com/kullanici-adiniz/sinefil-radari.git
    cd sinefil-radari
    ```

2.  **PHP Bağımlılıklarını Yükleyin:**
    ```bash
    composer install
    ```

3.  **Ortam Değişkenlerini Ayarlayın:**
    -   Proje kök dizininde `.env.example` dosyasını kopyalayıp `.env` adında yeni bir dosya oluşturun.
    -   `.env` dosyasını açıp gerekli tüm değerleri (Supabase, TMDB, Gemma anahtarları ve `BASE_URL`) kendi yerel ayarlarınız ile güncelleyin.

4.  **Apache Sanal Host (Virtual Host) Ayarı (Önerilir):**
    -   Apache `httpd-vhosts.conf` dosyanızı düzenleyerek projenin `public` klasörünü kök dizin olarak ayarlayın. Bu, `http://sinefil-radari.test` gibi temiz bir URL kullanmanızı sağlar.

5.  **Başlatma:**
    -   XAMPP kontrol panelinden Apache servisini başlatın.
    -   Tarayıcınızdan belirlediğiniz `BASE_URL` adresine gidin.

## 🤝 Katkıda Bulunma

Katkılarınız projenin gelişimi için çok değerlidir. Lütfen `CONTRIBUTING.md` (gelecekte oluşturulacak) dosyasını inceleyerek sürece dahil olun.
