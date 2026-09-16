# Genel Finansal Raporlar (`account_financial_report`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 1/24 · **Proje Kartı:** id 34 · **Test Durumu:** ✅ PASS · **Aktivasyon:** ✅ 15.09.2026

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

- **Faturalama → Raporlama** menüsüne yeni rapor ekranları ekler
- Her rapor için **filtre sihirbazları** (tarih aralığı, dönem, ortak, hesap filtreleri) getirir
- Rapor çıktılarında **XLSX dışa aktarma** seçeneği sağlar
- `date_range` modülü sayesinde önceki dönemlerle çalışan **mali dönem seçici** entegre olur
- Yaşlandırma raporu için **Ayarlar → Faturalama → OCA Yaşlandırma Yapılandırması** ekranı açılır (dinamik gün aralıkları tanımlanır)

## Nasıl Çalışır?

1. Kullanıcı ilgili rapor menüsünü açar (ör. *Faturalama → Raporlama → OCA Muhasebe Raporları → Büyük Defter*)
2. Sihirbazda tarih aralığı, mali dönem ve filtreler seçilir
3. Rapor ekranda üretilir; mizan ve genel defterde **hesap detayına inilebilir**
4. XLSX/PDF olarak indirilir veya yazdırılır

!!! tip "Yaşlandırma aralıkları"
    Varsayılan 30/60/90 gün yerine dinamik aralıklar tanımlanabilir: Örnek `15 → 30 → 60` yapılandırması ilk aralığı 0-15, ikinciyi 16-30, üçüncüyü 61+ yapar.

## Saha Notları (Aktivasyon Sonrası)

**Aktivasyon:** 15.09.2026'da canlıda aktive edildi.

**Otomatik gelenler:** Bağımlılıklar (`date_range`, `report_xlsx`) kuruldu; `date_range` + `account` mevcut olduğu için **`date_range_account`** (auto_install) kendiliğinden geldi — ayrıntılar: [Tarih Aralıkları](date_range.md).

**Menü yolu:** **Faturalama → Raporlama → OCA Muhasebe Raporları**

| Alt Menü | Rapor |
|---|---|
| Büyük Defter | General Ledger |
| Yevmiye Defteri | Journal Ledger |
| Geçici Mizan | Trial Balance |
| Açık Pozisyonlar | Open Items |
| Yaşlandırılmış İş Ortağı Bakiyesi | Aged Partner Balance |
| KDV Raporu | VAT Report |

**Çeviri notu:** Modülün kendi `tr.po` dosyası vardır ve menüler Türkçe gelir. Aktivasyon sırasında iki upstream bozulma tespit edilip düzeltildi: (1) `date_range_account` menülerindeki `Tarih Aral??klar??` bozulması, (2) bu modülün KDV Raporu sihirbazındaki `KDV Raporu Se??enekleri` dizgisi. Kalıcı düzeltme için upstream PR'lar açıldı: [OCA/server-ux#1333](https://github.com/OCA/server-ux/pull/1333) ve [OCA/account-financial-reporting#1573](https://github.com/OCA/account-financial-reporting/pull/1573) (proje kartı 57).

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
