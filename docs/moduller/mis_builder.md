# MIS Rapor Tasarımcısı (`mis_builder` + `mis_builder_budget`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 19/24 · **Proje Kartı:** id 35 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `mis_builder`, `mis_builder_budget` |
| OCA Deposu | [OCA/mis-builder](https://github.com/OCA/mis-builder) |
| Sürüm | 19.0.1.2.0 / 19.0.1.0.1 |
| Olgunluk | **Production** (her ikisi de) |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account`, `board`, `report_xlsx` (OCA/reporting-engine), `date_range` (OCA/server-ux) |
| Kategori | Accounting / Reporting |

## Nedir?

**Yönetim Bilgi Sistemi (MIS)** raporlarını sıfırdan tasarlamayı sağlayan güçlü rapor motorudur. Rapor **şablonları** (KPI tanımları) zamandan bağımsızdır; **rapor örnekleri** (instance) şablonu dönemlere bağlar ve sütunları oluşturur. `mis_builder_budget` modülü bütçe takibini ekler.

## Ne İşe Yarar?

- **Özel yönetim raporları:** Bilanço/gelir tablosu benzeri veya şirkete özel raporlar (ciro, brüt kâr, maliyet, KPI) tasarlanır
- **Formül gücü:** Hesap toplamları, oranlar, toplam-eksi-alt toplam, `+`/`-`/`%` işlemleri; koşullu hesaplar
- **Çoklu dönem karşılaştırması:** Aylık/çeyrek/yıllık yan yana sütunlar; gerçekleşen-gerçekleşen, gerçekleşen-bütçe karşılaştırmaları
- **Pano (dashboard) entegrasyonu:** Raporlar Odoo panosuna eklenir; PDF/XLSX dışa aktarılır
- **Hücre notları:** Rapor hücrelerine açıklama (annotation) eklenir; PDF/XLSX çıktısına basılır

## Odoo'da Neleri Değiştirir?

- **Muhasebe → Yapılandırma → MIS Reporting → MIS Report Templates** menüsü (rapor tasarımı)
- **Muhasebe → Raporlar → MIS Reporting → MIS Reports** menüsü (dönem bağlı rapor örnekleri)
- MIS Reports görünümünde: **Preview**, panoya ekleme, **PDF/Excel export**, hücre notları
- KPI tanımlarında: hesap kodları, formüller, sütun düzeni, birikim yöntemi
- `mis_builder_budget` ile: **MIS Budget (by KPIs)** ve **GL Account bazlı** bütçe ekranları; raporlarda bütçe sütunu

## Nasıl Çalışır?

### Rapor tanımı
1. **Template** oluşturulur; KPI'lar (satırlar) tanımlanır — hesap/ifade/formül
2. KPI'lar için toplama yöntemi ve işaret kuralları belirlenir
3. **Instance** oluşturulur: şablon + dönemler (sütunlar) bağlanır
4. **Preview** ile sonuç görülür; panoya/PDF/XLSX'e aktarılır

### Bütçe (mis_builder_budget)
- **KPI bazlı:** KPI'larda "budgetable" işaretlenir; `Sum` (tutarlar) veya `Average` (çalışan sayısı vb.) birikim yöntemi seçilir; budget item'larla dönemsel bütçe girilir
- **GL hesap bazlı:** Doğrudan muhasebe hesapları üzerinden bütçe satırları
- İki yöntem aynı bütçede birleştirilemez

## Kurulum ve Yapılandırma

- `report_xlsx` + `date_range` bağımlılıkları hazır mount edilmiştir (dönem yönetimi için bkz. [Tarih Aralıkları](date_range.md))
- **Production** olgunluk — üretimde güvenle kullanılabilir
- Rapor tasarımı iş birimiyle birlikte yapılmalı; hesap kodları ve analitik boyutlar planlanmalıdır
- Analitik raporlama için id 55 (analitik kuralları) ile birlikte en güçlü etkiyi verir

## Kaynaklar

- [GitHub — mis_builder (19.0)](https://github.com/OCA/mis-builder/tree/19.0/mis_builder)
- [GitHub — mis_builder_budget (19.0)](https://github.com/OCA/mis-builder/tree/19.0/mis_builder_budget)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/mis-builder&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/mis-builder/issues)
