# Gamification System Specification (Oyunlaştırma Sistemi Spesifikasyonu)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Felsefe ve Amaç

Bu doküman, Sinefil Radarı platformuna entegre edilecek oyunlaştırma mekaniklerini tanımlar. Oyunlaştırmanın temel amacı, kullanıcıların platformdaki doğal etkileşimlerini (film puanlama, yorum yapma, liste oluşturma vb.) eğlenceli ve ödüllendirici hedeflere dönüştürerek **kullanıcı bağlılığını (engagement) ve kalıcılığını (retention)** artırmaktır. Sistem, kullanıcıyı rekabete zorlamak yerine, kişisel başarımlarını ve sinema tutkusundaki uzmanlığını sergilemesine olanak tanıyarak onu motive etmeyi hedefler.

## 2. Sistem Bileşenleri

Oyunlaştırma sistemi üç ana bileşenden oluşur:

1.  **Sine-Puanlar (Cine-Points):** Kullanıcının platformdaki her anlamlı aktivite için kazandığı deneyim puanları.
2.  **Rozetler (Badges):** Belirli kilometre taşlarına veya özel görevlere ulaşıldığında kazanılan görsel başarımlar.
3.  **Liderlik Tabloları (Leaderboards):** En aktif ve bilgili kullanıcıları onurlandıran sıralamalar.

## 3. Veri Modeli Referansı

Bu sistem, `DATABASE_SCHEMA.md` dosyasında tanımlanan şu tablolara dayanır:
-   `badges`: Sistemdeki tüm olası rozetlerin tanımlarını içerir.
-   `user_badges`: Hangi kullanıcının hangi rozeti ne zaman kazandığını kaydeder.
-   `users` tablosuna `cine_points` adında bir `integer` sütun eklenerek her kullanıcının toplam puanı saklanacaktır.

## 4. Bileşenlerin Detayları

### 4.1. Sine-Puanlar (Cine-Points)

-   **Amaç:** Kullanıcının platforma yaptığı her katkıyı ölçülebilir bir değere dönüştürmek.
-   **Puan Kazanma Eylemleri:**
    -   Bir filme puan verme: **+2 Puan**
    -   Bir filme yorum yazma: **+5 Puan**
    -   Yeni bir liste oluşturma: **+10 Puan**
    -   Bir listeye film ekleme: **+1 Puan** (liste başına max 20 puan)
    -   Yorumunun başka bir kullanıcı tarafından beğenilmesi (ileride eklenecek bir özellik): **+1 Puan**
-   **Teknik Uygulama:** Bu eylemleri gerçekleştiren backend fonksiyonları, ilgili kullanıcıya puan ekleyen bir `addPoints(userId, amount)` servisini çağıracaktır. Bu işlem, veritabanı transaction'ları içinde güvenli bir şekilde yapılmalıdır.
-   **Gösterim:** Kullanıcının toplam Sine-Puanı, profil sayfasında ve kullanıcı avatarının yanında belirgin bir şekilde gösterilir.

### 4.2. Rozetler (Badges)

-   **Amaç:** Kullanıcılara ulaşmaları için somut hedefler sunmak ve profillerini kişiselleştirmelerine olanak tanımak.
-   **Rozet Kategorileri ve Örnekleri:**

    | Kategori | Rozet Adı | Kazanma Koşulu |
    | :--- | :--- | :--- |
    | **Başlangıç (Onboarding)** | First Critic (İlk Eleştirmen) | İlk yorumunu yazdığında. |
    | | First Review (İlk Puan) | İlk filmini puanladığında. |
    | | Curator (Küratör) | İlk listesini oluşturduğunda. |
    | **Etkileşim (Engagement)** | Cinephile (Sinemasever) | Toplamda 50 filme puan verdiğinde. |
    | | Film Critic (Film Eleştirmeni) | Toplamda 25 yorum yazdığında. |
    | | Prolific Lister (Üretken Listeleyici) | 10 farklı liste oluşturduğunda. |
    | **Uzmanlık (Expertise)** | Horror Aficionado (Korku Meraklısı) | 25 korku filmine puan verdiğinde. |
    | | Sci-Fi Specialist (Bilim Kurgu Uzmanı) | 25 bilim kurgu filmine puan verdiğinde. |
    | | Nolan Fanatic (Nolan Hayranı) | Christopher Nolan'ın tüm filmlerini puanladığında. |
    | **Topluluk (Community)** | Hidden Gem Hunter (Gizli Hazine Avcısı) | Platformda 1000'den az puana sahip bir filme yorum yazdığında. |
    | | Trendsetter (Trend Belirleyici) | Oluşturduğu bir liste 50 favoriye ulaştığında (ileride). |

-   **Teknik Uygulama:**
    1.  Tüm olası rozetler ve kazanma koşulları `badges` tablosuna önceden tanımlanır.
    2.  Kullanıcı puan veya yorum ekleme gibi bir eylem gerçekleştirdiğinde, backend'de bir "başarım kontrol servisi" (`AchievementCheckService`) tetiklenir.
    3.  Bu servis, kullanıcının son eyleminin bir rozet kazanma koşulunu tetikleyip tetiklemediğini kontrol eder (örn: "Kullanıcının toplam yorum sayısı 25'e ulaştı mı?").
    4.  Koşul sağlanmışsa ve kullanıcı o rozete daha önce sahip değilse, `user_badges` tablosuna yeni bir kayıt eklenir.
    5.  Kullanıcıya "Yeni bir rozet kazandın: Film Eleştirmeni!" şeklinde bir bildirim gösterilir.
-   **Gösterim:** Kazanılan rozetler, kullanıcının profil sayfasında özel bir bölümde sergilenir. Kullanıcı, en çok gurur duyduğu 3-5 rozeti profilinin en üstünde sergilemek için seçebilir.

### 4.3. Liderlik Tabloları (Leaderboards)

-   **Amaç:** Platformun en aktif ve bilgili üyelerini onurlandırmak ve diğer kullanıcılara keşfedecekleri ilginç profiller sunmak.
-   **Sıralama Türleri:**
    -   **Haftalık Liderlik Tablosu:** O hafta en çok Sine-Puan kazanan kullanıcılar. Her hafta sıfırlanır.
    -   **Aylık Liderlik Tablosu:** O ay en çok Sine-Puan kazanan kullanıcılar. Her ay sıfırlanır.
    -   **Tüm Zamanlar Liderlik Tablosu:** Platform açıldığından beri en çok Sine-Puan biriktirmiş olan kullanıcılar ("Hall of Fame").
-   **Teknik Uygulama:** Bu tablolar, `users` tablosundaki `cine_points` sütununa göre `ORDER BY` ve `LIMIT` komutları kullanılarak kolayca oluşturulabilir. Performansı artırmak için sonuçlar belirli bir süre (örn: 1 saat) önbellekte tutulmalıdır.
-   **Gösterim:** Platformda, ilk 20 veya 50 kullanıcıyı gösteren özel bir `/leaderboards` sayfası bulunur. Bu, yeni kullanıcıların platformun "power user"larını keşfetmesi için de bir yol sunar.

Bu sistem, kullanıcıları platforma sadece içerik tüketmek için değil, aynı zamanda katkıda bulunmak, başarı elde etmek ve topluluğun bir parçası olmak için geri dönmeye teşvik edecektir.
