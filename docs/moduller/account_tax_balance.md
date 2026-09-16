# Vergi Bakiyeleri (`account_tax_balance`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 2/24 · **Proje Kartı:** id 36 · **Test Durumu:** ✅ PASS · **Aktivasyon:** ✅ 15.09.2026

| Alan | Değer |
|---|---|
| Teknik Ad | `account_tax_balance` |
| OCA Deposu | [OCA/account-financial-report](https://github.com/OCA/account-financial-report/tree/19.0/account_tax_balance) |
| Sürüm | 19.0.1.0.3 |
| Olgunluk | Mature |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`, `date_range` (OCA/server-ux) |
| Kategori | Accounting / Reporting |

## Nedir?

Seçilen **tarih aralığına göre vergi bakiyelerini** hesaplayan rapor modülüdür. Vergi kodları bazında tahsil edilen/ödenen vergileri ve net pozisyonu gösterir.

## Ne İşe Yarar?

- **KDV (1/2) dönem kontrolü:** Dönem içinde hesaplanan KDV, indirilecek KDV ve ödenecek/geçecek tutarların hızlı özeti
- **Beyanname öncesi mutabakat:** Beyannamedeki rakamlar ile muhasebedeki vergi hesaplarının uyum kontrolü
- **Geçmiş dönem analizi:** Tarih aralığı serbestçe seçilebildiğinden önceki dönemlerle karşılaştırma
- **Vergi kodu kırılımı:** Hangi vergi kodunda ne kadar matrah/vergi oluştuğunun takibi

## Odoo'da Neleri Değiştirir?

- **Faturalama → Raporlama → Vergi Bakiyeleri** menüsü ekler
- Rapor ekranında şirket, tarih aralığı ve hareket türü seçilir; **"vergileri aç"** ile vergi satırlarının detayı incelenir
- Vergi hesaplarının (ör. 191/391) ve vergi kodlarının listesinde **bakiyeye doğrudan atlama** aksiyonları ekler
- Fatura/muhasebe hareketi ekranlarına ilgili vergi bakiyesine erişim bağlantıları ekler

## Nasıl Çalışır?

1. *Faturalama → Raporlama → Vergi Bakiyeleri* açılır
2. Şirket + tarih aralığı + hedef hareketler (taslak/kayıtlı) seçilir
3. Rapor; **Matrah**, **Vergi**, **Net** ve **Bakiye** sütunlarını vergi kodu bazında listeler
4. Satır açılarak ilgili vergi hareketleri görüntülenir

## Saha Notları (Aktivasyon Sonrası)

**Aktivasyon:** 15.09.2026'da canlıda aktive edildi (bağımlılığı `date_range` zaten kuruluydu).

**Menü yolu:** **Faturalama → Raporlama → Vergi Bakiyeleri**

**Ekran akışı (doğrulanmış):**

| Adım | Detay |
|---|---|
| Sihirbaz alanları | Şirketler, Tarih Aralığı, Başlangıç Tarihi, Bitiş Tarihi, Hedef Hareketler |
| Tarih Aralığı | Bir [tarih aralığı](date_range.md) seçildiğinde başlangıç/bitiş tarihleri **otomatik dolar**; elle de değiştirilebilir |
| Hedef Hareketler | *Tüm Onaylanmış Kayıtlar* (varsayılan) veya *Tüm Kayıtlar* |
| Buton | **Vergileri Aç** ile sonuç listesi açılır |
| Sonuç sütunları | Kısa Ad, Bakiye, Matrah Bakiyesi, İade Bakiyesi, İade Matrah Bakiyesi, Toplam Bakiye, Toplam Matrah Bakiyesi |
| Detay | Her satırdaki büyüteç (fa-search-plus) butonu ile ilgili vergi hareketleri açılır |

!!! note "Çeviri durumu (instance'a özel)"
    Modül **hiç `tr.po` içermediği** için menü ve ekranlar İngilizce geliyordu. 15.09.2026'da şu çeviriler instance veritabanına eklendi: menü/aksiyon adları ("Vergi Bakiyeleri"), sihirbaz alanları (Şirketler, Tarih Aralığı, Başlangıç/Bitiş Tarihi, Hedef Hareketler), hedef hareket seçenekleri, sonuç sütunları, butonlar (Vergileri Aç, İptal), liste/matrah toplam etiketleri ve arama filtreleri (Vergi Grubu, Vergi Kapsamı).

    Bu çeviriler **instance DB'sindedir** (modül dosyasında değil); upstream'e katkısı Weblate üzerinden yapılmalıdır — bu iş, proje kartı 57'deki Weblate görevi kapsamındadır.

## Kurulum ve Yapılandırma

- Bağımlılık `date_range` otomatik gelir; ek yapılandırma gerekmez
- **Mature** olgunlukta, hafif ve riski düşük bir modüldür
- Aynı depodaki `account_financial_report` içindeki KDV Raporu ile birlikte kullanılması önerilir
- Dönem seçimi için bkz. [Tarih Aralıkları (date_range)](date_range.md)

## Kaynaklar

- [GitHub — account_tax_balance (19.0)](https://github.com/OCA/account-financial-report/tree/19.0/account_tax_balance)
- [Hata Takibi](https://github.com/OCA/account-financial-report/issues)
