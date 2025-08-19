# Security and Access Control (Güvenlik ve Erişim Kontrolü)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Güvenlik Felsefesi

Sinefil Radarı'nın güvenlik stratejisi, **"varsayılan olarak reddet" (deny by default)** prensibine dayanır. Bu, hiçbir kullanıcının (anonim veya giriş yapmış) açıkça izin verilmemiş hiçbir veriye erişememesi, onu değiştirememesi veya silememesi anlamına gelir. Güvenliği uygulama katmanına (PHP) bırakmak yerine, doğrudan veritabanı seviyesinde, **Supabase Row Level Security (RLS)** politikaları ile zorunlu kılacağız. Bu, olası uygulama katmanı açıklarında bile verinin güvende kalmasını sağlar.

## 2. Anahtar ve Sır Yönetimi (Secret Management)

-   **Asla Koda Yazma:** `SUPABASE_SERVICE_ROLE_KEY`, `GEMMA_API_KEY`, `TMDB_API_KEY` gibi tüm hassas anahtarlar ve sırlar, asla Git repomuza commit edilmeyecek olan `.env` dosyasında saklanacaktır.
-   **İstemci Tarafı Anahtarları:** Tarayıcıya (frontend) gönderilecek tek anahtar, Supabase'in `ANON_KEY`'idir. Bu anahtar public'tir ve sadece RLS politikalarının izin verdiği verileri okumasına izin verir.
-   **Sunucu Tarafı Anahtarları:** `SERVICE_ROLE_KEY` gibi tüm RLS politikalarını baypas edebilen güçlü anahtarlar, **SADECE** sunucu tarafında (PHP backend) kullanılacaktır. Bu anahtarlar istemciye asla gönderilmez.

## 3. Kullanıcı Kimlik Doğrulama (Authentication)

-   Tüm kullanıcı kimlik doğrulama işlemleri (kayıt, giriş, şifre sıfırlama, sosyal medya girişi) için Supabase'in yerleşik **Auth** hizmeti kullanılacaktır.
-   Supabase Auth, güvenli JWT (JSON Web Tokens) yönetimi, şifre hashleme (bcrypt), e-posta doğrulaması ve siber saldırılara karşı koruma (rate limiting) gibi endüstri standardı güvenlik önlemlerini bizim için otomatik olarak yönetir.

## 4. Yetkilendirme ve Erişim Kontrolü: Row Level Security (RLS) Politikaları

RLS, veritabanı güvenliğimizin kalbidir. Her tablo için, hangi kullanıcının hangi satırları görebileceğini, ekleyebileceğini, güncelleyebileceğini veya silebileceğini tanımlayan SQL kuralları (politikalar) yazılacaktır.

Aşağıda, temel tablolar için uygulanacak RLS politikalarının bir özeti bulunmaktadır.

---

### **`users` Tablosu RLS Politikaları**

-   **SELECT (Okuma):** Herkes (anonim kullanıcılar dahil) tüm kullanıcı profillerini okuyabilir. (Public profiller için).
    ```sql
    CREATE POLICY "Public profiles are viewable by everyone."
    ON users FOR SELECT USING (true);
    ```
-   **UPDATE (Güncelleme):** Bir kullanıcı **sadece kendi** profilini güncelleyebilir.
    ```sql
    CREATE POLICY "Users can update their own profile."
    ON users FOR UPDATE USING (auth.uid() = id);
    ```
-   **INSERT / DELETE:** Kullanıcı ekleme ve silme işlemleri Supabase Auth tarafından yönetildiği için genellikle doğrudan tabloya INSERT veya DELETE politikasına gerek yoktur.

---

### **`lists` Tablosu RLS Politikaları**

-   **SELECT (Okuma):** Herkes `is_public` sütunu `true` olan listeleri görebilir. Bir kullanıcı, ek olarak kendi özel (`is_public = false`) listelerini de görebilir.
    ```sql
    CREATE POLICY "Public lists are viewable by everyone."
    ON lists FOR SELECT USING (is_public = true);

    CREATE POLICY "Users can view their own private lists."
    ON lists FOR SELECT USING (auth.uid() = user_id);
    ```
-   **INSERT (Ekleme):** Giriş yapmış bir kullanıcı yeni bir liste ekleyebilir.
    ```sql
    CREATE POLICY "Users can create new lists."
    ON lists FOR INSERT WITH CHECK (auth.uid() = user_id);
    ```
-   **UPDATE (Güncelleme):** Bir kullanıcı **sadece kendi** listesini güncelleyebilir.
    ```sql
    CREATE POLICY "Users can update their own lists."
    ON lists FOR UPDATE USING (auth.uid() = user_id);
    ```
-   **DELETE (Silme):** Bir kullanıcı **sadece kendi** listesini silebilir.
    ```sql
    CREATE POLICY "Users can delete their own lists."
    ON lists FOR DELETE USING (auth.uid() = user_id);
    ```

---

### **`comments` Tablosu RLS Politikaları**

-   **SELECT (Okuma):** Herkes tüm yorumları okuyabilir.
    ```sql
    CREATE POLICY "Comments are viewable by everyone."
    ON comments FOR SELECT USING (true);
    ```
-   **INSERT (Ekleme):** Giriş yapmış bir kullanıcı yeni bir yorum ekleyebilir.
    ```sql
    CREATE POLICY "Users can create new comments."
    ON comments FOR INSERT WITH CHECK (auth.uid() = user_id);
    ```
-   **UPDATE (Güncelleme):** Bir kullanıcı **sadece kendi** yorumunu güncelleyebilir.
    ```sql
    CREATE POLICY "Users can update their own comments."
    ON comments FOR UPDATE USING (auth.uid() = user_id);
    ```
-   **DELETE (Silme):** Bir kullanıcı **sadece kendi** yorumunu silebilir. (Alternatif olarak, site moderatörleri için de bir silme politikası eklenebilir).
    ```sql
    CREATE POLICY "Users can delete their own comments."
    ON comments FOR DELETE USING (auth.uid() = user_id);
    ```

---

### **`movie_interactions` Tablosu RLS Politikaları**

-   **SELECT (Okuma):** Herkes tüm etkileşimleri okuyabilir (Bir filmin ortalama puanını hesaplamak için).
    ```sql
    CREATE POLICY "Interactions are viewable by everyone."
    ON movie_interactions FOR SELECT USING (true);
    ```
-   **INSERT/UPDATE/DELETE:** Bir kullanıcı **sadece kendi** etkileşim kaydını (`user_id` kendi `auth.uid()`'sine eşit olan) oluşturabilir, güncelleyebilir veya silebilir. Bu genellikle tek bir politikada birleştirilebilir.
    ```sql
    CREATE POLICY "Users can manage their own interactions."
    ON movie_interactions FOR ALL
    USING (auth.uid() = user_id)
    WITH CHECK (auth.uid() = user_id);
    ```

## 5. Uygulama Katmanı Güvenliği (PHP Backend)

Veritabanı güvenliğine ek olarak, uygulama katmanında da standart güvenlik önlemleri alınacaktır.

-   **XSS (Cross-Site Scripting) Koruması:** Kullanıcıdan gelen ve ekrana basılacak olan tüm veriler (yorumlar, liste adları vb.) `htmlspecialchars()` gibi fonksiyonlardan geçirilerek XSS saldırıları engellenecektir.
-   **CSRF (Cross-Site Request Forgery) Koruması:** Durum değiştiren tüm formlarda (liste silme, profil güncelleme vb.) CSRF token'ları kullanılarak yetkisiz istekler engellenecektir.
-   **SQL Injection:** Supabase'in client kütüphaneleri ve REST API'si kullanıldığı için bu risk büyük ölçüde ortadan kalkmıştır. Ancak, doğrudan SQL sorgusu yazılması gereken nadir durumlarda mutlaka parametreli sorgular (prepared statements) kullanılacaktır.

Bu çok katmanlı güvenlik yaklaşımı, Sinefil Radarı'nı hem harici tehditlere hem de dahili hatalara karşı dirençli hale getirecektir.
