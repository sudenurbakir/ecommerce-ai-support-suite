# Faz 1: Web Destek Ticket Sistemi - Fonksiyonel Spesifikasyon

## 1. Modül Amacı
Müşterilerin web sitesi üzerinden üye olmadan destek talebi (ticket) oluşturabilmesini, kendilerine verilen `ticket_id` ile durum sorgulayabilmesini ve e-posta bildirimleri alabilmesini sağlayan altyapıdır.

---

## 2. İş Kuralları (Business Rules)

* **BR-01 (Üyesiz Erişim):** Kullanıcıların ticket oluşturmak veya sorgulamak için üye girişi yapma zorunluluğu yoktur.
* **BR-02 (Benzersiz ID):** Her ticket için sistem otomatik numara üretir (Format: `TK-YYYY-XXXX`, örn: `TK-2026-1001`).
* **BR-03 (Varsayılan Statü):** Form gönderildiği an ticket statüsü `Açık (Open)` olarak set edilir.
* **BR-04 (E-posta Tetikleyici):** Ticket oluşturulduğunda ve temsilci yanıt verdiğinde müşteriye e-posta bildirimi gider.
* **BR-05 (Zorunlu Alanlar):** `Talep Başlığı`, `Kategori`, `Açıklama` ve `E-posta` zorunlu; `Sipariş No` ve `Dosya` opsiyoneldir.

---

## 3. Veri Alanları

| Alan Adı | Veri Tipi | Zorunlu mu? | Açıklama / Seçenekler |
| :--- | :--- | :--- | :--- |
| `ticket_id` | String | Evet (Sistem) | Benzersiz Takip No (`TK-2026-XXXX`) |
| `title` | Varchar(150) | Evet | Talep Başlığı |
| `category` | Enum | Evet | `Sipariş & Kargo`, `Fatura`, `Teknik Sorun`, `Diğer` |
| `description` | Text | Evet | Detaylı Açıklama |
| `order_number` | Varchar(50) | Hayır | Sipariş Numarası |
| `priority` | Enum | Evet | `Düşük`, `Normal`, `Yüksek`, `Acil` (Varsayılan: `Normal`) |
| `status` | Enum | Evet (Sistem) | `Açık`, `İşlemde`, `Yanıt Bekliyor`, `Çözüldü` |
| `customer_email` | Varchar(100) | Evet | Müşteri E-Posta Adresi |
| `attachment_url` | String | Hayır | Dosya/Görsel Bağlantısı |

---

## 4. Durum Geçişleri (State Machine)

```text
[ Form Gönderildi ] ──► ( Açık ) ──► ( İşlemde ) ──┬──► ( Yanıt Bekliyor )
                                                   └──► ( Çözüldü )
