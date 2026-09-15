# Vergi Bakiyeleri (`account_tax_balance`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 2/24 · **Proje Kartı:** id 36 · **Test Durumu:** ✅ PASS

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

- **Muhasebe → Raporlama → Vergi Bakiyeleri** menüsü ekler
- Rapor ekranında şirket, tarih aralığı ve hareket türü seçilir; **"vergileri aç"** ile vergi satırlarının detayı incelenir
- Vergi hesaplarının (ör. 191/391) ve vergi kodlarının listesinde **bakiyeye doğrudan atlama** aksiyonları ekler
- Fatura/muhasebe hareketi ekranlarına ilgili vergi bakiyesine erişim bağlantıları ekler

## Nasıl Çalışır?

1. *Muhasebe → Raporlama → Vergi Bakiyeleri* açılır
2. Şirket + tarih aralığı + hedef hareketler (taslak/kayıtlı) seçilir
3. Rapor; **Matrah**, **Vergi**, **Net** ve **Bakiye** sütunlarını vergi kodu bazında listeler
4. Satır açılarak ilgili vergi hareketleri görüntülenir

## Kurulum ve Yapılandırma

- Bağımlılık `date_range` otomatik gelir; ek yapılandırma gerekmez
- **Mature** olgunlukta, hafif ve riski düşük bir modüldür
- Aynı depodaki `account_financial_report` içindeki KDV Raporu ile birlikte kullanılması önerilir
- Dönem seçimi için bkz. [Tarih Aralıkları (date_range)](date_range.md)

## Kaynaklar

- [GitHub — account_tax_balance (19.0)](https://github.com/OCA/account-financial-report/tree/19.0/account_tax_balance)
- [Hata Takibi](https://github.com/OCA/account-financial-report/issues)
