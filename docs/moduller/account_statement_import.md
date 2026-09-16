# Banka Ekstresi İçe Aktarma (`account_statement_import_camt` + `account_statement_import_sheet_file_xlsx`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 13/24 · **Proje Kartı:** id 42 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_statement_import_camt`, `account_statement_import_sheet_file_xlsx` |
| OCA Deposu | [OCA/bank-statement-import](https://github.com/OCA/bank-statement-import) |
| Sürüm | 19.0.1.0.0 / 19.0.2.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account_statement_import_file` → `account_statement_import_base` → `account_statement_base` (OCA/account-reconcile) |
| Kategori | Accounting / Bank |

## Nedir?

Bankadan alınan ekstre dosyalarını Odoo'ya **banka ekstresi olarak içe aktaran** iki modüldür:

| Modül | Format | Açıklama |
|---|---|---|
| `account_statement_import_camt` | CAMT.053 / CAMT.052 (ISO 20022 XML) | Avrupa bankalarının standart XML ekstreleri |
| `account_statement_import_sheet_file_xlsx` | XLSX (Excel) | Bankanın verdiği Excel/CSV dosyaları; kolon eşleme sihirbazı |

## Ne İşe Yarar?

- **Manuel ekstre girişini ortadan kaldırır:** Bankadan indirilen dosya doğrudan Odoo'ya yüklenir
- **Uzlaştırma akışının giriş noktası:** İçe aktarılan satırlar uzlaştırma ekranında (bkz. `account_reconcile_oca`) eşleştirilir
- **Şablonlu eşleme:** Excel kolonları bir kez eşlenir; sonraki aktarımlarda şablon otomatik uygulanır
- **Çok banka desteği:** Her banka formatı için ayrı eşleme şablonu tanımlanır

## Odoo'da Neleri Değiştirir?

- **Faturalama → Muhasebe → Banka Ekstreleri** ekranına **Import Statement** sihirbazı ekler (dosya yükleme + format seçimi)
- **Faturalama → Yapılandırma → Muhasebe → Ekstre Kolon Eşlemeleri** menüsü ekler:
  - Dosya formatı (XLSX/CSV), ayırıcı, kolon eşlemeleri (tarih, tutar, referans, ortak vb.)
- CAMT için ek parser; yüklenen dosya doğrudan ekstre kaydına dönüşür
- Aynı repoda `account_statement_import_camt54` (camt.054 debit/kredi hareketleri) ve online banka bağlantı modülleri de bulunur (opsiyonel)

## Nasıl Çalışır?

1. Banka internet şubesinden ekstre dosyası indirilir
2. Odoo'da **Banka Ekstreleri → Import** sihirbazı açılır
3. Dosya seçilir; CAMT ise otomatik algılanır, Excel ise eşleme şablonu seçilir
4. Satırlar önizlenir ve **Import** ile ekstreye dönüştürülür
5. Uzlaştırma ekranında (12) kalemlerle eşleştirilir

## Kurulum ve Yapılandırma

- Bağımlılık zinciri: `account_statement_import_base` → `account_statement_base` (12. karttaki modülle gelir)
- Excel eşlemesi için örnek dosya üzerinden kolon tanımları yapılır; tarih formatı bankaya göre ayarlanır
- **Türkiye pratiği:** Bankaların çoğu Excel/CSV verir → **`sheet_file_xlsx` ana kanal olacaktır**; CAMT, dosya üreten bankalar için hazırdır
- İçe aktarma sonrası otomatik uzlaştırma istenirse aynı repodaki `account_statement_import_file_reconcile_oca` değerlendirilebilir

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_readonly` | Muhasebe Özelliklerini Göster - Salt Okunur | Banka Ekstreleri menüsü ve içe aktarma sihirbazı |
| `account.group_account_manager` | Yönetici | Ekstre kolon eşleme şablonları (Yapılandırma → Muhasebe) |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari) bölümüne bakın.

## Kaynaklar

- [GitHub — account_statement_import_camt (19.0)](https://github.com/OCA/bank-statement-import/tree/19.0/account_statement_import_camt)
- [GitHub — account_statement_import_sheet_file_xlsx (19.0)](https://github.com/OCA/bank-statement-import/tree/19.0/account_statement_import_sheet_file_xlsx)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/bank-statement-import&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/bank-statement-import/issues)
