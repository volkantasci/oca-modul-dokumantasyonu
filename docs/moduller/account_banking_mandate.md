# Bankacılık Talimatları (`account_banking_mandate` + `_sale`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 15/24 · **Proje Kartı:** id 41 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_banking_mandate`, `account_banking_mandate_sale` |
| OCA Deposu | [OCA/bank-payment](https://github.com/OCA/bank-payment) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Production (mandate) / Beta (mandate_sale) |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account_payment_order` (id 39), `account_payment_mode` (id 40); _sale: `account_payment_sale` |
| Kategori | Accounting / Payment |

## Nedir?

Müşteri ve tedarikçiler için **bankacılık talimatı (mandate)** kayıtlarını yönetir: kimin, hangi banka hesabından, hangi ödeme yöntemiyle, hangi tarihten itibaren otomatik tahsilat/ödeme yapılmasına yetki verdiğini kayıt altına alır.

## Ne İşe Yarar?

- **Otomatik tahsilat yetkisi takibi:** Direkt borçlandırma (direct debit) için müşteri talimatının geçerliliği ve süresi kayıtlıdır
- **Fatura-sipariş kontrolü:** Talimat geçerliliği fatura ve satış siparişi üzerinden görülebilir; süresi bitmiş talimatla tahsilat engellenir
- **Talimat geçmişi:** Her talimatın başlangıç/bitiş tarihi, banka hesabı ve durumu izlenir
- **SEPA direct debit altyapısı:** SEPA otomatik tahsilat akışının zorunlu bileşenidir

## Odoo'da Neleri Değiştirir?

- **Ortak kartına** "Bankacılık Talimatları" sekmesi/alanları ekler (banka hesabı, imza tarihi, benzersiz talimat referansı, geçerlilik durumu)
- **Müşteri faturasına** talimat bağlantısı ekler
- **Faturalama → Müşteriler → Borç Emirleri** akışında talimatlı kalemler görünür
- `account_banking_mandate_sale` → satış siparişinde talimat seçimi sağlar

## Nasıl Çalışır?

1. Müşteriden alınan imzalı talimat (mandate) Odoo'ya girilir: banka hesabı, referans, imza tarihi
2. Talimat aktive edilir
3. Otomatik tahsilat gerektiren faturalar bu talimatla ilişkilendirilir
4. Borç emri oluşturulurken talimatlı kalemler tahsilata dahil edilir
5. Talimat iptal/bitiş durumları güncellenir

!!! note "Türkiye uygulaması"
    SEPA direct debit Türkiye'de kullanılmaz. Ancak "ön yetkili/sürekli ödeme talimatı" benzeri süreçlerde müşteri talimatlarının geçerlilik ve süre takibi için modül faydalıdır. SEPA kullanılmayacaksa **öncelik düşük** tutulabilir.

## Kurulum ve Yapılandırma

- Bağımlılıklar: `account_payment_order` (39) ve `account_payment_mode` (40) — bunlar önce kurulmalı
- SEPA direct debit kullanılacaksa `account_banking_sepa_direct_debit` modülü (aynı repo) ayrıca kurulur
- **mandate** Production (kararlı); **mandate_sale** Beta

## Kaynaklar

- [GitHub — account_banking_mandate (19.0)](https://github.com/OCA/bank-payment/tree/19.0/account_banking_mandate)
- [GitHub — account_banking_mandate_sale (19.0)](https://github.com/OCA/bank-payment/tree/19.0/account_banking_mandate_sale)
- [Hata Takibi](https://github.com/OCA/bank-payment/issues)
