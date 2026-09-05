# Faz 2: AI Canlı Destek & Ticket Entegrasyonu - Fonksiyonel Spesifikasyon

## 1. Modül Amacı
Web sitesine entegre RAG (Retrieval-Augmented Generation) tabanlı AI canlı destek widget'ı ile müşteri sorularını anlık yanıtlamak; AI'ın yetersiz kaldığı durumlarda sohbet verilerini toplayarak otomatik ticket oluşturmaktır.

---

## 2. İş Kuralları (Business Rules)

* **BR-01 (Güven Skoru Eşiği - %80):** AI üretilen yanıtın güven skorunu (Confidence Score) hesaplar. Skor $\ge \%80$ ise yanıt doğrudan müşteriye iletilir.
* **BR-02 (Otomatik Ticket Fallback):** Güven skoru $<\%80$ ise veya müşteri canlı temsilci talep ederse, AI chat akışından ayrılmadan "Destek Talebi Oluştur" seçeneği sunar.
* **BR-03 (Sohbet Verisi Aktarımı):** AI, sohbet geçmişinden Müşteri E-postası, Sipariş No, Talep Başlığı ve Açıklama verilerini otomatik çekerek **Phase 1 Ticket API**'sine (`POST /api/v1/tickets`) istek atar.
* **BR-04 (Bilgi Bankası Entegrasyonu):** AI canlı destek widget'ı; ürün katalogları, SSS (Sıkça Sorulan Sorular) ve kargo/sipariş durum servislerinden anlık beslenir.

---

**3. API Entegrasyon Detayı**

AI, ticket oluştururken Phase 1'deki POST /api/v1/tickets servisine aşağıdaki JSON yükünü otomatik iletir:
```json
{
  "title": "AI Chat - [Müşterinin Konusu]",
  "category": "Diğer",
  "description": "[Chat Geçmişi & Müşterinin Sorunu]",
  "order_number": "ORD-12345",
  "customer_email": "musteri@example.com",
  "source": "AI_Live_Chat"
}
