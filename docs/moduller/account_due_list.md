# Vade Listesi (`account_due_list`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 3/24 · **Proje Kartı:** id 46 · **Test Durumu:** ✅ PASS · **Aktivasyon:** ✅ 16.09.2026

| Alan | Değer |
|---|---|
| Teknik Ad | `account_due_list` |
| OCA Deposu | [OCA/account-payment](https://github.com/OCA/account-payment/tree/19.0/account_due_list) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Production |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account` |
| Kategori | Accounting / Payment |

## Nedir?

Vadesi gelen ve gelecek **açık alacak/borç kalemlerini tek listede** gösteren ekran modülüdür. Odoo Community'de açık kalemler yalnızca ortak kartı içinde görülebildiği için günlük ödeme/tahsilat takibini zorlaştırır; bu modül merkezî bir vade listesi ekler.

## Ne İşe Yarar?

- **Nakit akışı planlaması:** Hangi fatura ne zaman ödenecek/tahsil edilecek tek bakışta
- **Gecikmiş kalem takibi:** Vadesi geçen kalemlerin filtrelenmesi
- **Ödeme emri hazırlığı:** Toplu ödeme emrine (bkz. `account_payment_order`) girecek kalemleri seçmek için pratik kaynak liste
- **Önceliklendirme:** Vade tarihine göre sıralı çalışma listesi

## Odoo'da Neleri Değiştirir?

- **Faturalama → Muhasebe → Ödemeler ve Vade listesi** menüsü ekler
- Liste görünümünde: hareket kalemi, ortak, fatura no, vade tarihi, tutar, kalan tutar, para birimi sütunları
- Vade tarihi, gecikme günü ve ödeme durumuna göre **hazır filtreler ve gruplamalar** ekler
- Kalemden ilgili faturaya/muhasebe kaydına doğrudan geçiş sağlar

!!! note "Yetki gereksinimi"
    Menü, Faturalama içindeki **Muhasebe** bölümü altındadır; bu bölüm **"Muhasebe Özelliklerini Göster - Salt Okunur"** (`account.group_account_readonly`) grubuna bağlıdır. Bu grup yoksa menü hiç görünmez. 16.09.2026'da kullanıcıya **"Bütün Muhasebe Hesaplarını Göster"** (`account.group_account_user`) grubu verilerek bölüm görünür yapıldı — bu grup `readonly`, `basic` ve `invoice` gruplarını da kapsar.

## Saha Notları (Aktivasyon Sonrası)

**Aktivasyon:** 16.09.2026'da canlıda aktive edildi.

**Menü yolu:** **Faturalama → Muhasebe → Ödemeler ve Vade listesi**

**Çeviri durumu:** Modülün `tr.po` dosyası kapsamlıdır (36 çeviri: menü, filtreler, sütun etiketleri → "Vade Tarihi", "Tam Uzlaştı", "Vadesi Geçmiş" vb.) ve dört view'ın tamamında tr_TR çevirisi yüklüdür. **Ek çeviri çalışması gerekmedi.**

**Yetki notu (yaşanan sorun):** Kullanıcıda yalnızca *Muhasebe Yöneticisi* (`group_account_manager`) grubu varken "Muhasebe" bölümü görünmüyordu; çünkü bu grup Odoo 19'da `readonly` grubunu kapsamıyor. `group_account_user` verilerek çözüldü. Yeni muhasebe kullanıcılarında da bu grubun atanmasına dikkat edilmelidir.

## Nasıl Çalışır?

1. Menüden vade listesi açılır
2. Varsayılan filtrede **ödenmemiş ve kısmen ödenmiş** kalemler listelenir
3. Vade tarihi / ortak / gün aşımı filtreleri uygulanır
4. Satır seçilip ödeme/uzlaştırma akışına geçilir

## Kurulum ve Yapılandırma

- Yalnızca `account` modülüne bağımlıdır; ek yapılandırma gerektirmez
- **Production** olgunlukta — güvenle kullanılabilir
- Şirket bazlı çoklu para birimi tutarlarını destekler
- Aynı depodaki `account_due_list_payment_mode` modülü ile ödeme modu bazlı kırılım eklenebilir (opsiyonel)

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `account.group_account_readonly` | Muhasebe Özelliklerini Göster - Salt Okunur | Faturalama → **Muhasebe** bölümü ve vade listesi menüsü (canlıda doğrulandı) |
| `account.group_account_user` | Bütün Muhasebe Hesaplarını Göster | Önerilen; Salt Okunur'u da kapsar, eksiksiz erişim sağlar |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari-hizli-referans) bölümüne bakın.

## Kaynaklar

- [GitHub — account_due_list (19.0)](https://github.com/OCA/account-payment/tree/19.0/account_due_list)
- [Runboat (canlı demo)](https://runboat.odoo-community.org/builds?repo=OCA/account-payment&target_branch=19.0)
- [Hata Takibi](https://github.com/OCA/account-payment/issues)
