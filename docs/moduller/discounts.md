# İskonto Yönetimi (`account_global_discount` + `account_invoice_triple_discount`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 21/24 · **Proje Kartı:** id 47 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_global_discount`, `account_invoice_triple_discount` |
| OCA Deposu | [OCA/account-invoicing](https://github.com/OCA/account-invoicing) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account` + `base_global_discount` (OCA/server-backend) / `account` |
| Kategori | Accounting / Invoicing |

## Nedir?

İki tamamlayıcı iskonto modülüdür:

| Modül | İşlev |
|---|---|
| `account_global_discount` | Belge genelinde (tüm fatura satırlarına) uygulanan **genel iskonto**; ortak kartına bağlanabilir |
| `account_invoice_triple_discount` | Fatura satırında **3 ayrı iskonto alanı** (zincirleme yüzde) |

## Ne İşe Yarar?

### Genel İskonto
- Belirli müşteri gruplarına **belge bazında iskonto** (ör. tüm faturaya %5)
- İskonto kapsamı: satış veya satın alma; şirket bazında kısıtlama
- Faturada genel iskonto için ayrı **muhasebe satırları** oluşturulur; vergi satırlarına iskonto oranı yansıtılır

### Üçlü İskonto
- **% + % + %** zincirleme iskontolu fiyatlandırma (bayi/kanal iskontoları)
- Sıralı hesaplama: 2. iskonto 1.'nin sonucu üzerine, 3. iskonto 2.'nin sonucu üzerine uygulanır
- **Negatif değer** ile iskonto yerine ek ücret (markup) girilebilir

**Örnek (zincirleme):** 600,00 → %50 → 300,00 → %50 → 150,00 → %50 → 75,00

## Odoo'da Neleri Değiştirir?

- **Ayarlar → Parametreler → Genel İskontolar** yapılandırma ekranı (oran + kapsam + şirket)
- **Ortak kartı → Satış ve Satın Alma sekmesi:** satış/satın alma genel iskonto alanları
- **Fatura formu:**
  - `Invoice Global Discounts` alanı (ortaktan otomatik dolar, değiştirilebilir)
  - Toplam alanlarında genel iskonto etkisi
  - `Diğer Bilgiler` sekmesinde satır bazında uygulanan genel iskontolar tablosu
  - Journal Items sekmesinde iskonto yansımaları (vergi oranı + iskonto satırları)
- Fatura satırlarında `Disc. 1`, `Disc. 2`, `Disc. 3` alanları ve hesaplanmış alt toplam

!!! warning "Vergi kombinasyonları"
    Genel iskonto ile her vergi kombinasyonu uyumlu değildir; olağan dışı vergi yapılandırmalarında journal item sonuçları doğrulanmalıdır (modülün bilinen sınırı). `account_invoice_triple_discount`, Enterprise tarzı `account_invoice_fixed_discount` modülü ile çakışır (manifest'te `excludes` var).

## Nasıl Çalışır?

1. **Tanımlama:** Genel iskontolar yapılandırmada veya ortak kartında tanımlanır
2. **Faturalama:** Fatura oluşturulur; genel iskonto alanı ortaktan gelir
3. Satırlarda üçlü iskonto girilir (gerekirse)
4. Toplamlar ve journal item'lar iskonto dahil hesaplanır
5. Onay ile muhasebeye işlenir

## Kurulum ve Yapılandırma

- `base_global_discount` bağımlılığı OCA/server-backend mount'undan gelir
- İskonto tanımları için kullanıcıya **Manage Global Discounts** yetkisi verilmelidir
- Fiyatlandırma politikası (iskontonun liste fiyatına mı net fiyata mı uygulandığı) muhasebe/satış ile netleştirilmelidir

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `base_global_discount.group_global_discount` | Manage Global Discounts (Genel İskontoları Yönet) | Genel iskonto tanımlama ve ortak/faturaya atama |
| `analytic.group_analytic_accounting` | Analitik Muhasebe | Genel iskonto dağıtım alanları |
| `account.group_account_invoice` | Faturalama | İskontolu fatura oluşturma |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_global_discount (19.0)](https://github.com/OCA/account-invoicing/tree/19.0/account_global_discount)
- [GitHub — account_invoice_triple_discount (19.0)](https://github.com/OCA/account-invoicing/tree/19.0/account_invoice_triple_discount)
- [Hata Takibi](https://github.com/OCA/account-invoicing/issues)
