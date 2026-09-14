# Fatura Gruplama ve Otomatik Faturalama (`sale_order_invoicing_grouping_criteria` + `partner_invoicing_mode`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 23/24 · **Proje Kartı:** id 48 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `sale_order_invoicing_grouping_criteria`, `partner_invoicing_mode` |
| OCA Deposu | [OCA/account-invoicing](https://github.com/OCA/account-invoicing) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Production / Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | grouping: `sale_management`; mode: `account`, `sale`, `base_partition` + `base_view_inheritance_extension` (OCA/server-tools), `queue_job` (OCA/queue) |
| Kategori | Accounting / Invoicing |

## Nedir?

İki ilişkili modül:

| Modül | İşlev |
|---|---|
| `sale_order_invoicing_grouping_criteria` | Satış siparişlerinin **faturada nasıl gruplanacağını** tanımlayan ölçütler (ortak, ürün, sipariş, analitik dağıtım, tarih…) |
| `partner_invoicing_mode` | Ortak bazında **otomatik faturalama modu** temeli; zamanlanmış iş (cron) ile toplu fatura üretimi; sipariş bazında "One Invoice Per Order" |

## Ne İşe Yarar?

- **Periyodik toplu faturalama:** Haftalık/aylık teslimatları **tek faturada** birleştirme; "her sipariş için ayrı fatura istemiyorum" talepleri
- **Esnek gruplama:** Farklı seviyelerde gruplama: ortak + para birimi + satış siparişi + ürün + analitik + tarih kombinasyonları
- **Otomatik üretim:** Cron ile standard faturalama modunda bekleyen siparişler otomatik faturalanır
- **Ortak bazlı kural:** Her müşteriye farklı faturalama davranışı atanır

## Odoo'da Neleri Değiştirir?

### Gruplama Ölçütleri
- **Faturalama → Yapılandırma → Yönetim → Faturalama Gruplama Ölçütleri** menüsü
- Ölçütlerde satış siparişi başlık modeli alanları seçilir (ortak, sipariş, ürün, analitik, tarih…)
- **Faturalama adresi ve para birimi her zaman** temel ölçütlere eklenir
- Müşteri kartında müşteriye özel gruplama ölçütü atanabilir
- *Satış → Faturalanacak → Faturalanacak Siparişler* akışında **Create Invoices** bu ölçütlere göre fatura oluşturur

### Otomatik Faturalama Modu
- Ortak kartında **Invoicing Mode** alanı ve **Next Invoice Date** (sonraki fatura tarihi) eklenir
- Satış siparişi → *Diğer Bilgiler → Faturalama ve Ödemeler* bölümünde **One Invoice Per Order** seçeneği
- **Zamanlanmış iş (cron):** "Generate Standard Invoices" — varsayılan olarak **arşivli** gelir; etkinleştirilmesi gerekir
- Otomatik fatura üretimi **queue_job** ile arka planda çalışır

## Nasıl Çalışır?

1. *Faturalama Gruplama Ölçütleri* tanımlanır (ör. ortak + analitik)
2. Ortaklara uygun faturalama modu atanır; gerekirse sipariş bazında "One Invoice Per Order" işaretlenir
3. **Manuel akış:** *Faturalanacak Siparişler* listesinden seç → **Create Invoices** → ölçütlere göre fatura(lar) oluşur
4. **Otomatik akış:** Geliştirici modu → *Ayarlar → Otomasyon → Zamanlanmış İşlemler* → arşivden 'Generate Standard Invoices' aktifleştirilir; sıklık ayarlanır
5. Ortak kartındaki **Next Invoice Date** bir sonraki otomatik fatura zamanını gösterir

!!! warning "Kurulum öncesi önemli — queue_job bağımlılığı"
    `partner_invoicing_mode`, `queue_job` modülüne bağımlıdır ve bu modül **`openupgradelib` python paketi** gerektirir. Container imajında bu paket yoktur ve **container recreate'te pip ile yapılan kurulum silinir**. Canlı aktivasyon öncesi kalıcı çözüm (özel imaj) hazırlanmalıdır:
    ```bash
    docker exec odoo-web-1 pip install --break-system-packages openupgradelib
    ```

## Kurulum ve Yapılandırma

- `base_partition` ve `base_view_inheritance_extension` server-tools mount'undan, `queue_job` OCA/queue mount'undan gelir
- Cron varsayılan olarak **arşivlidir**; aktivasyon bilinçli yapılmalı ve **ilk çalıştırma mutlaka test instance'ında** izlenmelidir
- Otomatik faturalama, fatura kesme yetkisine sahip sistem kullanıcısıyla çalışır; muhasebe kontrol süreci planlanmalıdır
- **grouping_criteria Production**, **partner_invoicing_mode Beta** olgunlukta

## Kaynaklar

- [GitHub — sale_order_invoicing_grouping_criteria (19.0)](https://github.com/OCA/account-invoicing/tree/19.0/sale_order_invoicing_grouping_criteria)
- [GitHub — partner_invoicing_mode (19.0)](https://github.com/OCA/account-invoicing/tree/19.0/partner_invoicing_mode)
- [Hata Takibi](https://github.com/OCA/account-invoicing/issues)
