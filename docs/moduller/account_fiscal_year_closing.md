# Dönem/Yıl Kapanışı (`account_fiscal_year_closing`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 10/24 · **Proje Kartı:** id 53 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_fiscal_year_closing` |
| OCA Deposu | [OCA/account-closing](https://github.com/OCA/account-closing/tree/19.0/account_fiscal_year_closing) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account` |
| Kategori | Accounting / Closing |

## Nedir?

Mali yıl **dönem kapanışını sihirbazla adım adım** yürüten modüldür: gelir-gider hesaplarını kapatıp dönem kârına aktarma, kapanış fişi üretme ve gerekiyorsa devir (açılış) fişi oluşturma.

## Ne İşe Yarar?

- **Yıl sonu kapanış otomasyonu:** Community'de otomatik kapanış yoktur; bu modül standart süreci tanımlı adımlara böler
- **Güvenli deneme:** Kapanış öncesi *Calculate* ile taslak sonuç görülür; *Cancel* ile geri alınabilir (fişler silinir, uzlaştırmalar çözülür)
- **Şablon tabanlı:** Kapanış fişleri özelleştirilebilir şablonlarla üretilir
- **Kontrol mekanizması:** Dönem içinde taslak fiş varsa uyarı seçeneği

## Odoo'da Neleri Değiştirir?

- **Muhasebe → Danışman → Mali Yıl Kapanışları** menüsü ekler
- **Yapılandırma → Mali Yıl Kapanışı → Kapanış Şablonları** menüsü ekler (şablon yönetimi)
- Kapanış kaydında: yıl seçimi, şablon seçimi, **Calculate**, **Show Moves**, **Show Move Lines**, **Confirm and post moves**, **Cancel** aksiyonları
- Taslak fiş kontrolü, dengesiz fiş uyarı ekranı

## Nasıl Çalışır?

1. *Mali Yıl Kapanışları → Yeni* ile kayıt açılır
2. Kapanacak yıl (mali yıl takvim yılından farklıysa son yıl) seçilir
3. Uygun kapanış şablonu seçilir
4. **Calculate** ile kapanış fişleri hesaplanır (henüz kaydedilmez)
5. **Show Moves / Show Move Lines** ile sonuç incelenir
6. Uygunsa **Confirm and post moves** ile fişler onaylanır ve uzlaştırılır
7. Hata durumunda **Cancel** ile geri alınır

## Kurulum ve Yapılandırma

- Yalnızca `account` bağımlılığı
- Kapanış şablonları şirketin hesap planına göre uyarlanmalıdır (şablon satırları = üretilecek fişler)
- **Türkiye uygulaması:** Kapanış/devir fişleri muhasebe biriminin yetkisindedir; ilk kapanış mutlaka **test instance'ında muhasebe ile birlikte** denenmelidir
- Sabit kıymet amortisman fişleri (bkz. `account_asset_management`) kapanıştan **önce** üretilmiş olmalıdır

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_readonly` | Muhasebe Özelliklerini Göster - Salt Okunur | Mali Yıl Kapanışları menüsü (Muhasebe bölümü) |
| `account.group_account_manager` | Yönetici | Kapanış şablonları yapılandırması |
| `account.group_account_user` | Bütün Muhasebe Hesaplarını Göster | Kapanış/aktarma fişlerinin oluşturulması için önerilir |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_fiscal_year_closing (19.0)](https://github.com/OCA/account-closing/tree/19.0/account_fiscal_year_closing)
- [Hata Takibi](https://github.com/OCA/account-closing/issues)
