# Geri Dönen Ödemeler (`account_payment_return` + `_import_iso20022`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 16/24 · **Proje Kartı:** id 44 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_payment_return`, `account_payment_return_import_iso20022` |
| OCA Deposu | [OCA/account-payment](https://github.com/OCA/account-payment) |
| Sürüm | 19.0.1.0.0 / 19.0.1.0.1 |
| Olgunluk | Mature |
| Lisans | AGPL-3 |
| Bağımlılıklar | `mail`, `account`; import modülü: `account_payment_return_import` + `account_payment_order` (OCA/bank-payment) |
| Kategori | Accounting / Payment |

## Nedir?

Bankadan **geri dönen (unpaid/return) ödemeleri** yönetir: karşılıksız çek, reddedilen otomatik tahsilat, hesap kapalı gibi nedenlerle iade olan ödemelerin muhasebeleştirilmesi ve orijinal belgeye bağlanması. `_import_iso20022` modülü ise ISO 20022 formatlı geri dönüş dosyalarını (PAIN) otomatik içe aktarır.

## Ne İşe Yarar?

- **Doğru muhasebeleştirme:** Geri dönen tutar için ters kayıt üretir; banka masrafları satır bazında eklenebilir
- **İzlenebilirlik:** İade, orijinal ödeme/fatura ile ilişkilendirilir; "hangi fatura neden geri döndü" geçmişi
- **Otomatik eşleştirme:** Referans/satır eşleştirme butonuyla fatura ve hareketler bulunur
- **Otomatik içe aktarma:** Banka PAIN formatlı iade dosyası veriyorsa elle kayıt gerekmez

## Odoo'da Neleri Değiştirir?

- **Faturalama → Müşteriler → Müşteri Ödeme İadeleri** menüsü ekler
- İade formu satırlarında: iade edilen kalem, tutar, **banka masrafı** alanları
- **Confirm** butonu ile iade fişi oluşturulur; ödeme geçmişi fatura üzerinden izlenir
- `_import_iso20022`: banka dosyası yükleme sihirbazı ekler (PAIN.002/camt54 iade hareketleri parser'ları)

## Nasıl Çalışır?

1. *Müşteri Ödeme İadeleri → Yeni* kayıt açılır
2. Her satıra tahsil edilmiş (uzlaşmış) alacak kalemi ve iade edilecek tutar girilir
   - Alternatif: referans girip **Match** butonuyla otomatik eşleştirme
3. Banka masrafı varsa satırda belirtilir
4. **Confirm** → banka günlüğünden bakiyeyi düşen ters fiş oluşur, kalemler uzlaştırılır
5. Oluşturulan hareket ödeme formundan takip edilir

## Kurulum ve Yapılandırma

- İade içe aktarma modülü **cross-repo** bağımlılık taşır: `account_payment_order` (bank-payment, id 39) önce kurulmuş olmalıdır
- **Mature** olgunlukta; güvenle kullanılabilir
- İade nedenleri listesi ihtiyaç halinde özelleştirilebilir (mail chatter entegrasyonu vardır)

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_invoice` | Faturalama | Müşteri Ödeme İadeleri menüleri (Faturalama → Müşteriler) |
| `account.group_account_readonly` | Muhasebe Özelliklerini Göster - Salt Okunur | İade fişi detayı ve muhasebe erişimi |
| `account_payment_order.group_account_payment` | Accounting / Payments | ISO 20022 iade dosyası içe aktarma (bank-payment altyapısı) |
| `base.group_multi_company` | Çoklu Şirket | Çok şirketli görünüm |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_payment_return (19.0)](https://github.com/OCA/account-payment/tree/19.0/account_payment_return)
- [GitHub — account_payment_return_import_iso20022 (19.0)](https://github.com/OCA/account-payment/tree/19.0/account_payment_return_import_iso20022)
- [Hata Takibi](https://github.com/OCA/account-payment/issues)
