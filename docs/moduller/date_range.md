# Tarih Aralıkları (`date_range` + `date_range_account`) — Bağımlılık Modülü

!!! info "Kart Bilgisi"
    **İlişkili Kart:** 1 (Genel Finansal Raporlar) bağımlılığı ile geldi · `date_range_account` otomatik kuruldu · **Test Durumu:** ✅ PASS · **Aktivasyon:** ✅ 15.09.2026 (`date_range`, scratch DB)

| Alan | Değer |
|---|---|
| Teknik Ad | `date_range`, `date_range_account` |
| OCA Deposu | [OCA/server-ux](https://github.com/OCA/server-ux) |
| Sürüm | 19.0.1.0.0 (her ikisi) |
| Olgunluk | Mature / Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `date_range` → `web`; `date_range_account` → `account` + `date_range` (**auto_install**) |
| Kategori | Technical / Accounting |

## Nedir?

**`date_range`**, muhasebe ve raporlama için **yeniden kullanılabilir dönem kayıtları** (ör. "Ocak 2026", "2026 1. Çeyrek", "2026 Mali Yılı") tanımlamanızı sağlayan altyapı modülüdür. **`date_range_account`** ise bu yönetim ekranlarını Faturalama uygulamasına taşıyan köprü modüldür ve `account` + `date_range` kurulu olduğunda **kendiliğinden (auto_install)** kurulur.

!!! warning "Mali dönem kilidi değildir"
    Tarih aralıkları **yalnızca rapor filtresidir**: fiş kilitlemez, kapanış yapmaz, muhasebe kaydını etkilemez. Mali dönem kilidi için farklı mekanizmalar (ör. `account_journal_lock_date`) kullanılır.

## Ne İşe Yarar?

- **Raporlarda hızlı dönem seçimi:** Her raporda başlangıç/bitiş tarihi yazmak yerine tek seçimle dönem uygulanır
- **Standart dönem isimlendirmesi:** Ay/çeyrek/yıl gibi dönemler tek merkezden yönetilir; tüm kullanıcılar aynı dönem tanımını kullanır
- **Toplu üretim:** Jeneratör sihirbazı ile (tür + başlangıç + adet + birim) dönemler tek işlemde oluşturulur
- **Otomatik üretim:** Türde otomatik oluşturma tanımlanırsa günlük çalışan zamanlanmış iş dönemleri kendiliğinden açar
- **Veri tutarlılığı:** Çakışma kontrolü ile aynı türde örtüşen dönemler engellenebilir

## Nerede Kullanılır?

| Yer | Kullanım |
|---|---|
| **Genel Finansal Raporlar** (kart 1) | Mizan, genel defter, açık kalemler, yaşlandırma, günlük defteri, KDV raporu sihirbazlarında **Date Range / Tarih Aralığı** alanı |
| **Vergi Bakiyeleri** (kart 2) | Dönemsel KDV bakiyesi raporunda tarih aralığı seçimi |
| **MIS Builder** (kart 19) | Rapor örneklerinin dönem/sütun tanımları |
| Menüler | `date_range_account` ile: **Faturalama → Yapılandırma → Tarih Aralıkları** (menü sırası 200 — en altta) |

## Türler (`date.range.type`) ve Alanlar

| Alan | Açıklama |
|---|---|
| `name` | Tür adı (çevrilebilir; şirket + ad bazında benzersiz) |
| `unit_of_time` | Birim: **Gün / Hafta / Ay / Yıl** — jeneratörün varsayılan birimi olur |
| `allow_overlap` | Açıksa aynı türdeki dönemler çakışabilir; kapalıysa (varsayılan) çakışma **engellenir** |
| `autogeneration_unit` + `autogeneration_count` | Otomatik üretim: her N gün/hafta/ay/yıl için yeni dönem açılır |
| `company_id`, `active` | Şirket bazlı tanım ve arşivleme |

Dönem kaydı (`date.range`) alanları: `name`, `type_id`, `date_start`, `date_end`, `company_id`. Çakışma kontrolü PostgreSQL `daterange` fonksiyonu ile yapılır.

## Nasıl Çalışır?

1. **Tür tanımlama:** Faturalama → Yapılandırma → Tarih Aralıkları → **Tarih Aralığı Türleri** (ör. "Ay", "Çeyrek", "Mali Yıl")
2. **Dönem üretimi:**
   - **Elle:** Tarih Aralıkları menüsünden tek tek
   - **Jeneratörle:** *Tarih Aralıkları Oluştur* sihirbazı — tür, başlangıç tarihi, adet, birim
   - **Otomatik:** Türde otomatik üretim tanımlıysa cron (`Auto-generate date ranges`, günlük) dönemleri açar
3. **Kullanım:** Rapor sihirbazında *Tarih Aralığı* seçilir → başlangıç/bitiş tarihleri otomatik dolar (isteğe bağlı elle değiştirilebilir)

## Kurulum ve Yapılandırma

- `date_range`, **Mature** olgunlukta kararlı bir modüldür; ek yapılandırma gerektirmez
- `date_range_account` **auto_install**'dır: `account` ve `date_range` kurulu olduğu anda kendiliğinden kurulur (kart 1 aktivasyonunda otomatik geldi)
- İlk kullanımda **hiç dönem kaydı gelmez**; tür + dönem tanımları iş biriminin raporlama alışkanlığına göre yapılmalıdır

!!! note "Çeviri düzeltmesi (12-15.09.2026)"
    `date_range_account` modülünün Weblate kaynaklı `tr.po` dosyasında Türkçe karakterler bozuktu (`Tarih Aral??klar??`). Canlı DB kayıtları ve yerel çeviri dosyası düzeltildi. Kalıcı çözüm (upstream PR + Weblate doğrulaması) **kart 57** ve alt görevlerinde takip edilmektedir. Benzer bozulmalar için tarama:

    ```bash
    cd /home/odoo/deployments/odoo-instances/odoo/repos && \
    find . -path "*/i18n/tr*.po" -exec grep -l '??' {} +
    ```

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `base.group_no_one` | Teknik Özellikleri | Ayarlar → Teknik → Tarih Aralıkları menüleri |
| `account.group_account_manager` | Yönetici | Faturalama → Yapılandırma → Tarih Aralıkları menüleri |
| (rapor yetkisi) | Muhasebe grupları | Raporlardaki tarih aralığı seçimi, raporun kendi yetkisiyle çalışır |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — date_range (19.0)](https://github.com/OCA/server-ux/tree/19.0/date_range)
- [GitHub — date_range_account (19.0)](https://github.com/OCA/server-ux/tree/19.0/date_range_account)
- [Runboat — OCA/server-ux](https://runboat.odoo-community.org/builds?repo=OCA/server-ux&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/server-ux/issues)
