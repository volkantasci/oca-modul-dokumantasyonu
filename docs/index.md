# OCA Modül Dokümantasyonu

Odoo 19 Community altyapımızda muhasebe, banka/ödeme, analitik-bütçe, faturalama ve doküman yönetimi süreçlerini tamamlamak üzere kurulan **OCA (Odoo Community Association)** modüllerinin detaylı dokümantasyonu.

!!! info "Kapsam"
    **24 proje kartı**, toplam **37 modül** + bağımlılık sağlayıcı modüller. Tüm modüller 19.0 branch'lerinde doğrulandı, scratch veritabanlarında test edildi (PASS) ve canlı aktivasyon öncesi hazır durumda.

## Ortam Bilgileri

| Bileşen | Değer |
|---|---|
| Odoo Sürümü | 19.0 Community (Docker: `odoo:19.0`) |
| Canlı Instance | `odoo` — erp.netahavuz.com |
| Test Instance | `odoo-test` |
| Paket Yöneticisi | `oii` (odoo-installer 0.6.4) — `/home/odoo/deployments/.oii-venv` |
| GitHub Kuruluşu | [OCA](https://github.com/OCA) |

!!! note "Menü adları: Community'de \"Muhasebe\" uygulaması yok"
    Odoo **Community**'de ayrı bir *Muhasebe* uygulaması bulunmaz (Enterprise'daki `account_accountant` modülüne özeldir); muhasebe menüleri **Faturalama** uygulaması altında toplanır. Modül sayfalarında upstream README'den gelen *"Muhasebe → Raporlama"* gibi ifadeler, bu instance'ta **"Faturalama → Raporlama"** yoluna karşılık gelir. İleri muhasebe menüleri için kullanıcıda *Tam Muhasebe Özellikleri* grubu gerekebilir. Her modül aktive edildikçe gerçek menü yolu sahada doğrulanıp ilgili sayfada netleştirilir.

## Neden OCA Modülleri?

Odoo **Community** sürümü, Enterprise'da bulunan birçok finansal özelliği içermez:

- Gelişmiş uzlaştırma ekranı (bank reconciliation widget)
- Kurumsal finansal raporlar (genel defter, mizan, yaşlandırma)
- Sabit kıymet/amortisman yönetimi
- Toplu ödeme emirleri ve banka dosya üretimi
- MIS rapor tasarımcısı, bütçe yönetimi

OCA, bu boşlukları topluluk tarafından geliştirilen ve bakımı yapılan **AGPL-3 lisanslı** modüllerle doldurur. Modüllerin tamamı OCA'nın kalite süreçlerinden (review, test, CI) geçer.

## Kurulum Sırası (Özet)

Modüller **bağımlılık zinciri + risk** gözetilerek 6 grupta kurulur:

1. **Finansal Raporlama** — Genel raporlar, vergi bakiyeleri, vade listesi, cari ekstre
2. **Temel Yardımcılar** — Fiş şablonları, not şablonları, netleştirme, hizmet dönemleri, sabit kıymet, dönem kapanışı
3. **Banka ve Ödeme** — Ödeme modları → uzlaştırma → ekstre içe aktarma → ödeme emirleri → mandat → geri dönen ödemeler
4. **Analitik, Bütçe ve Raporlama** — Analitik kuralları → bütçe → MIS raporları
5. **Faturalama Akışları** — Vadeler, iskontolar, sevkiyattan fatura, otomatik faturalama
6. **Doküman Yönetimi** — Wiki ve iş talimatları

Detaylı sıra gerekçeleri ve aktivasyon adımları için [Kurulum Rehberi](kurulum-rehberi.md); modül tablosu için [Modül Matrisi](modul-matrisi.md).

## Kritik Uyarılar

!!! warning "Aktivasyon öncesi mutlaka okuyun"
    - **Beta olgunluktaki modüller** üretim öncesi test instance'ında (`odoo-test`) iş akışıyla birlikte denenmelidir.
    - **`partner_invoicing_mode` (queue_job)** için container'da `openupgradelib` python paketi gereklidir; pip ile kurulum container recreate'te **silinir** — kalıcı çözüm (özel imaj) hazırlanmadan o kart aktif edilmemelidir.
    - **SEPA modülleri** (sepa_credit_transfer, mandat) Avrupa bankacılık standardıdır; Türkiye'de doğrudan kullanılamaz — ödeme emri altyapısı için değerlidir, banka dosya formatı ihtiyaç halinde özelleştirilir.
    - Tüm modüller **canlı DB'ye oii ile kurulmadı**; aktivasyon, Apps ekranından **kullanıcı tarafından** yapılacaktır.

## Sayfa Yapısı

Her modül sayfası şu bölümleri içerir:

- **Teknik künye** — modül adı, repo, sürüm, olgunluk, lisans, bağımlılıklar
- **Nedir?** — modülün tanımı
- **Ne İşe Yarar?** — iş değeri ve kullanım senaryoları
- **Odoo'da Neleri Değiştirir?** — menüler, alanlar, ekranlar, raporlar, zamanlanmış işler
- **Nasıl Çalışır?** — süreç akışı
- **Kurulum ve Yapılandırma** — ayarlar, dikkat edilecekler
- **Kaynaklar** — GitHub, Runboat, hata takibi bağlantıları

!!! note "Yaşayan dokümantasyon"
    Bu dokümantasyon her modül kurulumu/aktivasyonu sonrasında sahadan gelen gerçek detaylarla zenginleştirilir: menü yerleri, otomatik kurulan bağımlılıklar, çeviri düzeltmeleri, yapılandırma adımları. Bağımlılık olarak gelen modüller (ör. `date_range`) "Bağımlılık Modülleri" bölümünde ayrıca belgelenir.
