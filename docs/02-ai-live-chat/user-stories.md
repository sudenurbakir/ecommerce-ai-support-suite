# Faz 2: AI Canlı Destek - Detaylı User Story'ler & Acceptance Criteria

## Story 1: AI Yanıt Veremediğinde Otomatik Ticket Oluşturma
**Jira Key:** SUP-201  
**Epic:** Faz 2: AI Live Chat  
**Issue Type:** Story  
**Priority:** High  

### Story Description
**As a** canlı destek kullanan web sitesi müşterisi  
**I want** AI sorumu çözemediğinde sohbet ekranından çıkmadan destek talebi oluşturabilmeyi  
**So that** aynı bilgileri tekrar yazmak zorunda kalmadan müşteri temsilcisine ulaşabilmeyi.

### Pre-conditions (Ön Koşullar)
* Müşterinin AI Canlı Destek widget'ı ile aktif bir sohbet oturumunun bulunması.
* Phase 1 Ticket API servisinin çalışır durumda olması.

### Functional Business Rules (İş Kuralları)
* **Güven Skoru Kontrolü:** AI yanıt üretirken güven skoru %80'in altındaysa cevabı doğrudan vermek yerine kullanıcıya yönlendirme sunar.
* **Otomatik Veri Çekme:** Chatbot, konuşma geçmişinden e-posta, sipariş numarası ve özet sorunu ayıklayarak ticket formunu otomatik doldurur.

### Acceptance Criteria

**Senaryo 1**: Güven skoru %80 altında kaldığında otomatik ticket seçeneği sunulması
**Given** Müşteri AI Canlı Destek Widget'ı ile mesajlaşmaktadır
**When** AI'ın ürettiği yanıtın güven skoru %80'in altına düştüğünde
**Then** AI ekranda "Talebinizi destek ekibimize iletmek ister misiniz?" seçeneğini ve "Destek Talebi Oluştur" butonunu sunmalıdır
**And** Müşteri "Destek Talebi Oluştur" butonuna bastığında AI, sohbetten topladığı verilerle /api/v1/tickets adresine POST isteği atmalıdır
**And** Üretilen Ticket No'yu (örn: TK-2026-1002) sohbet penceresinde müşteriye göstermelidir.

**Senaryo 2**: Yüksek güven skorunda yanıtın doğrudan iletilmesi
**Given** Müşteri AI Canlı Destek Widget'ı üzerinden kargo durumunu sormaktadır
**When** AI'ın yanıt güven skoru %80 ve üzerinde olduğunda
**Then** AI yanıtı doğrudan sohbet ekranında müşteriye sunmalı ve ticket oluşturma seçeneği çıkarmamalıdır.

**Senaryo 3**: Müşterinin sohbet içinde canlı temsilci talep etmesi
**Given** Müşteri AI Canlı Destek Widget'ı içerisindedir
**When** Müşteri chat ekranına "Temsilciye bağlanmak istiyorum" yazar
**Then** AI güven skoruna bakmaksızın sohbeti sonlandırıp "Sizi müşteri temsilcimize aktarabilmem için bir destek talebi oluşturuyorum" mesajını vermeli
**And** Otomatik ticket kaydını tetiklemelidir.
