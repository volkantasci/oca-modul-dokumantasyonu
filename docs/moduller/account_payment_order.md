# Ödeme Emirleri ve Toplu Ödeme (`account_payment_order` + PAIN + SEPA)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 14/24 · **Proje Kartı:** id 39 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_payment_order`, `account_banking_pain_base`, `account_banking_sepa_credit_transfer` |
| OCA Deposu | [OCA/bank-payment](https://github.com/OCA/bank-payment) |
| Sürüm | 19.0.1.0.5 / 19.0.1.1.1 / 19.0.1.0.0 |
| Olgunluk | Mature / Beta / Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account_payment_mode` (id 40), `base_iban`; python: `unidecode`, `lxml` |
| Kategori | Accounting / Payment |

## Nedir?

Toplu ödeme sürecinin çekirdeğidir:

- **`account_payment_order`** — Açık satıcı fatura kalemlerinden **ödeme emri** oluşturur; tahsilat tarafında **borç emri (debit order)**; onaylanınca bankaya gidecek dosyayı üretir
- **`account_banking_pain_base`** — ISO 20022 **PAIN XML** üretiminin ortak altyapısı (dosya formatı, transfer tipleri, batch mantığı)
- **`account_banking_sepa_credit_transfer`** — **SEPA havale (pain.001)** dosyası üreten uygulama modülü

## Ne İşe Yarar?

- **Toplu ödeme:** Onlarca faturayı tek emirde toplayıp tek dosyayla bankaya gönderme
- **Çift ödeme engeli:** Aynı kalem birden fazla emre giremez
- **Onay akışı:** Emir hazırlama → onay → dosya üretimi adımları; yetki kullanıcı gruplarıyla ayrılır
- **Borç emri:** Müşterilerden toplu tahsilat/direkt borçlandırma emirleri
- **Banka dosya standardı:** PAIN altyapısı ile banka dosya üretimi tek yerden yönetilir

## Odoo'da Neleri Değiştirir?

- **Faturalama → Tedarikçiler → Ödeme Emirleri** menüsü
- **Faturalama → Müşteriler → Borç Emirleri (Debit Orders)** menüsü
- Fatura ekranlarına **"Add to Payment Order"** (tedarikçi) / **"Add to Debit Order"** (müşteri) aksiyonları
- Ödeme emri ekranı: kalem seçimi, onay, **Print** (ödeme listesi çıktısı), dosya üretimi
- Ödeme modlarına toplu ödeme ile ilgili **ek seçenekler** (transfer tipi, batch booking vb.)

!!! warning "SEPA ve Türkiye"
    SEPA, Avrupa bankacılık standardıdır ve Türkiye'de bankalar tarafından doğrudan kabul edilmez. Ancak `account_payment_order` + `pain_base` altyapısı, **banka dosya üretiminin temelidir**; Türkiye'ye özgü format (ör. banka talimat dosyası) gerekirse bu altyapı üzerine özel dışa aktarma geliştirilir.

## Nasıl Çalışır?

1. Ödeme modu tanımlıdır (id 40)
2. *Ödeme Emirleri → Yeni* ile emir açılır; mod seçilir
3. **Kalem seçimi:** vadesi gelen borç/alacak kalemleri listelenip emre eklenir (faturadan da eklenebilir)
4. Emir onaylanır → ödeme/dosya oluşturulur
5. Üretilen dosya bankaya yüklenir; **Print** ile iç ödeme listesi alınır
6. Banka geri dönüşleri (başarısız ödeme) için bkz. `account_payment_return`

## Kurulum ve Yapılandırma

- `account_payment_mode` (id 40) **önce** kurulmuş olmalıdır
- Python bağımlılıkları `unidecode` ve `lxml` imajda mevcuttur
- Ödeme modunda transfer tipi/batch ayarları yapılır
- SEPA kullanılmayacaksa `sepa_credit_transfer` aktif edilmeden de `payment_order` kullanılabilir
- **Mature** olgunlukta çekirdek modül; SEPA uygulaması Beta

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account_payment_order.group_account_payment` | Accounting / Payments | Ödeme ve borç emri menüleri/işlemleri bu grupla kısıtlıdır; kurulumda admin'lere atanır |
| `account.group_account_manager` | Yönetici | Ödeme modu ayarları (transfer tipi, batch) yapılandırması |
| `base.group_multi_company` | Çoklu Şirket | Ödeme modu/şirket alanları |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari) bölümüne bakın.

## Kaynaklar

- [GitHub — account_payment_order (19.0)](https://github.com/OCA/bank-payment/tree/19.0/account_payment_order)
- [GitHub — account_banking_pain_base (19.0)](https://github.com/OCA/bank-payment/tree/19.0/account_banking_pain_base)
- [GitHub — account_banking_sepa_credit_transfer (19.0)](https://github.com/OCA/bank-payment/tree/19.0/account_banking_sepa_credit_transfer)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/bank-payment&target_branch=19.0)
