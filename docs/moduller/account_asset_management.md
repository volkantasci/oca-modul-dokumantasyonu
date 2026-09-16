# Sabit Kıymet ve Amortisman Yönetimi (`account_asset_management`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 9/24 · **Proje Kartı:** id 38 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_asset_management` |
| OCA Deposu | [OCA/account-financial-tools](https://github.com/OCA/account-financial-tools/tree/19.0/account_asset_management) |
| Sürüm | 19.0.1.0.3 |
| Olgunluk | Mature |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`, `report_xlsx_helper`; python: `python-dateutil` |
| Kategori | Accounting / Assets |

## Nedir?

Community sürümde bulunmayan **sabit kıymet (demirbaş) ve amortisman yönetimini** sağlayan kapsamlı modüldür. Enterprise'daki *Accounting → Assets* özelliğinin OCA karşılığıdır.

## Ne İşe Yarar?

- **Sabit kıymet takibi:** Her demirbaş için kart; alış tarihi, maliyeti, hesap eşlemeleri, durumu
- **Amortisman profilleri:** Doğrusal veya azalan bakiyeli yöntemler, farklı oran/süreler, katsayılar
- **Otomatik amortisman fişleri:** Dönemsel amortismanlar cron (zamanlanmış iş) ile otomatik oluşturulur
- **Faturadan varlık üretimi:** Tedarikçi faturasındaki satırdan doğrudan sabit kıymet kartı açılır
- **Elden çıkarma (disposal):** Satış/ıskat işlemlerinin muhasebeleştirilmesi
- **Raporlama:** Kıymet listesi, amortisman tabloları (XLSX)

## Odoo'da Neleri Değiştirir?

- **Faturalama/Muhasebe menüsüne `Assets` (Sabit Kıymetler)** üst menüsü ekler:
  - **Asset Groups** (kıymet grupları)
  - **Assets** (kıymet kartları)
  - **Asset Profiles** (amortisman profilleri)
- Kıymet kartında aksiyonlar: *Confirm Asset*, *Compute*, *Create Move*, *Delete/Reverse Move*, *Close*
- Fatura satırına **"Create Asset"** akışı ekler (fatura kesilirken/kaydedilirken kıymet oluşturma)
- Amortisman **plan satırları** (Asset Line) ve dönemsel fiş üretimi
- **Zamanlanmış iş (cron):** "Generate Assets Entries" benzeri otomatik amortisman görevi

!!! warning "Enterprise ile karışmaz"
    Modül standart `account_asset` (Enterprise) ile uyumsuzdur; manifest'te `excludes` tanımlıdır. Community'de bu modül zaten bulunmadığı için çakışma beklenmez.

## Nasıl Çalışır?

1. **Profil tanımı:** Amortisman süresi, yöntemi (linear/declining), hesap eşlemeleri tanımlanır
2. **Varlık oluşturma:**
   - Manuel: *Assets → Create*
   - Faturadan: fatura satırında kıymet oluşturma (fatura kaydı/onayı anında)
3. **Amortisman hesaplama:** Kıymet kartında *Compute* ile plan satırları üretilir
4. **Fiş üretimi:** Dönemsel olarak *Create Move* veya cron ile fişler oluşur
5. **Elden çıkarma:** Satış/ıskat kaydı ile kıymet kapatılır

## Kurulum ve Yapılandırma

- `report_xlsx_helper` bağımlılığı reporting-engine mount'undan gelir; python `python-dateutil` imajda mevcuttur
- Amortisman oran ve süreleri **vergi mevzuatına göre** profil olarak tanımlanmalıdır (VUK oranları)
- Otomatik amortisman fişlerinin oluşma zamanı (cron) ve günlüğü muhasebe politikasına göre ayarlanmalı
- **Mature** olgunlukta; yine de ilk kullanım öncesi test instance'ında bir kıymetle uçtan uca deneme önerilir

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_readonly` | Muhasebe Özelliklerini Göster - Salt Okunur | Faturalama → Sabit Kıymetler menüleri |
| `account.group_account_manager` | Yönetici | Kıymet profilleri, grupları ve yapılandırma |
| `account.group_account_invoice` | Faturalama | Fatura üzerinden kıymet oluşturma |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_asset_management (19.0)](https://github.com/OCA/account-financial-tools/tree/19.0/account_asset_management)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/account-financial-tools&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/account-financial-tools/issues)
