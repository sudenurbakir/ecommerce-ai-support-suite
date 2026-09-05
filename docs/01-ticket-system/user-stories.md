# Faz 1: Web Destek Ticket Sistemi User Story

## Story 1: Form Üzerinden Destek Talebi (Ticket) Oluşturma
**Jira Key:** SUP-101  
**Epic:** Faz 1: Ticket Altyapısı  
**Issue Type:** Story  
**Priority:** High  

### Story Description
**As a** web sitesi ziyaretçisi (üye olan veya olmayan)  
**I want** destek formu üzerinden problemimi detaylandırıp dosya ekleyerek talep gönderebilmeyi  
**So that** yaşadığım sorunun hızlıca müşteri temsilcisine ulaşmasını ve tarafıma dönüş yapılmasını sağlamak.

### Pre-conditions (Ön Koşullar)
* Kullanıcının internet bağlantısının bulunması.
* Destek formunun web sitesinde erişilebilir olması (`/destek-talebi` URL'i).

### Functional Business Rules (İş Kuralları)
* **Zorunlu Alanlar:** Talep Başlığı, Kategori, Açıklama, E-posta Adresi.
* **Opsiyonel Alanlar:** Sipariş Numarası, İlgili Ürün, Öncelik Derecesi, Dosya/Görsel Eki.
* **Format Kontrolleri:** E-posta adresi geçerli regex formatında (`user@domain.com`) olmalıdır.
* **Dosya Kısıtlamaları:** Maksimum 3 adet dosya yüklenebilir. Kabul edilen formatlar: `.jpg`, `.png`, `.pdf`. Maksimum dosya boyutu: Dosya başı 5 MB.
* **ID Formatı:** Sistem başarılı kayıtta otomatik `TK-YYYY-XXXX` formatında ID üretir (Örn: `TK-2026-1001`).

**Acceptance Criteria**

**Senaryo 1**: Tüm zorunlu ve opsiyonel alanlar doldurularak başarılı ticket açılması
**Given** kullanıcı destek formu sayfasındadır.
**When** "Talep Başlığı" alanına "Kargom gecikti", "Kategori" seçiminde "Sipariş & Kargo", "Açıklama"alanına "3 gündür kargoya verilmedi", "E posta" alanına "test@example.com" girer.
**And** "Sipariş no" alanına "ord-932111" ve "Öncelik" seçimine "Yüksek" girip 2 MB'lık bir Pdf faturası ekler
**And** "Gönder" butonuna tıklar
**Then** Sistem veritabanına kaydı "Açık" statüsüne atar
**And** Benzersiz bir "TK-2026-1001" numarası oluşturur
**And** Ekranda "Talebiniz alınmıştır. Takip No: TK-2026-1001" onay mesajını gösterir
**And** "test@example.com" adresine onay ve takip linki içeren bir e-posta gönderir.

**Senaryo 2**: Zorunlu alanların boş bırakılması durumunda form doğrulaması (Validation Error)
**Given** Kullanıcı destek formu sayfasındadır
**When** "Talep Başlığı" ve "Açıklama" alanlarını doldurup "E-posta" alanını boş bırakır
**And** "Gönder" butonuna tıklar
**Then** Form gönderimi engellenmelidir
**And** "E-posta" alanının altında kırmızı renkte "Bu alanın doldurulması zorunludur" uyarısı çıkmalıdır.

**Senaryo 3**: Geçersiz e-posta formatı girilmesi
**Given** Kullanıcı destek formu sayfasındadır
**When** "E-posta" alanına "testemail.com" (at işareti olmadan) yazar ve formu gönderir
**Then** Form gönderilmemelidir
**And** "Lütfen geçerli bir e-posta adresi giriniz" hatası görüntülenmelidir.

**Senaryo 4**: Desteklenmeyen dosya formatı veya boyutu aşılan dosya yükleme denemesi
**Given** Kullanıcı destek formu sayfasındadır
**When** Dosya yükleme alanına 10 MB boyutunda bir `.exe` uzantılı dosya sürükler
**Then** Sistem dosyayı kabul etmemelidir
**And**** "Yalnızca JPG, PNG ve PDF formatında maksimum 5 MB boyutunda dosya yükleyebilirsiniz" uyarısı vermelidir.

**Story 2: Ticket No İle Üyesiz Durum Sorgulama ve Takip**
**Jira Key: SUP-102**
**Epic: Faz 1: Ticket Altyapısı**
**Issue Type: Story**
**Priority: High**

**Story Description**
**As a** destek talebi açmış olan müşteri
**I want** bana verilen Ticket Numarası ve E-posta adresim ile talebimin güncel durumunu sorgulayabilmeyi
**So that** üye girişi yapmadan temsilcinin verdiği yanıtı ve sürecin aşamasını görebilmeyi.

**Pre-conditions (Ön Koşullar)**
Müşterinin elinde sistem tarafından üretilmiş geçerli bir Ticket ID bulunması.

**Acceptance Criteria**

**Senaryo 1**: Doğru Ticket No ile başarılı durum sorgulama
**Given** Kullanıcı Ticket Sorgulama ekranındadır (`/ticket-sorgula`)
**When** "Ticket No" alanına "TK-2026-1001" yazar
**And** "Sorgula" butonuna basar
**Then** Sistem talebin mevcut durumunu (Örn: "İşlemde"), oluşturulma tarihini ve temsilcinin yazdığı son cevabı ekranda listelemelidir.

**Senaryo 2**: Sistemde bulunmayan veya hatalı Ticket No girilmesi
**Given** Kullanıcı Ticket Sorgulama ekranındadır
**When** "Ticket No" alanına sistemde kayıtlı olmayan "TK-0000-0000" numarasını girer
**And** "Sorgula" butonuna basar
**Then** Ekranda "Girdiğiniz Ticket Numarası ile eşleşen bir kayıt bulunamadı. Lütfen kontrol edip tekrar deneyiniz." uyarısı görüntülenmelidir.

**Senaryo 3**: Çözülmüş (Resolved) ticket sorgulama ve yeniden açma kısıtı
**Given** Müşterinin "TK-2026-1001" numaralı talebi panelde "Çözüldü" statüsündedir
**When** Müşteri bu ticket numarası ile sorgulama yaptığında
**Then** Durum etiketi yeşil renkte "Çözüldü" olarak görünmelidir
**And** Alt kısımda "Bu talep kapatılmıştır. Yeni bir sorununuz varsa lütfen yeni destek formu doldurunuz." bilgisi yer almalıdır.

**Story 3: Müşteri Temsilcisi Yanıtı ve Otomatik E-Posta Tetikleyici**
**Jira Key: SUP-103**
**Epic: Faz 1: Ticket Altyapısı**
**Issue Type: Story**
**Priority: Medium**

**Story Description**
**As a** destek sistemi altyapısı
**I want** Müşteri temsilcisi admin panelinden bir ticket'a yanıt verdiğinde müşteriye otomatik bilgilendirme e-postası göndermeyi
**So that** müşterinin yanıtı anında öğrenmesini ve iletişim kopukluğu yaşanmamasını sağlamak.


**Acceptance Criteria** 

**Senaryo 1**: Temsilci yanıtı sonrası e-posta tetiklenmesi (Outbound Email Trigger)
**Given** Müşteri temsilcisi admin panelinde "TK-2026-1001" numaralı talebe "Kargonuz bugün çıkış yapacaktır" yanıtını girer
**When** Temsilci "Yanıtla ve Durumu Güncelle" butonuna basıp statüyü "Müşteri Yanıtı Bekleniyor" yaparsa
**Then** Sistem müşterinin "test@example.com" e-posta adresine bir bildirim e-postası atar
**And** E-posta içeriğinde temsilcinin yanıt metni ve web sitesindeki detaylı takip linki yer almalıdır.
