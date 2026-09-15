# Genel Finansal Raporlar (`account_financial_report`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 1/24 · **Proje Kartı:** id 34 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_financial_report` |
| OCA Deposu | [OCA/account-financial-report](https://github.com/OCA/account-financial-report/tree/19.0/account_financial_report) |
| Sürüm | 19.0.0.0.21 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`, `date_range` (OCA/server-ux), `report_xlsx` (OCA/reporting-engine) |
| Kategori | Accounting / Reporting |

## Nedir?

Odoo Community'de bulunmayan **kurumsal muhasebe raporlarını** sağlayan sihirbaz tabanlı rapor setidir. Enterprise sürümdeki muhasebe raporlama yeteneklerinin OCA'daki karşılığıdır.

## Ne İşe Yarar?

Aşağıdaki raporları **PDF ve XLSX** formatında üretir:

| Rapor | Açıklama | Kullanım Alanı |
|---|---|---|
| **Genel Muhasebe Defteri** (General Ledger) | Hesap bazında tüm hareketler, açılış/kapanış bakiyeleri | Denetim, hesap takibi |
| **Mizan** (Trial Balance) | Hesapların borç/alacak toplamları ve bakiyeleri | Dönem kapanışı kontrolü |
| **Açık Kalemler** (Open Items) | Henüz kapatılmamış hareket kalemleri | Mutabakat takibi |
| **Cari Yaşlandırma** (Aged Partner Balance) | Alacak/borçların gün aralığına göre kırılımı | Tahsilat riski analizi |
| **Günlük Defteri** (Journal Ledger) | Günlük (dergi) bazında hareket dökümü | Belge bazlı inceleme |
| **KDV Raporu** (VAT Report) | Dönemsel vergi pozisyonu özeti | Beyanname ön kontrol |

## Odoo'da Neleri Değiştirir?

- **Faturalama → Muhasebe → Raporlama** menüsüne yeni rapor ekranları ekler
- Her rapor için **filtre sihirbazları** (tarih aralığı, dönem, ortak, hesap filtreleri) getirir
- Rapor çıktılarında **XLSX dışa aktarma** seçeneği sağlar
- `date_range` modülü sayesinde önceki dönemlerle çalışan **mali dönem seçici** entegre olur
- Yaşlandırma raporu için **Ayarlar → Faturalama → OCA Yaşlandırma Yapılandırması** ekranı açılır (dinamik gün aralıkları tanımlanır)

## Nasıl Çalışır?

1. Kullanıcı ilgili rapor menüsünü açar (ör. *Faturalama → Muhasebe → Raporlama → Genel Muhasebe Defteri*)
2. Sihirbazda tarih aralığı, mali dönem ve filtreler seçilir
3. Rapor ekranda üretilir; mizan ve genel defterde **hesap detayına inilebilir**
4. XLSX/PDF olarak indirilir veya yazdırılır

!!! tip "Yaşlandırma aralıkları"
    Varsayılan 30/60/90 gün yerine dinamik aralıklar tanımlanabilir: Örnek `15 → 30 → 60` yapılandırması ilk aralığı 0-15, ikinciyi 16-30, üçüncüyü 61+ yapar.

## Kurulum ve Yapılandırma

- Bağımlılıklar otomatik kurulur: `date_range` ve `report_xlsx` mount edilmiş durumdadır
- Raporlara erişim için kullanıcıda **Faturalama: Sorumlu** veya **Tam Muhasebe Özellikleri** yetkisi gerekir
- KDV raporu genel amaçlıdır; **resmî beyanname yerine geçmez**, beyanname öncesi hızlı mutabakat için kullanılır

!!! tip "Birlikte gelen modül: Tarih Aralıkları"
    Bu modülün bağımlılığı olan **`date_range`** ve otomatik kurulan **`date_range_account`** (raporlarda hazır dönem tanımları) için bkz. [Tarih Aralıkları (date_range)](date_range.md). Yaşlandırma aralıkları bu yapıdan bağımsızdır; raporlardaki *Tarih Aralığı* seçimi bu modülden gelir.

## Kaynaklar

- [GitHub — account_financial_report (19.0)](https://github.com/OCA/account-financial-report/tree/19.0/account_financial_report)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/account-financial-report&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/account-financial-report/issues)
