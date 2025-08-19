# Database Schema (Veritabanı Şeması)

**Document Version:** 1.2
**Last Updated:** 18.08.2025
**Platform:** Supabase (PostgreSQL)

## 1. Genel Bakış ve Kurallar

-   **İsimlendirme:** Tüm tablo ve sütun adları İngilizce, küçük harfle ve kelimeler arasında alt çizgi (`_`) kullanılarak (`snake_case`) yazılacaktır.
-   **Birincil Anahtarlar (Primary Keys):** Tüm tablolar, `id` adında, `UUID` tipinde bir birincil anahtara sahip olacaktır (istisna: `genres` ve ara tablolar).
-   **Zaman Damgaları (Timestamps):** Kritik tablolarda `created_at` ve `updated_at` adında, `timestamp with time zone` tipinde sütunlar bulunacaktır.
-   **İlişkiler (Relationships):** Tablolar arası ilişkiler yabancı anahtarlar (foreign keys) ile kurulacaktır.
-   **Row Level Security (RLS):** Tüm tablolarda RLS varsayılan olarak **aktif** edilecektir.

## 2. Tablo Şemaları

### `users`
Kullanıcı profillerini ve Supabase Authentication ile senkronize verileri saklar.

| Sütun Adı (Column) | Veri Tipi (Type) | Kısıtlamalar (Constraints) | Açıklama (Description) |
| :----------------- | :---------------- | :------------------------- | :----------------------------- |
| `id` | `uuid` | Primary Key, `auth.users.id`'ye referans verir | Supabase Auth kullanıcısının ID'si. |
| `username` | `text` | Unique, Not Null | Kullanıcının sitede görünecek benzersiz adı. |
| ... | ... | ... | ... |
| `updated_at` | `timestamptz` | Not Null, Default `now()` | Profilin son güncellenme tarihi. |

---

### `movies`
TMDB'den alınan ve sistemimizde önbelleklenen (cached) film meta verilerini saklar.

| Sütun Adı (Column) | Veri Tipi (Type) | Kısıtlamalar (Constraints) | Açıklama (Description) |
| :----------------- | :---------------- | :------------------------- | :----------------------------- |
| `id` | `uuid` | Primary Key | Bizim sistemimizdeki benzersiz ID. |
| `tmdb_id` | `integer` | Unique, Not Null | The Movie Database'deki benzersiz film ID'si. |
| `title` | `text` | Not Null | Filmin orijinal adı. |
| ... | ... | ... | ... |
| `updated_at` | `timestamptz` | Not Null, Default `now()` | Kaydın son güncellenme tarihi. |

---

### `genres` - (YENİ)
TMDB'den çekilen tüm film türlerini (kategorilerini) saklar.

| Sütun Adı (Column) | Veri Tipi (Type) | Kısıtlamalar (Constraints) | Açıklama (Description) |
| :----------------- | :---------------- | :------------------------- | :----------------------------- |
| `id` | `integer` | Primary Key | TMDB'den gelen orijinal tür ID'si. |
| `name` | `text` | Unique, Not Null | Türün adı (örn: "Action", "Science Fiction"). |
| `slug` | `text` | Unique, Not Null | URL'de kullanılacak format (örn: "action", "science-fiction"). |

---

### `movie_genres` - (YENİ)
Filmler ve türler arasındaki çoktan çoğa (many-to-many) ilişkiyi kuran ara tablo.

| Sütun Adı (Column) | Veri Tipi (Type) | Kısıtlamalar (Constraints) | Açıklama (Description) |
| :----------------- | :---------------- | :------------------------- | :----------------------------- |
| `movie_id` | `uuid` | Foreign Key -> `movies.id` | Filmin ID'si. |
| `genre_id` | `integer` | Foreign Key -> `genres.id` | Türün ID'si. |
| *Constraint* | `PRIMARY KEY(movie_id, genre_id)` | | Bir film bir türe sadece bir kez eklenebilir. |

---

### `movie_interactions`
Kullanıcıların filmlerle olan etkileşimlerini (puan, favori, beğeni) saklar.
*(Bu tabloda değişiklik yoktur)*

| Sütun Adı (Column) | Veri Tipi (Type) | Kısıtlamalar (Constraints) | Açıklama (Description) |
| :----------------- | :---------------- | :------------------------- | :----------------------------- |
| `id` | `uuid` | Primary Key | Etkileşimin benzersiz ID'si. |
| ... | ... | ... | ... |
| `updated_at` | `timestamptz` | Not Null, Default `now()` | Etkileşimin son güncellenme tarihi. |

---

### `comments`
Kullanıcıların filmlere yazdığı yorumları saklar.
*(Bu tabloda değişiklik yoktur)*

| Sütun Adı (Column) | Veri Tipi (Type) | Kısıtlamalar (Constraints) | Açıklama (Description) |
| :----------------- | :---------------- | :------------------------- | :----------------------------- |
| `id` | `uuid` | Primary Key | Yorumun benzersiz ID'si. |
| ... | ... | ... | ... |
| `updated_at` | `timestamptz` | Not Null, Default `now()` | Yorumun son güncellenme tarihi. |

---

### `lists`
Kullanıcıların oluşturduğu film listelerini saklar.
*(Bu tabloda değişiklik yoktur)*
| ... | ... | ... | ... |

---

### `list_items`
Bir listede hangi filmlerin olduğunu belirten ara tablo.
*(Bu tabloda değişiklik yoktur)*
| ... | ... | ... | ... |

---

### `badges`
Oyunlaştırma sistemi için kazanılabilecek rozetleri tanımlar.
*(Bu tabloda değişiklik yoktur)*
| ... | ... | ... | ... |

---

### `user_badges`
Kullanıcıların hangi rozetleri kazandığını belirten ara tablo.
*(Bu tabloda değişiklik yoktur)*
| ... | ... | ... | ... |

---

### `ai_usage_logs`
Yapay zeka API çağrılarının kullanımını ve maliyetini takip eder.
*(Bu tabloda değişiklik yoktur)*

| Sütun Adı (Column) | Veri Tipi (Type) | Kısıtlamalar (Constraints) | Açıklama (Description) |
| :----------------- | :---------------- | :------------------------- | :----------------------------- |
| `id` | `uuid` | Primary Key | Log kaydının benzersiz ID'si. |
| ... | ... | ... | ... |
| `created_at` | `timestamptz` | Not Null, Default `now()` | Çağrının yapıldığı an. |
