# Tekrarlayan Fiş Şablonları (`account_move_template`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 5/24 · **Proje Kartı:** id 50 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_move_template` |
| OCA Deposu | [OCA/account-financial-tools](https://github.com/OCA/account-financial-tools/tree/19.0/account_move_template) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account` |
| Kategori | Accounting |

## Nedir?

Yinelenen muhasebe fişlerini (günlük kayıtları) **şablon olarak tanımlayıp** tek tıkla fişe dönüştüren modüldür. Kira, aidat, abonelik, sabit aylık gider/gelir kayıtları gibi tekrarlayan işlemler içindir.

## Ne İşe Yarar?

- **Zaman tasarrufu:** Her ay elle yazılan fişler şablondan saniyeler içinde üretilir
- **Hata azaltma:** Hesap, tutar ve analitik dağıtım şablonda sabit olduğundan tutarlılık garanti
- **Kademeli giriş:** Şablon satırları **formülle** hesaplanabilir veya kullanıcıdan **girdi istenebilir** (ör. tutar sorulur)
- **Fatura dışı giderler:** Tedarikçi faturası olmayan kayıtlar (amortisman dışı, kira stopajı vb.) için standart akış

## Odoo'da Neleri Değiştirir?

- **Faturalama → Yapılandırma → Muhasebe → Fiş Şablonları** menüsü ekler (şablon tanımı)
- **Faturalama → Muhasebe → Aksiyonlar → Şablondan Fiş Oluştur** sihirbazı ekler
- Şablon satırlarında üç tamamlama türü:
  - **Sabit değer:** hesap + tutar doğrudan yazılır
  - **Formül:** diğer satırlara göre hesaplanır (ör. toplamın %20'si)
  - **Kullanıcı girdisi:** sihirbaz açıldığında tutar sorulur
- Üretilen fiş taslak olarak kaydedilir; kontrol edilip onaylanır

## Nasıl Çalışır?

1. **Şablon tanımı:** *Fiş Şablonları* menüsünden şablon oluşturulur; her satır için hesap, etiket, tutar/formül/girdi türü belirlenir
2. **Kullanım:** *Şablondan Fiş Oluştur* sihirbazı açılır, şablon seçilir
3. Girdi bekleyen alanlar doldurulur
4. **Generate Journal Entry** ile taslak fiş üretilir
5. Gözden geçirilip kaydedilir/onaylanır

!!! note "Yetki"
    Şablon tanımlamak için kullanıcıda *Tam Muhasebe Özellikleri* ve **Faturalama Sorumlusu** yetkisi gerekir.

## Kurulum ve Yapılandırma

- Yalnızca `account` bağımlılığı; ek yapılandırma yok
- Şablonlarda **analitik dağıtım** da tanımlanabilir (analitik modülleriyle uyumlu)
- **Beta** olgunluk — basit ve düşük riskli; test instance'ında bir şablonla doğrulama yeterli

## Kaynaklar

- [GitHub — account_move_template (19.0)](https://github.com/OCA/account-financial-tools/tree/19.0/account_move_template)
- [Hata Takibi](https://github.com/OCA/account-financial-tools/issues)
