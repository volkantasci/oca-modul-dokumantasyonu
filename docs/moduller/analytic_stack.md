# Analitik Dağıtım Kuralları (`account_analytic_required` + `purchase_analytic` + `stock_analytic`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 17/24 · **Proje Kartı:** id 55 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_analytic_required`, `purchase_analytic`, `stock_analytic` |
| OCA Deposu | [OCA/account-analytic](https://github.com/OCA/account-analytic) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`; purchase_analytic: `purchase` + `base_view_inheritance_extension`; stock_analytic: `stock_account` + `analytic` |
| Kategori | Accounting / Analytic |

## Nedir?

Analitik (maliyet merkezi) dağıtımını tüm süreçlerde **tutarlı ve zorunlu** kılan üç modüllük settir:

| Modül | Görev |
|---|---|
| `account_analytic_required` | Hesap bazında **analitik politika** uygular (opsiyonel / önerilen / **zorunlu**) |
| `purchase_analytic` | Satın alma siparişi ve satırlarına **analitik dağıtım** alanı ekler |
| `stock_analytic` | Stok hareketlerine analitik dağıtım atar; envanter fişlerine yansıtır |

## Ne İşe Yarar?

- **Veri kalitesi:** "Zorunlu" hesap türlerinde (ör. gider hesapları) analitik dağıtım girilmeden fiş kaydedilemez
- **Maliyet merkezi raporlaması:** Proje/departman/maliyet merkezi bazlı kârlılık raporları güvenilir olur (MIS raporlarıyla birlikte)
- **Satın alma aşamasında dağıtım:** Analitik bilgi sipariş aşamasında girilir, fiş aşamasında unutulmaz
- **Stok maliyet dağıtımı:** Sevkiyat/envanter hareketlerindeki maliyetler ilgili analitik hesaba yansır

## Odoo'da Neleri Değiştirir?

- **Hesap planında** her hesap için `Analytic Policy` alanı: *Optional*, *Recommended*, *Always*
- **Satın alma siparişi** (başlık + satır) üzerinde `Analytic Distribution` alanı ve varsayılan dağıtım
- **Stok hareketi / sevkiyat / sayım** ekranlarında `Analytic Distribution` alanı
- **Analitik Planlar** yapılandırmasına **"Stock Move" uygulanabilirlik seçeneği** ekler
- Stok hareketinden otomatik fiş üretildiğinde:
  - Ürün değerleme hesabındaki satıra analitik **atanmaz**
  - Diğer hesaplardaki satırlara stok hareketinin analitik dağıtımı **işlenir**

## Nasıl Çalışır?

1. *Faturalama → Yapılandırma → Analitik Planlar* açılır, gerekli planların uygulanabilirliği (özellikle Stock Move) ayarlanır
2. Hesap planında ilgili hesaplara politika atanır (ör. gider hesapları = Always)
3. Satın alma ve stok süreçlerinde analitik alanlar doldurulur
4. Fiş oluşurken politika uygulanır; eksik dağıtımda **hata verilir**

!!! warning "Geçiş planlaması"
    Politika "zorunlu" (Always) seçildiğinde mevcut açık fişlerde analitik girilmesi zorunlu hale gelebilir. Canlıya almadan önce **test instance'ında geçiş senaryosu** planlanmalıdır.

## Kurulum ve Yapılandırma

- `purchase_analytic` bağımlılığı `base_view_inheritance_extension` server-tools mount'undan gelir
- `stock_analytic` için çekirdek `analytic` ve `stock_account` modülleri gerekir
- Üç modül birlikte kurulmalıdır; tek başına politikalar anlamlı çalışmaz

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `analytic.group_analytic_accounting` | Analitik Muhasebe | Üç modülün tamamı analitik dağıtım alanlarını bu grupla gösterir |
| `account.group_account_user` | Bütün Muhasebe Hesaplarını Göster | Analitik zorunluluk politikası ve hesap planı düzenleme |
| `account.group_account_invoice` / `account.group_account_readonly` | Faturalama / Salt Okunur | Fatura, fiş ve stok maliyet satırlarında dağıtım kullanımı |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_analytic_required (19.0)](https://github.com/OCA/account-analytic/tree/19.0/account_analytic_required)
- [GitHub — purchase_analytic (19.0)](https://github.com/OCA/account-analytic/tree/19.0/purchase_analytic)
- [GitHub — stock_analytic (19.0)](https://github.com/OCA/account-analytic/tree/19.0/stock_analytic)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/account-analytic&target_branch=19.0)
