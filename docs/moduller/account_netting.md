# Karşılıklı Borç-Alacak Netleştirme (`account_netting`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 7/24 · **Proje Kartı:** id 52 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `account_netting` |
| OCA Deposu | [OCA/account-financial-tools](https://github.com/OCA/account-financial-tools/tree/19.0/account_netting) |
| Sürüm | 19.0.1.0.0 |
| Olgunluk | Beta |
| Lisans | AGPL-3 |
| Bağımlılıklar | `account` |
| Kategori | Accounting |

## Nedir?

Aynı ortağın **alacak ve borç kalemlerini karşılıklı kapatarak (netleştirme)** tek fişle mahsup eden sihirbaz modülüdür.

## Ne İşe Yarar?

Bir şirketin hem müşteri hem tedarikçi olduğu durumlarda (ilişkili şirketler, karşılıklı ticaret), karşılıklı borç-alacağın banka hareketi olmadan temizlenmesini sağlar:

- **Nakit akışı optimizasyonu:** Brüt ödeme yapmak yerine net bakiye kapatılır
- **Manuel fiş yükünü kaldırır:** Karşılıklı mahsup fişi elle yazılmaz
- **Açık kalem tutarlılığı:** Netleştirilen kalemler kapanır; vade listesi ve yaşlandırma doğru kalır

## Odoo'da Neleri Değiştirir?

- **Muhasebe → Fiş Kayıtları → Günlük Kalemleri** listesine yeni bir aksiyon ekler: *Action → Compensate*
- Seçilen kalemler alacak (AR) ve borç (AP) hesaplarından değilse veya farklı ortaklara aitse **hata verir**
- Netleştirme sihirbazında sonuç özeti ve **kayıt günlüğü seçimi** sunulur; onaylanınca karşılıklı mahsup fişi oluşturulur

## Nasıl Çalışır?

1. *Muhasebe → Fiş Kayıtları → Günlük Kalemleri* açılır
2. Aynı ortağa ait AR/AP açık kalemleri birlikte seçilir
3. *Action → Compensate* çalıştırılır
4. Sihirbaz net tutarı ve günlük seçimini gösterir
5. **Compensate** onayıyla netleştirme fişi üretilir; fark varsa açık kalır

## Kurulum ve Yapılandırma

- Yalnızca `account` bağımlılığı; ek ayar gerekmez
- Netleştirme fişinin muhasebeleştiği günlük sihirbazda seçilir; şirket politikasına uygun günlük (ör. mahsup/yevmiye) tercih edilmelidir
- **Beta** olgunluk; basit ve tek fonksiyonlu — düşük risk

## Kaynaklar

- [GitHub — account_netting (19.0)](https://github.com/OCA/account-financial-tools/tree/19.0/account_netting)
- [Hata Takibi](https://github.com/OCA/account-financial-tools/issues)
