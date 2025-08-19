# User Authentication and Profiles (Kullanıcı Kimlik Doğrulama ve Profilleri)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Genel Bakış

Bu doküman, Sinefil Radarı platformundaki kullanıcıların kayıt olma, giriş yapma, oturumlarını yönetme ve profillerini düzenleme süreçlerini tanımlar. Mimarinin temeli, güvenli, ölçeklenebilir ve yönetimi kolay bir çözüm sunan **Supabase Auth** hizmetine dayanmaktadır. Amacımız, geliştirme sürecini hızlandırmak ve kullanıcı güvenliğini en üst düzeyde tutmak için Supabase'in yerleşik özelliklerinden maksimum düzeyde faydalanmaktır.

## 2. Kimlik Doğrulama Akışı (Authentication Flow)

### 2.1. Kullanıcı Kaydı (Sign Up)

1.  **Arayüz:** Kullanıcı, Kayıt Ol (`/signup`) sayfasındaki forma e-posta adresini, bir şifre ve bir `username` (kullanıcı adı) girer.
2.  **İstemci (Frontend):** Form gönderildiğinde, istemci tarafı JavaScript, Supabase'in `supabase.auth.signUp()` fonksiyonunu çağırır. Bu fonksiyona `email` ve `password` verilir.
    ```javascript
    const { data, error } = await supabase.auth.signUp({
      email: 'user@example.com',
      password: 'example-password',
      options: {
        // 'data' alanı, custom kullanıcı verilerini saklamak için kullanılır.
        // Kayıt sırasında alınan username'i buraya ekliyoruz.
        data: { 
          username: 'new_user_123'
        }
      }
    });
    ```
3.  **Supabase Auth:** Supabase, `auth.users` tablosuna yeni bir kullanıcı kaydı ekler. `options.data` içinde gönderilen `username`, bu kullanıcının `raw_user_meta_data` alanına JSON olarak kaydedilir. Aynı zamanda, kullanıcıya bir doğrulama e-postası gönderir (bu ayar Supabase panelinden aktif edilmelidir).
4.  **Veritabanı Tetikleyicisi (Database Trigger):** `auth.users` tablosuna yeni bir kayıt eklendiğinde, bu olayı dinleyen bir PostgreSQL trigger (tetikleyici) otomatik olarak çalışır.
5.  **Profil Oluşturma:** Bu trigger, public `users` tablomuzda yeni bir satır oluşturur. `id`'yi yeni Auth kullanıcısının ID'sinden, `username`'i ise `raw_user_meta_data` alanından alır.
    ```sql
    -- Örnek Trigger Fonksiyonu
    create function public.handle_new_user()
    returns trigger as $$
    begin
      insert into public.users (id, username)
      values (new.id, new.raw_user_meta_data->>'username');
      return new;
    end;
    $$ language plpgsql security definer;

    -- Trigger'ı oluşturma
    create trigger on_auth_user_created
      after insert on auth.users
      for each row execute procedure public.handle_new_user();
    ```
6.  **Sonuç:** Kullanıcı hem `auth.users` (güvenli) hem de `public.users` (profil) tablolarında oluşturulmuş olur ve doğrulama e-postasını bekler.

### 2.2. E-posta Doğrulaması (Email Confirmation)

-   Kullanıcı, aldığı e-postadaki linke tıklayarak hesabını doğrular. Bu işlem Supabase tarafından otomatik olarak yönetilir.
-   Doğrulama yapılana kadar, RLS (Row Level Security) politikaları kullanıcının platformda içerik oluşturmasını (yorum yapma, liste oluşturma vb.) engelleyebilir.

### 2.3. Kullanıcı Girişi (Sign In)

1.  **Arayüz:** Kullanıcı, Giriş Yap (`/signin`) sayfasındaki forma e-posta ve şifresini girer.
2.  **İstemci (Frontend):** Form gönderildiğinde, `supabase.auth.signInWithPassword()` fonksiyonu çağrılır.
3.  **Supabase Auth:** Kimlik bilgilerini doğrular. Başarılı olursa, bir **JWT (JSON Web Token)** içeren bir oturum (session) nesnesi döndürür.
4.  **Oturum Yönetimi:** Supabase client kütüphanesi, bu oturum bilgilerini tarayıcının `localStorage`'ında güvenli bir şekilde saklar. Sonraki tüm API istekleri (hem Supabase'e hem de bizim backend'imize) bu JWT'yi otomatik olarak `Authorization` başlığında gönderir.

### 2.4. Oturumun Korunması ve Çıkış (Session Management & Sign Out)

-   Kullanıcı sayfayı yenilediğinde veya tekrar ziyaret ettiğinde, Supabase client kütüphanesi `localStorage`'daki oturumu kontrol eder ve kullanıcıyı otomatik olarak giriş yapmış sayar.
-   Çıkış yapmak için `supabase.auth.signOut()` fonksiyonu çağrılır. Bu, `localStorage`'daki oturum bilgilerini temizler.

### 2.5. Sosyal Medya ile Giriş (OAuth Providers)

-   Supabase panelinden Google, GitHub, Facebook vb. sağlayıcılar kolayca aktif edilebilir.
-   Arayüzde "Google ile Giriş Yap" butonu, `supabase.auth.signInWithOAuth({ provider: 'google' })` fonksiyonunu tetikler.
-   Geri kalan akış (kullanıcı ve profil oluşturma) Supabase tarafından yönetilir.

## 3. Kullanıcı Profilleri (User Profiles)

### 3.1. Profil Sayfası (`/profile/{username}`)

-   Her kullanıcının, `users` tablosundaki verileri gösteren, herkese açık bir profil sayfası olacaktır.
-   Bu sayfada kullanıcının `username`, `avatar_url`, `bio`, herkese açık (`is_public = true`) listeleri ve son aktiviteleri (yorumlar, puanlamalar) listelenir.

### 3.2. Profil Düzenleme (`/settings/profile`)

-   Kullanıcılar sadece kendi profillerini düzenleyebilir. Bu, `SECURITY_AND_ACCESS_CONTROL.md` dosyasında tanımlanan RLS politikaları ile garanti altına alınır.
-   Kullanıcı bu sayfada `full_name`, `bio` gibi alanları güncelleyebilir.
-   **Profil Resmi Yükleme:**
    1.  Kullanıcı bir resim dosyası seçer.
    2.  İstemci tarafı JavaScript, Supabase'in **Storage** hizmetini kullanarak resmi `avatars` adında bir "bucket"a yükler. Dosya adı, çakışmaları önlemek için kullanıcının `id`'si ile ilişkilendirilir (örn: `user_id.png`).
    3.  Yükleme başarılı olduğunda, Supabase Storage dosyanın public URL'ini döndürür.
    4.  Bu URL, Supabase client'ı kullanılarak kullanıcının `users` tablosundaki `avatar_url` sütununa kaydedilir.

Bu yapı, modern, güvenli ve bakımı kolay bir kullanıcı yönetim sistemi kurmamızı sağlar ve geliştirme odağımızı platformun özgün özelliklerine kaydırmamıza olanak tanır.
