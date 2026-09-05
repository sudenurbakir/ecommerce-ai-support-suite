# E-Commerce Support & AI Automation Suite

E-ticaret ve pazaryeri sistemleri için tasarlanmış **Yapay Zeka Destekli Müşteri Hizmetleri Otomasyonu** iş analizi dokümantasyon projesidir.

---

## Sistem Modülleri

### 1. Temel Destek Sistemi (Phase 1)
* **Kapsam:** Üyesiz destek formu doldurma, Ticket ID ile durum sorgulama ve e-posta bildirimleri.
* **Çıktılar:** Veri modeli, durum geçişleri (State Machine), Jira User Story'leri.

### 2. AI Canlı Destek & Fallback (Phase 2)
* **Kapsam:** RAG tabanlı AI canlı destek widget'ı.
* **İş Kuralı:** Yanıt güven skoru **<%80** ise sohbet verileriyle otomatik ticket oluşturur.

### 3. Pazaryeri AI Soru-Cevap (Phase 3)
* **Kapsam:** Trendyol, Hepsiburada vb. platformlardan gelen soruların otomasyonu.
* **İş Kuralı:** Güven skoru **≥%85** ise otomatik yanıtlanır; düşükse admin "Manuel Onay" havuzuna düşer.

---

## Klasör Mimarisi

```text
ecommerce-ai-support-suite/
├── README.md                      # Proje özeti
├── docs/                          # İş Analizi Dokümanları
│   ├── 01-ticket-system/          # Faz 1 Spec & User Story
│   ├── 02-ai-live-chat/           # Faz 2 AI Fallback Spec & User Story
│   └── 03-marketplace-qa/         # Faz 3 Pazaryeri Entegrasyon Spec
├── diagrams/                      # Process & Sequence Flows (Mermaid.js)
├── api/                           # OpenAPI 3.0 (Swagger) YAML
└── database/                      # SQL Veritabanı Şemaları (DDL)
