# Cari Hesap Ekstresi (`partner_statement`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 4/24 · **Proje Kartı:** id 37 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `partner_statement` |
| OCA Deposu | [OCA/account-financial-report](https://github.com/OCA/account-financial-report/tree/19.0/partner_statement) |
| Sürüm | 19.0.1.1.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`, `report_xlsx`, `report_xlsx_helper` (OCA/reporting-engine) |
| Kategori | Accounting / Reporting |

## Nedir?

Müşteri ve tedarikçiler için **profesyonel hesap ekstresi (statement)** üreten rapor modülüdür. İki tür ekstre sunar:

| Tür | İçerik | Kullanım |
|---|---|---|
| **Activity Statement** (Hareket Ekstresi) | Dönemdeki tüm hareketler (borç/alacak) ve bakiye | Ay sonu cari mutabakatı |
| **Outstanding Statement** (Açık Bakiye Ekstresi) | Açık (ödenmemiş) kalemler + yaşlandırma kovaları | Tahsilat takibi, yaşlandırma |

## Ne İşe Yarar?

- **Müşteri/tedarikçi mutabakatı:** Ay sonlarında taraflara gönderilen standart ekstre belgesi
- **Uyuşmazlık çözümü:** Bakiye farklarının hangi hareketten kaynaklandığının izlenmesi
- **Yaşlandırma analizi:** Açık kalemlerin 30/60/90 gün kovalarında dağılımı
- **Toplu üretim:** Birden fazla ortak seçilip tek seferde ekstre üretilebilir

## Odoo'da Neleri Değiştirir?

- **Ortak / Kişi / Müşteri / Tedarikçi listelerinde** çoklu seçim sonrası:
  - *Aksiyon → Partner Activity Statement*
  - *Aksiyon → Partner Outstanding Statement*
- Ekstre sihirbazı seçenekleri: alacak/borç yönü, yaşlandırma kovaları, yaşlandırma tipi (vade tarihine göre / fatura tarihine göre), vadesi gelmemişleri hariç tutma, negatif bakiyeleri hariç tutma
- **Ayarlar → Faturalama → Partner Statements** bölümünden ekstre türleri kullanıcı tabanında açılır/kapatılır
- Çıktılar **PDF ve XLSX**; antetli şablon desteği

## Nasıl Çalışır?

1. Ortak listesinden bir veya çok ortak seçilir
2. İlgili aksiyon menüsünden ekstre sihirbazı açılır
3. Parametreler seçilir ve rapor üretilir
4. PDF indirilir/yazdırılır, XLSX ile dışa aktarılır

!!! warning "Bilinen sınırlama"
    Bu sürümde **otomatik e-posta gönderimi yoktur**; ekstre üretilip manuel gönderilir (yol haritasında e-posta şablonu ve toplu gönderim bulunuyor).

## Kurulum ve Yapılandırma

- `report_xlsx` ve `report_xlsx_helper` bağımlılıkları reporting-engine mount'undan otomatik karşılanır
- Erişim için kullanıcıda Faturalama yetkisi (Invoicing veya Administrator) gerekir
- **Beta** olgunlukta; canlı öncesi test instance'ında ekstre çıktıları kontrol edilmelidir

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `partner_statement.group_activity_statement` | Use activity statements | Etkinlik ekstresi aksiyonunun görünürlüğü |
| `partner_statement.group_outstanding_statement` | Use outstanding statements | Açık bakiye ekstresi aksiyonunun görünürlüğü |
| `account.group_account_invoice` veya `account.group_account_manager` | Faturalama / Yönetici | Raporu çalıştırmak için muhasebe yetkisi (upstream README) |

Ekstre türleri ayrıca **Ayarlar → Faturalama → Partner Statements** bölümünden kullanıcı tabanında açılıp kapatılır.

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari) bölümüne bakın.

## Kaynaklar

- [GitHub — partner_statement (19.0)](https://github.com/OCA/account-financial-report/tree/19.0/partner_statement)
- [Hata Takibi](https://github.com/OCA/account-financial-report/issues)
