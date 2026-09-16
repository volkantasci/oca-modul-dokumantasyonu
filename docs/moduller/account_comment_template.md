# Fatura ve Belge Not Şablonları (`account_comment_template`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 6/24 · **Proje Kartı:** id 51 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_comment_template` |
| OCA Deposu | [OCA/account-invoice-reporting](https://github.com/OCA/account-invoice-reporting/tree/19.0/account_comment_template) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`, `base_comment_template` (OCA/reporting-engine) |
| Kategori | Accounting / Reporting |

## Nedir?

Fatura, iade, irsaliye gibi muhasebe belgelerine **şablondan otomatik not/yorum ekleyen** modüldür. `base_comment_template` altyapısını muhasebe belgelerine bağlar.

## Ne İşe Yarar?

- **Standart metinlerin otomasyonu:** IBAN, ödeme koşulu, teslim şartı, garanti metni, banka bilgisi gibi her faturaya elle yazılan notlar şablondan gelir
- **Çok dil desteği:** Türkçe/İngilizce müşterilere farklı dilde not şablonu
- **Ortak bazlı özelleştirme:** Belirli müşteriye özel not kuralları
- **Yazdırılan belgede tutarlılık:** Notlar belge üzerinde (üst/alt) sabit konumda görünür

## Odoo'da Neleri Değiştirir?

- **Faturalama → Yapılandırma → Muhasebe → Belge Yorumları** menüsü (şablon yönetimi) ekler
- Fatura formunda **Comments** alanı ve şablon seçimi genişletilir
- Şablonda dinamik alan kullanımı: fatura tarihi, tutarı, ortak adı gibi alanlar metne gömülebilir
- Şablona göre belge yazdırma çıktısına not bloğu eklenir

## Nasıl Çalışır?

1. *Belge Yorumları* menüsünden şablonlar oluşturulur (metin + koşullar)
2. Fatura oluşturulurken/onaylanırken uygun şablon seçilir (varsayılanlar otomatik gelebilir)
3. Şablonun metni fatura notuna uygulanır
4. PDF çıktısında not görünür

## Kurulum ve Yapılandırma

- `base_comment_template` bağımlılığı reporting-engine mount'undan karşılanır
- Basit, bağımlılığı düşük bir raporlama iyileştirmesidir
- Aynı depodaki `account_invoice_line_report`, `account_invoice_report_grouped_by_picking` gibi modüller opsiyonel olarak eklenebilir

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_manager` | Yönetici | Belge Yorumları şablon yönetimi (Faturalama → Yapılandırma → Muhasebe) |
| `account.group_account_invoice` | Faturalama | Faturalarda not şablonu seçimi ve kullanımı |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari) bölümüne bakın.

## Kaynaklar

- [GitHub — account_comment_template (19.0)](https://github.com/OCA/account-invoice-reporting/tree/19.0/account_comment_template)
- [Hata Takibi](https://github.com/OCA/account-invoice-reporting/issues)
