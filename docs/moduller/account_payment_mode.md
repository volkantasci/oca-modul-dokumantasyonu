# Ödeme Modları (`account_payment_mode`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 11/24 · **Proje Kartı:** id 40 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_payment_mode` (+ `account_payment_sale`, `account_payment_purchase`) |
| OCA Deposu | [OCA/bank-payment](https://github.com/OCA/bank-payment/tree/19.0/account_payment_mode) |
| Sürüm | 19.0.1.1.0 (sale 1.0.1 / purchase 1.0.0) |
| Olgunluk | Mature (sale/purchase: Beta, auto_install) |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`; sale/purchase uzantıları: `sale`, `purchase` |
| Kategori | Accounting / Payment |

## Nedir?

**Ödeme modu (payment mode)** kavramını getirir: bir ödeme yöntemini (havale/EFT, çek, kredi kartı, otomatik tahsilat) **banka günlüğü ve banka hesabı ile eşleştiren** altyapı modülüdür. `bank-payment` ekosisteminin temel taşıdır.

## Ne İşe Yarar?

- **Para trafiğinin doğru kanaldan geçmesi:** Her ödeme/tahsilat hangi banka hesabına bağlıysa o kanaldan işlenir
- **Cari bazlı varsayılan:** Müşteri/tedarikçi kartında varsayılan ödeme modu tanımlanır; faturalara otomatik taşınır
- **Filtreli işlem:** Ödeme emri oluşturulurken yalnızca ilgili ödeme modunun kalemleri listelenir
- **Tutarlı uzlaştırma:** Banka hesabı-günlük eşleşmesi doğru olduğundan ekstre uzlaştırması sadeleşir

## Odoo'da Neleri Değiştirir?

- **Faturalama → Yapılandırma → Yönetim → Ödeme Modları** menüsü ekler
  - Her mod: ad, ödeme yöntemi (havale/çek vb.), **banka günlüğü**, şirket
- **Ortak kartına** ödeme modu alanları: satış tarafı (alacak) ve satın alma tarafı (borç) için ayrı varsayılanlar
- **Fatura formuna** ödeme modu alanı; ortaktan otomatik doldurulur, taslakta değiştirilebilir
- **Fatura raporunda** ortak banka hesabını gösterme opsiyonu
- `account_payment_sale` → satış siparişlerine, `account_payment_purchase` → satın alma siparişlerine ödeme modu taşır (ilgili modüller kuruluysa **otomatik aktif** olur)

## Nasıl Çalışır?

1. *Ödeme Modları* ekranında modlar tanımlanır (ör. "Ziraat EFT", "İş Bankası Çek")
2. Ortak kartında varsayılan mod atanır
3. Yeni fatura oluşturulduğunda mod ortaktan otomatik gelir
4. Bu fatura, ödeme emri (bkz. `account_payment_order`) oluşturulurken ilgili mod seçilirse listelenir
5. Vadesi gelince ödeme emri üzerinden toplu işlem yapılır

!!! tip "Kurulum sırası önemi"
    Bu modül, Banka ve Ödeme grubunun **ilk** modülüdür. `account_payment_order`, `account_banking_mandate` ve ekstre akışları bu kavram üzerine kurulur; önce bu kurulmalıdır.

## Kurulum ve Yapılandırma

- `account_payment_sale` ve `account_payment_purchase` modülleri `auto_install`'dır: `sale`/`purchase` kuruluysa kendiliğinden aktifleşir
- Ödeme modları şirket bazındadır; çok şirketli yapıda her şirkete uygun banka günlükleri tanımlanmalıdır
- **Mature** olgunluk — güvenle kullanılabilir

## Kaynaklar

- [GitHub — account_payment_mode (19.0)](https://github.com/OCA/bank-payment/tree/19.0/account_payment_mode)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/bank-payment&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/bank-payment/issues)
