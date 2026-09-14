# Gelişmiş Ödeme Vadeleri (`account_payment_term_extension`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 20/24 · **Proje Kartı:** id 45 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_payment_term_extension` |
| OCA Deposu | [OCA/account-payment](https://github.com/OCA/account-payment/tree/19.0/account_payment_term_extension) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`, `purchase` |
| Kategori | Accounting / Payment |

## Nedir?

Standart ödeme vadesi (payment term) tanımını genişleten modüldür: ayın belirli günleri, ay sonu seçeneği, haftalık/aylık aralıklar, çoklu vade günleri ve **yuvarlama** desteği ekler.

## Ne İşe Yarar?

Türkiye pratiğinde yaygın olan vade koşullarını Odoo'da tanımlanabilir hale getirir:

- **Ayın belirli günü:** "Her ayın 15'i", "Her ayın son günü"
- **Ay sonu + gün:** "Ay sonundan 30 gün sonra"
- **Çoklu vade günleri:** "Ayın 15'i ve 30'u" gibi birden fazla ödeme günü (boşluk, virgül veya tire ile ayrılır)
- **Tatil ertelemesi:** Tatil günü tanımlanır; vade o güne denk gelirse sonraki iş gününe ertelenir
- **Kısmi taksit satırları:** Vade birden fazla satıra bölünür (ör. %40 / %30 / %30)
- **Yuvarlama:** Vade tutarlarının belirli birime yuvarlanması

## Odoo'da Neleri Değiştirir?

- **Faturalama → Yapılandırma → Faturalama → Ödeme Vadeleri** ekranındaki vade satırlarına yeni alanlar/tipler ekler:
  - Gün/hafta/ay aritmetiği genişletilir
  - Ay sonu (end of month) seçeneği
  - Çoklu ödeme günü tanımı
- **Ayarlar → Faturalama → Ödeme Vadeleri** bölümünden vade tipi seçenekleri yönetilir
- Fatura tarihi girildiğinde vade tarihleri yeni kurallara göre hesaplanır

## Nasıl Çalışır?

1. Vade kaydı açılır; satırlarda gecikme tipi ve değerleri ayarlanır
2. Çoklu gün için örnek: `15, 30` veya `1-15-30` gibi gün listesi girilir
3. Tatil tanımı yapılır (vade tatil gününe denk gelirse ötelenir)
4. Faturada vade seçilip tarih girilince **hesaplanan vade tarihleri** görüntülenir

!!! warning "Bilinmeyen sınırlama"
    Modül **nakit yuvarlaması (cash rounding)** ile uyumlu değildir. Nakit yuvarlama kullanılan senaryolarda dikkat edilmelidir.

## Kurulum ve Yapılandırma

- Bağımlılıklar `account` + `purchase` (ikisi de çekirdek)
- **Beta** olgunlukta ama uzun yıllardır yaygın kullanılan bir modüldür
- Vade davranışı faturalama akışını etkilediği için **test instance'ında senaryo testi** yapılmalıdır (ör. ayın 31'i, şubat ayı, ay sonu + 15 gün)

## Kaynaklar

- [GitHub — account_payment_term_extension (19.0)](https://github.com/OCA/account-payment/tree/19.0/account_payment_term_extension)
- [Hata Takibi](https://github.com/OCA/account-payment/issues)
