# Modül Matrisi

24 proje kartının modül, sürüm ve test durumu özeti. Tüm testler scratch veritabanında yapılmış ve **PASS** olarak kaydedilmiştir (12.09.2026).

## Özet Tablo

| # | Kart (Proje) | Modül(ler) | OCA Deposu | Sürüm | Olgunluk | Lisans |
|---|---|---|---|---|---|---|
| 1 | Genel Finansal Raporlar | account_financial_report | [account-financial-report](https://github.com/OCA/account-financial-report) | 19.0.0.0.21 | Beta | AGPL-3 |
| 2 | Vergi Bakiyeleri | account_tax_balance | [account-financial-report](https://github.com/OCA/account-financial-report) | 19.0.1.0.3 | Mature | AGPL-3 |
| 3 | Vade Listesi | account_due_list | [account-payment](https://github.com/OCA/account-payment) | 19.0.1.0.0 | Production | AGPL-3 |
| 4 | Cari Hesap Ekstresi | partner_statement | [account-financial-report](https://github.com/OCA/account-financial-report) | 19.0.1.1.0 | Beta | AGPL-3 |
| 5 | Tekrarlayan Fiş Şablonları | account_move_template | [account-financial-tools](https://github.com/OCA/account-financial-tools) | 19.0.1.0.0 | Beta | AGPL-3 |
| 6 | Belge Not Şablonları | account_comment_template | [account-invoice-reporting](https://github.com/OCA/account-invoice-reporting) | 19.0.1.0.0 | Beta | AGPL-3 |
| 7 | Karşılıklı Netleştirme | account_netting | [account-financial-tools](https://github.com/OCA/account-financial-tools) | 19.0.1.0.0 | Beta | AGPL-3 |
| 8 | Hizmet Dönemi Tarihleri | account_invoice_start_end_dates | [account-closing](https://github.com/OCA/account-closing) | 19.0.1.0.0 | Beta | AGPL-3 |
| 9 | Sabit Kıymet ve Amortisman | account_asset_management | [account-financial-tools](https://github.com/OCA/account-financial-tools) | 19.0.1.0.3 | Mature | AGPL-3 |
| 10 | Dönem/Yıl Kapanışı | account_fiscal_year_closing | [account-closing](https://github.com/OCA/account-closing) | 19.0.1.0.0 | Beta | AGPL-3 |
| 11 | Ödeme Modları | account_payment_mode + _sale/_purchase | [bank-payment](https://github.com/OCA/bank-payment) | 19.0.1.1.0 | Mature | AGPL-3 |
| 12 | Uzlaştırma Ekranı | account_reconcile_oca | [account-reconcile](https://github.com/OCA/account-reconcile) | 19.0.1.0.9 | Beta | AGPL-3 |
| 13 | Ekstre İçe Aktarma | account_statement_import_camt, _sheet_file_xlsx | [bank-statement-import](https://github.com/OCA/bank-statement-import) | 19.0.1.0.0 / 19.0.2.0.0 | Beta | AGPL-3 |
| 14 | Ödeme Emirleri | account_payment_order, account_banking_pain_base, account_banking_sepa_credit_transfer | [bank-payment](https://github.com/OCA/bank-payment) | 19.0.1.0.5 / 1.1.1 / 1.0.0 | Mature/Beta | AGPL-3 |
| 15 | Bankacılık Talimatları | account_banking_mandate + _sale | [bank-payment](https://github.com/OCA/bank-payment) | 19.0.1.0.0 | Production/Beta | AGPL-3 |
| 16 | Geri Dönen Ödemeler | account_payment_return + _import_iso20022 | [account-payment](https://github.com/OCA/account-payment) | 19.0.1.0.0 / 1.0.1 | Mature | AGPL-3 |
| 17 | Analitik Kuralları | account_analytic_required, purchase_analytic, stock_analytic | [account-analytic](https://github.com/OCA/account-analytic) | 19.0.1.0.0 | Beta | AGPL-3 |
| 18 | Bütçe Yönetimi | account_budget_oca | [account-budgeting](https://github.com/OCA/account-budgeting) | 19.0.1.1.0 | Beta | LGPL-3 |
| 19 | MIS Rapor Tasarımcısı | mis_builder, mis_builder_budget | [mis-builder](https://github.com/OCA/mis-builder) | 19.0.1.2.0 / 1.0.1 | Production | AGPL-3 |
| 20 | Gelişmiş Vadeler | account_payment_term_extension | [account-payment](https://github.com/OCA/account-payment) | 19.0.1.0.0 | Beta | AGPL-3 |
| 21 | İskonto Yönetimi | account_global_discount, account_invoice_triple_discount | [account-invoicing](https://github.com/OCA/account-invoicing) | 19.0.1.0.0 | Beta | AGPL-3 |
| 22 | Sevkiyattan Fatura | stock_picking_invoicing | [account-invoicing](https://github.com/OCA/account-invoicing) | 19.0.1.0.1 | Beta | AGPL-3 |
| 23 | Fatura Gruplama + Otomatik | sale_order_invoicing_grouping_criteria, partner_invoicing_mode | [account-invoicing](https://github.com/OCA/account-invoicing) | 19.0.1.0.0 | Production/Beta | AGPL-3 |
| 24 | Wiki + İş Talimatları | document_page, document_knowledge, mgmtsystem, document_page_work_instruction | [knowledge](https://github.com/OCA/knowledge), [management-system](https://github.com/OCA/management-system) | 19.0.1.0.2 / 1.2.0 / 1.0.1 | Beta | AGPL-3 |

## Bağımlılık Sağlayıcı Modüller

Kartlardaki modüllerin çalışması için ek olarak mount edilen OCA modülleri:

| Modül | Sağlayıcı Repo | Hangi Kartlar İçin |
|---|---|---|
| `date_range` | OCA/server-ux | 1, 2, 19 |
| `report_xlsx` | OCA/reporting-engine | 1, 4, 19 |
| `report_xlsx_helper` | OCA/reporting-engine | 4, 9 |
| `base_comment_template` | OCA/reporting-engine | 6 |
| `account_statement_base` | OCA/account-reconcile | 12, 13 |
| `account_statement_import_base`, `_file` | OCA/bank-statement-import | 13 |
| `base_view_inheritance_extension` | OCA/server-tools | 17, 22 |
| `base_partition` | OCA/server-tools | 23 |
| `base_global_discount` | OCA/server-backend | 21 |
| `queue_job` | OCA/queue | 23 (⚠️ openupgradelib gerekir) |
| `stock_picking_invoice_link` | OCA/stock-logistics-workflow | 22 |
| `document_knowledge` | OCA/knowledge | 24 |
| `mgmtsystem` | OCA/management-system | 24 |

## Test Durumu

- **37/37 kart modülü** scratch DB'de test edildi → **PASS**
- Testler `--with l10n_generic_coa,sale` ile genel hesap planı + satış ortamında çalıştırıldı
- Sonuçlar `~/.config/odoo-installer/tested.toml` whitelist'ine kaydedildi
- Canlı veritabanına **hiçbir kurulum yapılmadı**; aktivasyon kullanıcı tarafından UI'dan yapılacaktır

## Modül Sayfaları

Kurulum sırasına göre gruplanmış detaylı dokümantasyon sol menüdedir. Her sayfa; modülün ne yaptığını, Odoo'da neleri değiştirdiğini, süreç akışını ve kurulum notlarını içerir.
