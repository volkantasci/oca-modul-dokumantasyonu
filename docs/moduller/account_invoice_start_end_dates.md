# Hizmet Dönemi Tarihleri (`account_invoice_start_end_dates`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 8/24 · **Proje Kartı:** id 54 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_invoice_start_end_dates` |
| OCA Deposu | [OCA/account-closing](https://github.com/OCA/account-closing/tree/19.0/account_invoice_start_end_dates) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account` |
| Kategori | Accounting |

## Nedir?

Fatura/günlük fiş **satırlarına başlangıç ve bitiş tarihi** alanları ekleyen modüldür. Bir hizmetin hangi döneme ait olduğunu belge üzerinde gösterir (ör. Ocak ayı kirası).

## Ne İşe Yarar?

- **Dönemsel hizmet faturaları:** Kira, abonelik, bakım sözleşmesi, danışmanlık gibi döneme yayılan hizmetlerde ait olunan dönemin kaydı
- **Dönemsel raporlama:** Gelir/giderin hangi döneme ait olduğunun tarih bazlı filtrelenmesi
- **Yenileme takibi:** Bitiş tarihi yaklaşan hizmetlerin listelenmesi
- **Denetim izi:** Fatura ile hizmet dönemi arasındaki ilişkinin belgelenmesi

## Odoo'da Neleri Değiştirir?

- Fatura/günlük fiş **satırlarına yeni alanlar:** `start_date` (başlangıç), `end_date` (bitiş)
- **Ürün kartına varsayılan dönem** tanımı ve **`must_have_dates`** (tarih zorunlu) seçeneği ekler
- *Cut-offs* (dönem kesişimi) görünümleri ve filtreleri ekler
- Alanlar fatura raporlarında ve liste görünümlerinde gösterilir; tarih bazlı arama yapılabilir
- Üründe "tarih zorunlu" işaretlenirse bu ürünü içeren satırda tarih girişi zorunlu olur

## Nasıl Çalışır?

1. Ürün kartında varsayılan dönem uzunluğu ve zorunluluk ayarlanır
2. Fatura oluşturulur; satırda başlangıç/bitiş tarihleri girilir (veya varsayılan gelir)
3. Fatura onaylandığında tarihler muhasebe satırlarına işlenir
4. Dönem bazlı listeler/raporlar ile hizmet dönemi takibi yapılır

## Kurulum ve Yapılandırma

- Yalnızca `account` bağımlılığı; ek ayar yok
- Sektörde vergi/muhasebe açısından **gelir tahakkuku** çalışmalarında muhasebe birimiyle ortak kural belirlenmesi önerilir
- Modül **OCA/account-closing** deposundadır (kart başlığındaki repo bilgisi buna göre güncellendi)

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| — | — | Modül özel bir grup tanımlamaz; alanlar mevcut ekranlara eklenir |
| `account.group_account_invoice` | Faturalama | Fatura satırlarında başlangıç/bitiş tarihi girişi |
| `account.group_account_readonly` | Muhasebe Özelliklerini Göster - Salt Okunur | Fiş ve muhasebe satırlarında alanların görünümü |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_invoice_start_end_dates (19.0)](https://github.com/OCA/account-closing/tree/19.0/account_invoice_start_end_dates)
- [Hata Takibi](https://github.com/OCA/account-closing/issues)
