# Wiki ve İş Talimatları (`document_page` + `document_knowledge` + `mgmtsystem` + `document_page_work_instruction`)

!!! info "Kart Bilgisi"
    **Kurulum Sırası:** 24/24 · **Proje Kartı:** id 30 · **Test Durumu:** ✅ PASS

| Alan | Değer |
|---|---|
| Teknik Ad | `document_page`, `document_knowledge`, `mgmtsystem`, `document_page_work_instruction` |
| OCA Deposu | [OCA/knowledge](https://github.com/OCA/knowledge) + [OCA/management-system](https://github.com/OCA/management-system) |
| Sürüm | document_page 19.0.1.0.2 · document_knowledge 19.0.1.0.1 · mgmtsystem 19.0.1.2.0 · work_instruction 19.0.1.0.1 |
| Olgunluk | Beta (tümü) |
| Lisans | AGPL-3 |
| Bağımlılıklar | `mail`, `document_knowledge`, `html_editor` (çekirdek); work_instruction: `document_page` + `mgmtsystem` |
| Kategori | Knowledge / Management System |

## Nedir?

Community sürümde **Dokümanlar/KB (Knowledge)** uygulaması bulunmaz. Bu dört modül, wiki tarzı doküman yönetimi ve ISO tipi **iş talimatı** yapısını kurar:

| Modül | Görev |
|---|---|
| **document_knowledge** | Alt yapı: tüm Odoo kayıtlarına bağlı dokümanların merkezî erişimi; **Knowledge** üst menüsü |
| **document_page** | Wiki motoru: kategori + sayfa yapısı, zengin metin, **sürüm geçmişi ve karşılaştırma (diff)** |
| **mgmtsystem** | Yönetim sistemi temeli: ISO uyumluluk menüleri, güvenlik grupları, dokümantasyon yapısı |
| **document_page_work_instruction** | **İş Talimatı (Work Instructions)** kategorisi + "Adım 1/2/3" şablonu |

## Ne İşe Yarar?

- **İç doküman yönetimi:** İş talimatları, prosedürler, politika dokümanları Odoo içinde merkezî tutulur
- **Sürüm takibi:** Her sayfa revizyonu otomatik saklanır; **kim, ne zaman, neyi değiştirdi** karşılaştırmalı görüntülenir — ISO/Kalite denetimleri için kritik
- **Şablonlu kategori:** Kategori bazında HTML şablon tanımlanır; yeni sayfa şablonla açılır (iş talimatında hazır 3 adım iskeleti)
- **Menü otomasyonu:** Sayfa kategorisinden tek tıkla menü oluşturma sihirbazı
- **Erişim yönetimi:** Merkezî doküman erişimi yetkisi (`Central access to Documents`); opsiyonel `document_page_access_group` ile sayfa bazlı yetki
- **Onay akışı (opsiyonel):** `document_page_approval` modülü ile doküman onayı ve sürüm yayını

## Odoo'da Neleri Değiştirir?

- **Knowledge** üst menüsü: *Categories* (kategoriler) ve *Pages* (sayfalar)
- **Yönetim Sistemleri (Management Systems)** üst menüsü: *Documentation → Work Instructions*
- Sayfa ekranında: zengin metin düzenleyici (html_editor), **History** sekmesi (sürüm geçmişi + diff)
- Kategori kartında **Template** alanı (yeni sayfaya uygulanan içerik şablonu)
- Kategoriden **Create Menu** sihirbazı
- İş Talimatları kategorisinde hazır `<h1>Adım 1/2/3</h1>` şablonu
- Kayıtlara bağlı dokümanlara Knowledge menüsünden merkezî erişim

## Nasıl Çalışır?

1. **Kategori oluşturma:** *Knowledge → Categories*; şablon içeriği tanımlanır
2. **Sayfa oluşturma:** *Knowledge → Pages*; kategori seçilir → şablon otomatik yüklenir
3. **İçerik düzenleme:** Zengin metin; tablo, görsel, bağlantı eklenebilir
4. **Revizyon:** Kaydettikçe sürüm oluşur; History sekmesinden eski sürümler karşılaştırılır (html_diff paketi kuruluysa gelişmiş görünüm)
5. **İş talimatı:** *Management Systems → Documentation → Work Instructions* kategorisindeki şablon düzenlenir; talimat sayfaları bu şablonla üretilir

## Kurulum ve Yapılandırma

- **Kurulum sırası:** `document_knowledge → document_page → mgmtsystem → document_page_work_instruction`
- İki ayrı repo mount edilir: **OCA/knowledge** ve **OCA/management-system**
- `html_editor` çekirdek Odoo 19 modülüdür
- **Opsiyonel genişletmeler:** `document_page_approval` (onay akışı), `document_page_access_group` (sayfa bazlı yetki grupları), `document_page_project` (proje bağlantısı), `document_page_procedure` (prosedür şablonları), `mgmtsystem_action/nonconformity` (aksiyon ve uygunsuzluk yönetimi)
- Gelişmiş diff görünümü için container'a `html_diff` python paketi kurulabilir (kalıcılık notu: recreate'te silinir)

## Gerekli Yetki Grupları

| Grup (teknik ad) | Türkçe Adı | Neden Gerekli |
|---|---|---|
| `document_knowledge.group_document_user` | Document Knowledge user | Knowledge menüsü temel erişimi (`base.group_user`'ı kapsar) |
| `document_knowledge.group_ir_attachment_user` | Central access to Documents | Tüm kayıtlara bağlı dokümanlara merkezî erişim |
| `document_page.group_document_editor` | Editor | Wiki sayfası oluşturma/düzenleme |
| `document_page.group_document_manager` | Manager | Sayfa/kategori yönetimi (Editor'ü kapsar) |
| `mgmtsystem.group_mgmtsystem_viewer` | Viewer | Yönetim sistemi dokümanlarını görüntüleme |
| `mgmtsystem.group_mgmtsystem_user` | User | Dokümanları kullanma |
| `mgmtsystem.group_mgmtsystem_user_manager` | Approving User | Doküman onaylama |
| `mgmtsystem.group_mgmtsystem_manager` | Manager | Yönetim sistemi yönetimi |
| `mgmtsystem.group_mgmtsystem_auditor` | Auditor | Denetim erişimi |
| `base.group_user` | Rol / Kullanıcı | İş talimatı şablonu görünümü |

!!! tip "Grup atama"
    Gruplar **Ayarlar → Kullanıcılar → (kullanıcı) → Yetkiler** bölümünden atanır. Muhasebe grubu hiyerarşisi için [Ana Sayfa → Yetki Grupları Hızlı Referans](../index.md#yetki-gruplari) bölümüne bakın.

## Kaynaklar

- [GitHub — document_page (19.0)](https://github.com/OCA/knowledge/tree/19.0/document_page)
- [GitHub — document_knowledge (19.0)](https://github.com/OCA/knowledge/tree/19.0/document_knowledge)
- [GitHub — mgmtsystem (19.0)](https://github.com/OCA/management-system/tree/19.0/mgmtsystem)
- [GitHub — document_page_work_instruction (19.0)](https://github.com/OCA/management-system/tree/19.0/document_page_work_instruction)
- [Runboat — knowledge](https://runboat.odoo-community.org/builds?repo=OCA/knowledge&target_branch=19.0) | [Runboat — management-system](https://runboat.odoo-community.org/builds?repo=OCA/management-system&target_branch=19.0)
