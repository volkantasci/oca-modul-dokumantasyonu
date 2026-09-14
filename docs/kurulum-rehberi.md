# Kurulum Rehberi

Bu rehber, modüllerin hangi sırayla ve nasıl devreye alınacağını açıklar.

## Kurulum Akışı (oii)

Modüller `oii` aracıyla üç aşamada hazırlanır:

```bash
source /home/odoo/deployments/.oii-venv/bin/activate

# 1) Repoyu instance'a ekle (klonlar, bağımlılıkları çözer, mount eder)
oii module add <repo> --instance odoo --apply

# 2) Scratch DB'de test et (canlı DB'ye dokunmaz, PASS kaydeder)
oii module test <modul> --with l10n_generic_coa,sale --instance odoo

# 3) Canlı aktivasyon: UI'dan yapılır (oii install canlı DB'de kullanılmaz)
#    Apps > Update Apps List > modülü bul > Activate
```

!!! note "Test komutu hakkında"
    `--with l10n_generic_coa,sale` parametresi, scratch DB'ye genel hesap planı ve satış modülünü ekler; muhasebe modüllerinin kendi testlerinin sağlıklı çalışması için gereklidir. Test PASS olduğunda modül `~/.config/odoo-installer/tested.toml` whitelist'ine kaydedilir.

## Kurulum Sırası ve Gerekçeleri

### 1. Finansal Raporlama
| Sıra | Kart | Modül |
|---|---|---|
| 1 | Genel Finansal Raporlar | `account_financial_report` |
| 2 | Vergi Bakiyeleri | `account_tax_balance` |
| 3 | Vade Listesi | `account_due_list` |
| 4 | Cari Hesap Ekstresi | `partner_statement` |

**Gerekçe:** Bağımsız, davranış değiştirmeyen, anında fayda sağlayan raporlama modülleri. Genel raporlar, sonraki tüm süreçlerin doğrulama aracıdır. Raporlama altyapısı (`date_range`, `report_xlsx`) bu grupta gelir ve sonraki gruplarca kullanılır.

### 2. Temel Yardımcılar
| Sıra | Kart | Modül(ler) |
|---|---|---|
| 5 | Tekrarlayan Fiş Şablonları | `account_move_template` |
| 6 | Belge Not Şablonları | `account_comment_template` |
| 7 | Karşılıklı Netleştirme | `account_netting` |
| 8 | Hizmet Dönemi Tarihleri | `account_invoice_start_end_dates` |
| 9 | Sabit Kıymet ve Amortisman | `account_asset_management` |
| 10 | Dönem/Yıl Kapanışı | `account_fiscal_year_closing` |

**Gerekçe:** Muhasebe operasyonunu düzenli yürütmek için kullanılan yardımcılar. Sabit kıymet, dönem kapanışından **önce** kurulur (kapanış öncesi amortisman fişleri). Kapanış sihirbazı en sonda; çünkü diğer modüllerin ürettiği fişleri de kapsar.

### 3. Banka ve Ödeme
| Sıra | Kart | Modül(ler) |
|---|---|---|
| 11 | Ödeme Modları | `account_payment_mode` (+`_sale`, `_purchase`) |
| 12 | Uzlaştırma Ekranı | `account_reconcile_oca` (+`account_statement_base`) |
| 13 | Ekstre İçe Aktarma | `account_statement_import_camt`, `_sheet_file_xlsx` |
| 14 | Ödeme Emirleri | `account_payment_order`, `account_banking_pain_base`, `account_banking_sepa_credit_transfer` |
| 15 | Bankacılık Talimatları | `account_banking_mandate` (+`_sale`) |
| 16 | Geri Dönen Ödemeler | `account_payment_return`, `_import_iso20022` |

**Gerekçe (sıra kritik):**
- `account_payment_mode` tüm ödeme ailesinin temelidir — önce kurulur.
- `account_statement_import_base` modülü `account_statement_base`'e bağımlıdır → **uzlaştırma (12), ekstre içe aktarmadan (13) önce kurulmalıdır**; tersi durumda içe aktarma kurulamaz.
- `account_payment_order` ve `account_banking_pain_base` → SEPA ve mandat modüllerinin ön koşuludur.
- Geri dönen ödeme içe aktarma modülü, bank-payment'taki `account_payment_order`'a bağımlıdır.

### 4. Analitik, Bütçe ve Raporlama
| Sıra | Kart | Modül(ler) |
|---|---|---|
| 17 | Analitik Dağıtım Kuralları | `account_analytic_required`, `purchase_analytic`, `stock_analytic` |
| 18 | Bütçe Yönetimi | `account_budget_oca` |
| 19 | MIS Rapor Tasarımcısı | `mis_builder`, `mis_builder_budget` |

**Gerekçe:** Bütçe satırları analitik hesaplara dayanır; MIS raporları da analitik verilerle zenginleşir. Analitik kurallar önce, bütçe ve yönetim raporları sonra kurulur.

### 5. Faturalama Akışları
| Sıra | Kart | Modül(ler) |
|---|---|---|
| 20 | Gelişmiş Vadeler | `account_payment_term_extension` |
| 21 | İskonto Yönetimi | `account_global_discount`, `account_invoice_triple_discount` |
| 22 | Sevkiyattan Fatura | `stock_picking_invoicing` |
| 23 | Gruplama + Otomatik Faturalama | `sale_order_invoicing_grouping_criteria`, `partner_invoicing_mode` |

**Gerekçe:** Bunlar faturalama davranışını değiştirir; test instance'ında senaryo testleri sonrası canlıya alınmalıdır. `partner_invoicing_mode` cron ile otomatik fatura ürettiği için en dikkatli kurulum grubudur.

### 6. Doküman Yönetimi
| Sıra | Kart | Modül(ler) |
|---|---|---|
| 24 | Wiki ve İş Talimatları | `document_page`, `document_knowledge`, `mgmtsystem`, `document_page_work_instruction` |

**Gerekçe:** Muhasebe süreçlerinden bağımsızdır; en son kurulur. Sıralama: `document_knowledge → document_page → mgmtsystem → document_page_work_instruction`.

## UI'dan Aktivasyon Adımları

1. `erp.netahavuz.com` → **Apps** menüsü
2. Sağ üstten **Update Apps List** (geliştirici modu gerekmez; gerekirse `--dev=all` yerine Admin > Update Apps List yeterlidir)
3. Modül adını arayın (ör. `account_financial_report`)
4. **Activate** butonuna basın
5. Bağımlılıklar otomatik kurulur (OCA mount'ları addons_path'te hazır)

!!! tip "Toplu aktivasyon"
    Aynı gruptaki modüller sırayla aktive edilebilir. Kurulum sırası canlıda da korunmalıdır; özellikle Banka ve Ödeme grubunda (12 → 13) sıra önemlidir.

## Aktivasyon Sonrası Yapılandırma Kontrol Listesi

| Modül | Yapılandırma |
|---|---|
| `account_financial_report` | Ayarlar → Faturalama → OCA Yaşlandırma konfigürasyonu (dinamik aralıklar) |
| `partner_statement` | Ayarlar → Faturalama → Ortak Ekstresi seçenekleri |
| `account_payment_mode` | Faturalama → Yapılandırma → Ödeme Modları |
| `account_statement_import_sheet_file_xlsx` | Ekstre kolon eşleme şablonları (bankaya göre) |
| `account_asset_management` | Amortisman profilleri, hesap eşlemeleri |
| `account_fiscal_year_closing` | Kapanış şablonları |
| `account_analytic_required` | Hesap bazında analitik politikası (opsiyonel/önerilen/zorunlu) |
| `partner_invoicing_mode` | Geliştirici modu → Zamanlanmış İşlemler → 'Generate Standard Invoices' aktifleştirme + sıklık |
| `mis_builder` | MIS rapor şablonları ve dönem örnekleri |
| `account_global_discount` | Ayarlar → Parametreler → Genel İskontolar + ortak kartı iskonto alanları |

## Sorun Giderme

| Belirti | Sebep | Çözüm |
|---|---|---|
| `HTTP 403` (add sırasında) | GitHub API rate limit (60/saat) | Repoyu `git clone --depth 1 --branch 19.0` ile elle klonlayıp `oii module add <repo> --repo <dizin> --apply` kullanın |
| `modules not found ... (available: …)` | Repo sparse klonlanmış, modül checkout'ta yok | `oii module remove <repo> --apply`, dizini silin, `oii module add <repo> --apply` ile tam klonlayın |
| Test FAIL: `No journal could be found` | Scratch DB'de hesap planı yok | `--with l10n_generic_coa,sale` parametresini kullanın |
| Test FAIL: `external dependency ... openupgradelib` | queue_job python bağımlılığı | `docker exec odoo-web-1 pip install --break-system-packages openupgradelib` (recreate sonrası tekrar gerekir) |
| Modül Apps'te görünmüyor | Container yeni mount'u almamış | `docker compose up -d web` (oii add normalde otomatik yapar) |

## Sürüm Yükseltme

```bash
source /home/odoo/deployments/.oii-venv/bin/activate
pip install --no-cache-dir --upgrade "odoo-installer==<sürüm>"
oii version
```

!!! warning
    `pip install --upgrade odoo-installer` bazen en son sürüme yükseltmez; sürümü açıkça sabitleyin.
