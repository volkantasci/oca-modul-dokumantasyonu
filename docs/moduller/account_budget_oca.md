# Bütçe Yönetimi (`account_budget_oca`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 18/24 · **Proje Kartı:** id 56 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_budget_oca` |
| OCA Deposu | [OCA/account-budgeting](https://github.com/OCA/account-budgeting/tree/19.0/account_budget_oca) |
| Sürüm | 19.0.1.1.0 |
| Olgunluk | Beta |
| Lisans | **LGPL-3** (diğerlerinden farklı) |
| Bağımlılıklar | `account` (analitik hesaplar üzerinden çalışır) |
| Kategori | Accounting / Budget |

## Nedir?

Standart Community bütçe modülünün (**`account_budget`**) OCA tarafından geliştirilmiş ve bakımı yapılan sürümüdür. **Analitik plan/hesap bazında bütçe** tanımlamayı ve gerçekleşen tutarlarla karşılaştırmayı sağlar.

!!! warning "Standart modülü devre dışı bırakır"
    Manifest'te `excludes: account_budget` tanımlıdır; kurulumda çekirdek bütçe modülü **otomatik devre dışı** bırakılır. Çakışma beklenmez, ancak daha önce standart modülle girilmiş bütçe kayıtları varsa gözden geçirilmelidir.

## Ne İşe Yarar?

- **Maliyet merkezi bütçesi:** Analitik hesap (departman, proje, maliyet merkezi) bazında gelir/gider bütçesi
- **Gerçekleşen karşılaştırması:** Bütçe satırları ile gerçekleşen muhasebe tutarlarının karşılaştırması
- **Bütçe durumları:** Draft → Confirm → Approve → Cancel iş akışı ile kontrollü bütçe yönetimi
- **Dönemsel kırılım:** Bütçe satırlarında tarih aralığı ve dönem bazlı planlama

## Odoo'da Neleri Değiştirir?

- **Muhasebe menüsüne Bütçeler** bölümü ekler: *Budgets* (bütçe kartları), *Budgetary Positions* (bütçe pozisyonları)
- Bütçe kartı aksiyonları: **Confirm**, **Approve**, **Cancel**, **Draft**
- **Analitik hesap** kartına bütçe bağlantıları ekler
- Bütçe satırları (Budget Lines) üzerinden analitik hesap + dönem + planlanan tutar tanımı
- Bütçe durumuna göre kullanıcı yetkileri (onay mercii ayrımı)

## Nasıl Çalışır?

1. *Bütçeler → Yeni* kaydı açılır (ad, dönem, şirket)
2. **Budgetary Position** ile hangi analitik hesapların bütçeleneceği tanımlanır
3. Bütçe satırları girilir (analitik hesap, dönem, tutar)
4. **Confirm → Approve** ile bütçe yürürlüğe girer
5. Gerçekleşen tutarlar analitik dağıtımdan otomatik okunur; karşılaştırma raporlanır

!!! tip "mis_builder_budget alternatifi"
    Daha esnek, KPI/formül tabanlı bütçeleme için `mis_builder_budget` (id 35) da mevcuttur. İki yöntemin aynı anda kullanım stratejisi muhasebe birimiyle netleştirilmelidir.

## Kurulum ve Yapılandırma

- Analitik dağıtımın düzenli girilmesi gerekir; bu yüzden **analitik kuralları (id 55) önce kurulur**
- **Beta** olgunluk; bütçe/gerçekleşme sorguları test instance'ında kontrol edilmelidir
- Lisansı LGPL-3'tür (diğer kart modüllerinin tamamı AGPL-3)

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_user` | Bütün Muhasebe Hesaplarını Göster | Bütçeler menüleri (kaynakta bu grupla kısıtlı) |
| `analytic.group_analytic_accounting` | Analitik Muhasebe | Bütçe satırları analitik hesap bazlıdır |
| `base.group_no_one` | Teknik Özellikleri | Teknik/gelişmiş görünümler |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_budget_oca (19.0)](https://github.com/OCA/account-budgeting/tree/19.0/account_budget_oca)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/account-budgeting&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/account-budgeting/issues)
