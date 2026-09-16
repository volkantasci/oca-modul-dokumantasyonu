# Sevkiyattan Fatura (`stock_picking_invoicing`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 22/24 · **Proje Kartı:** id 49 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `stock_picking_invoicing` |
| OCA Deposu | [OCA/account-invoicing](https://github.com/OCA/account-invoicing/tree/19.0/stock_picking_invoicing) |
| Sürüm | 19.0.1.0.1 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `stock`, `account`, `stock_picking_invoice_link` (OCA/stock-logistics-workflow), `base_view_inheritance_extension` (OCA/server-tools) |
| Kategori | Accounting / Invoicing |

## Nedir?

Satış siparişi yerine **sevkiyat (picking) bazlı faturalama** sağlar: teslim edilen mallar için doğrudan sevkiyat kaydından fatura oluşturulur.

## Ne İşe Yarar?

- **Sevkiyat bazlı faturalama:** Her teslimat (veya seçilen birkaçı) ayrı ayrı ya da toplu faturalanır
- **Teslim edilen miktar garantisi:** Fatura, sevkiyatta gerçekleşen miktarlar üzerinden oluşur; henüz teslim edilmemiş ürünler faturaya sızmaz
- **İade yönetimi:** İade sevkiyatları için iade faturası akışı
- **İzlenebilirlik:** Fatura-irsaliye bağlantısı kayıtlı kalır; "bu fatura hangi sevkiyatlardan oluştu" sorgulanır

## Odoo'da Neleri Değiştirir?

- **Sevkiyat (picking) formuna fatura durumu** ve aksiyon butonu ekler: durum *"Faturalanacak"* ise **Fatura Oluştur** butonu görünür
- **Sevkiyat listesinde** çoklu seçim ile **gruplu fatura** oluşturma aksiyonu
- Sevkiyatta en az bir fatura oluştuysa yeni bir **"Invoicing" sekmesi** açılır (oluşan faturalar listelenir)
- Fatura iptal/silinirse ilgili sevkiyatın fatura durumu otomatik **"Faturalanacak"** olur

## Nasıl Çalışır?

1. Satış siparişi sevk edilir; teslim edilen picking oluşur
2. Picking ekranında fatura durumu kontrol edilir
3. Tek picking'den **Fatura Oluştur** veya listeden çoklu seçimle **gruplu fatura** üretilir
4. Fatura taslak olarak oluşur; satırlar teslim miktarlarına göre dolar
5. Fatura onaylanır; Invoicing sekmesinden ilişki izlenir

## Kurulum ve Yapılandırma

- Bağımlılık `stock_picking_invoice_link` bu instance'ta **zaten mount edilmiş** durumdadır (OCA/stock-logistics-workflow)
- `base_view_inheritance_extension` server-tools mount'undan gelir
- **Beta** olgunluk; teslim-iptal-iyi iade senaryoları test instance'ında denenmelidir
- Faturalama politikası (sipariş bazlı mı sevkiyat bazlı mı) satış süreciyle birlikte netleştirilmelidir

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_invoice` | Faturalama | Sevkiyattan fatura aksiyonları (kaynakta 12 referans) |
| `stock.group_stock_user` | Kullanıcı (Stok) | Sevkiyat kayıtlarına erişim |
| `base.group_no_one` | Teknik Özellikleri | Teknik görünümler (opsiyonel) |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — stock_picking_invoicing (19.0)](https://github.com/OCA/account-invoicing/tree/19.0/stock_picking_invoicing)
- [Hata Takibi](https://github.com/OCA/account-invoicing/issues)
