# Project Vision and Requirements (Proje Vizyonu ve Gereksinimleri)

**Document Version:** 1.0
**Last Updated:** 18.08.2025

## 1. Proje Vizyonu

**Sinefil Radarı**, filmleri bir katalogdan ibaret görmeyen, bunun yerine her kullanıcı için kişisel bir sinema asistanı ve yaşayan bir dijital sinema kulübü olmayı hedefleyen, yapay zeka merkezli bir keşif ve topluluk platformudur. Temel amacımız, kullanıcının zevklerini derinlemesine öğrenerek onlara sadece sevecekleri filmleri sunmakla kalmayıp, aynı zamanda sinema tutkularını paylaşabilecekleri, tartışabilecekleri ve yeni bakış açıları kazanabilecekleri bir ortam yaratmaktır. Spotify'ın müzik keşfi için yaptığını, Sinefil Radarı sinema için yapacaktır.

## 2. Hedef Kitle

- **The Casual Viewer (Sıradan İzleyici):** "Bu akşam ne izlesem?" sorusuna hızlı ve isabetli yanıtlar arayan, karmaşık filtrelerle uğraşmak istemeyen kullanıcılar.
- **The Cinephile (Sinemasever):** Filmler hakkında derinlemesine analizler okumayı, teoriler üretmeyi, listeler oluşturmayı ve benzer zevklere sahip diğer kullanıcılarla etkileşim kurmayı seven tutkulu izleyiciler.
- **The Social Sharer (Sosyal Paylaşımcı):** Kendi film listelerini oluşturup arkadaşlarıyla paylaşmaktan, izleme alışkanlıklarını "Sinefil Karnesi" gibi araçlarla sergilemekten keyif alan kullanıcılar.

## 3. Temel Prensipler

- **AI-First Personalization:** Her özellik, kullanıcının deneyimini daha kişisel hale getirmek için yapay zeka (Gemma 3) potansiyelini merkeze almalıdır.
- **Community-Driven Content:** Platformun en değerli içeriği, kullanıcıların yorumları, listeleri ve tartışmaları olacaktır. Sistem, bu etkileşimi teşvik etmeli ve ödüllendirmelidir.
- **Frictionless Discovery to Watch:** Kullanıcının bir filmi keşfetmesi ile onu izleyebileceği platforma gitmesi arasındaki adımlar mümkün olan en aza indirilmelidir.
- **Sustainable & Unobtrusive Monetization:** Gelir modeli, kullanıcı deneyimini bozmamalı, aksine değeri artıran (reklamsız deneyim gibi) seçenekler sunmalıdır.

## 4. Fonksiyonel Gereksinimler (Functional Requirements)

### FR-AUTH: Kullanıcı Yönetimi
- **FR-AUTH-01:** Kullanıcılar e-posta veya sosyal medya hesapları (Google, Facebook vb.) ile kayıt olabilmeli ve giriş yapabilmelidir.
- **FR-AUTH-02:** Kullanıcıların kendilerine ait, düzenlenebilir bir profil sayfası olmalıdır (kullanıcı adı, profil resmi, kısa bio).
- **FR-AUTH-03:** Şifre sıfırlama ve hesap yönetimi gibi standart fonksiyonlar bulunmalıdır.

### FR-AI: Yapay Zeka Keşif Motoru
- **FR-AI-01:** Kullanıcıların doğal dilde (Örn: "mind-bending sci-fi movies like Inception") arama yapabilmesini sağlayan bir arama çubuğu olmalıdır.
- **FR-AI-02:** Her film detay sayfasında Gemma 3 tarafından üretilmiş özgün analizler ("Who is this movie for?", "Adjust Your Expectations") yer almalıdır.
- **FR-AI-03:** Her kullanıcı için, izleme geçmişine dayalı, haftalık olarak güncellenen kişisel bir "Discover Weekly" film listesi oluşturulmalıdır.
- **FR-AI-04:** Kullanıcıların ruh haline ve favori türlerine göre dinamik olarak oluşturulan "Mood Mixes" listeleri ana sayfada sunulmalıdır.
- **FR-AI-05:** Her yıl sonunda kullanıcıların izleme istatistiklerini özetleyen, görsel olarak zengin ve paylaşılabilir bir "Cinephile Wrapped" sayfası oluşturulmalıdır.
- **FR-AI-06:** Kullanıcı yorumları Gemma 3 tarafından analiz edilerek, "Community Consensus" (Topluluk Görüşü Özeti) bölümü oluşturulmalıdır.

### FR-COMMUNITY: Topluluk ve Etkileşim
- **FR-COMMUNITY-01:** Kullanıcılar filmlere 1-5 arası yıldızla puan verebilmelidir.
- **FR-COMMUNITY-02:** Kullanıcılar filmlere yorum yazabilmelidir. Yorum sistemi "Spoiler" etiketini desteklemelidir.
- **FR-COMMUNITY-03:** Kullanıcılar filmleri "Favori" listelerine ekleyebilmeli ve "Beğen" (Like) butonunu kullanabilmelidir.
- **FR-COMMUNITY-04:** Kullanıcılar sınırsız sayıda film listesi oluşturabilmelidir.
- **FR-COMMUNITY-05:** Oluşturulan her liste "Public" (Herkese Açık) veya "Private" (Özel) olarak ayarlanabilmelidir.
- **FR-COMMUNITY-06:** Public listeler ve yorumlar için sosyal medya paylaşım butonları bulunmalıdır.
- **FR-COMMUNITY-07:** Belirlenen filmler veya konular için özel "Discussion Forum" (Tartışma Forumu) alanları olmalıdır.

### FR-GAMIFY: Oyunlaştırma
- **FR-GAMIFY-01:** Kullanıcılar belirli eylemleri tamamladıkça ("10 yorum yaz", "5 liste oluştur" vb.) profil rozetleri kazanmalıdır.
- **FR-GAMIFY-02:** Kullanıcı etkileşimlerine dayalı bir puanlama sistemi ve haftalık/aylık liderlik tabloları olmalıdır.

### FR-WATCH: İzleme Entegrasyonu
- **FR-WATCH-01:** Her film sayfasında, filmin hangi streaming platformlarında (Netflix, Prime Video vb.) mevcut olduğunu gösteren bir "Where to Watch" bölümü olmalıdır.
- **FR-WATCH-02:** Bu bölümdeki linkler, affiliate (yönlendirme ortağı) kimliklerini içermeli ve kullanıcıyı doğrudan ilgili platformun film sayfasına yönlendirmelidir.

### FR-ADMIN: Yönetim Paneli
- **FR-ADMIN-01:** Kullanıcıları, yorumları ve listeleri yönetmek için kapsamlı bir admin paneli olmalıdır.
- **FR-ADMIN-02:** "Haftanın Filmi" gibi öne çıkan içerikleri belirleme yetkisi olmalıdır.
- **FR-ADMIN-03:** Reklam alanlarını ve affiliate partner linklerini yönetmek için bir arayüz bulunmalıdır.

## 5. Fonksiyonel Olmayan Gereksinimler (Non-Functional Requirements)

- **NFR-PERF-01:** Sayfa yükleme süreleri modern web standartlarına uygun olmalı, ana sayfa ve film detay sayfaları 2 saniyenin altında yüklenmelidir.
- **NFR-SEC-01:** Tüm kullanıcı verileri güvenli bir şekilde saklanmalı, şifreler hashlenmeli ve Supabase'in güvenlik standartlarından faydalanılmalıdır.
- **NFR-I18N-01:** Sitenin ana kullanıcı arayüzü, URL yapıları ve veritabanı şeması **İngilizce** olacaktır.
- **NFR-I18N-02:** Sistem, kullanıcı tarafından oluşturulan içeriklerde (yorumlar, liste adları vb.) Türkçe karakterleri de içeren tam **UTF-8** desteği sağlamalıdır.
- **NFR-UX-01:** Arayüz, mobil öncelikli (mobile-first) bir yaklaşımla tasarlanmalı ve tüm cihazlarda sorunsuz bir deneyim sunmalıdır.

## 6. Kapsam Dışı (Out of Scope for v1.0)

- Native iOS veya Android uygulamaları.
- Platform üzerinden doğrudan film kiralama veya satma.
- Kullanıcılar arası özel mesajlaşma.
- Dizi ve TV şovları (İlk odak sadece filmler olacaktır).
