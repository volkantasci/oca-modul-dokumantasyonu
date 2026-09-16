# Uzlaştırma Ekranı (`account_reconcile_oca`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 12/24 · **Proje Kartı:** id 43 · **Test Durumu:** ✅ PASS (44s)

| Alan | Değer |
|---|---|
| Teknik Ad | `account_reconcile_oca` (+ bağımlılık: `account_statement_base`) |
| OCA Deposu | [OCA/account-reconcile](https://github.com/OCA/account-reconcile/tree/19.0/account_reconcile_oca) |
| Sürüm | 19.0.1.0.9 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account_statement_base` (aynı repo) |
| Kategori | Accounting / Reconciliation |

## Nedir?

Odoo **Community'de bulunmayan** gelişmiş **uzlaştırma (reconciliation) ekranını** sağlar. Enterprise'daki banka uzlaştırma widget'ının OCA karşılığıdır; banka ekstrelerini ve "uzlaştırılabilir" işaretli hesapları görsel ekrandan eşleştirmeyi sağlar.

## Ne İşe Yarar?

- **Banka uzlaştırması:** Banka ekstresi satırını fatura, gider veya ödeme kaydıyla tıklayarak eşleştirme
- **Muhasebe uzlaştırması:** Cari/hesap bazlı açık kalemlerin görsel ekrandan eşleştirilmesi
- **Toplu çalışma:** Tek ekranda çok sayıda kalemi hızlıca eşleştirme; öneri ve otomatik eşleştirme kuralları
- **Uzlaştırma modeli:** Tekrarlayan hareketler için otomatik eşleştirme önerileri (reconcile models)

## Odoo'da Neleri Değiştirir?

- **Faturalama → Kontrol Paneli:** Banka günlüğü kartında **"Reconcile"** butonu (Tam Muhasebe yetkisiyle)
- **Muhasebe → Aksiyonlar → Tümünü Uzlaştır** menüsü
- **Hesap ve ortak ekranlarından** aynı uzlaştırma widget'ına erişim
- Banka ekstresi satırı, günlük kalemi ve fatura ekranlarına **uzlaştırma görünümleri** ekler
- Uzlaştırma modelleri yönetim ekranı (otomatik eşleştirme kuralları)

## Nasıl Çalışır?

1. Banka ekstresi içe aktarılır (bkz. `account_statement_import`)
2. *Kontrol Paneli → Banka günlüğü → Reconcile* açılır
3. Ekstre satırları solda, bekleyen kalemler/öneriler sağda listelenir
4. Eşleşen kalem seçilip **Reconcile** ile kapatılır; fark/artık tutar açık kalabilir
5. Kaydedilen uzlaştırmalar muhasebeye işlenir; raporlar (mizan, yaşlandırma) güncellenir

!!! warning "Sıra bağımlılığı — önemli"
    `account_statement_import_base` modülü `account_statement_base`'e bağımlıdır. Bu nedenle **uzlaştırma modülü, ekstre içe aktarma modülünden (13) ÖNCE kurulmalıdır**; aksi durumda içe aktarma modülü kurulamaz.

## Kurulum ve Yapılandırma

- Bağımlılığı `account_statement_base` aynı repodan otomatik gelir
- **Beta** olgunlukta ve arayüz ağırlıklı bir modüldür; canlı kullanım öncesi test instance'ında ekstre uzlaştırma akışı denenmelidir
- Banka ekstresi içe aktarma (13) ile birlikte tam akış oluşturur: **içe aktar → uzlaştır → mutabakat**

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_readonly` | Muhasebe Özelliklerini Göster - Salt Okunur | Uzlaştırma menüleri ve ekranları (kaynakta doğrulandı) |
| `account.group_account_user` | Bütün Muhasebe Hesaplarını Göster | Uzlaştırma işlemi için önerilir ("Full Accounting capabilities") |
| `analytic.group_analytic_accounting` | Analitik Muhasebe | Analitik dağıtım alanları görünürse |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_reconcile_oca (19.0)](https://github.com/OCA/account-reconcile/tree/19.0/account_reconcile_oca)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/account-reconcile&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/account-reconcile/issues)
