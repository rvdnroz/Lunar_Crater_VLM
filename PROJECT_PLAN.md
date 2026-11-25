# Ay Kraterlerinin Görüntü-Dil Modelleriyle Analizi

**TÜBİTAK 2209-A — Teknik proje planı ve görev takip listesi**

**Araştırma başlığı:** Ay Yüzeyindeki Kraterlerin Görüntü-Dil Modelleriyle Analizi: Geometrik Kanıtla Desteklenen Görsel Muhakeme

**Durum:** Başlangıç / planlama  
**Ekip:** 3 bilgisayar mühendisliği öğrencisi ve akademik danışman  
**Resmî süre:** 12 ay  
**İç hedef:** 24 haftalık aktif teknik geliştirme ve deney süreci (pilot sonuçlarına göre güncellenecek).

Bu dosya araştırma önerisinin teknik uygulama yol haritasıdır. Makale ve bildiri yazımı kapsam dışıdır. Kutular görev tamamlandığında `[x]` olarak işaretlenir. Bir görev ancak çıktısı kaydedildiğinde ve gerekli testleri geçtiğinde tamamlanmış sayılır.

## 1. Amaç ve kapsam

LLaVA-LE modelinin Ay kraterlerini sayma, konumlandırma, boyutlandırma ve morfolojik olarak açıklama performansının klasik görüntü işleme ile üretilen geometrik kanıt sayesinde iyileşip iyileşmediğini ölçmek. Yeni bir model mimarisi veya sıfırdan eğitim birincil kapsamda değildir.

- H1: Geometrik kanıt sayım MAE değerini düşürür ve konum F1 değerini artırır.
- H2: Geometrik kanıt etiketlerle eşleşmeyen krater bildirme oranını düşürür.
- H3: Geometri kalitesi düştükçe performans kazancı azalır (oracle, Hough, bozulmuş geometri).

Olumsuz sonuçlar da geçerli araştırma çıktısıdır. B0'a göre iyileşme tek başına görsel muhakeme kanıtı değildir; K1 ve K2 karşılaştırmaları ayrıca değerlendirilmelidir.

## 2. Ekip ve sorumluluklar

| Rol | Sorumluluk | Teknolojiler | İsim |
|---|---|---|---|
| Öğrenci 1 | Veri seti, referans etiketler, metrikler ve istatistik | Python, LabelMe/CVAT, NumPy, SciPy, Pandas | TBD |
| Öğrenci 2 | Görüntü işleme, krater adayları ve geometrik kanıt | OpenCV, NumPy, Pillow | TBD |
| Öğrenci 3 | VLM, deney otomasyonu, entegrasyon ve arayüz | PyTorch, Transformers, bitsandbytes, CUDA, Gradio | TBD |
| Danışman | Protokol, bilimsel yöntem ve kilometre taşı kontrolü | — | TBD |

**Etiketleme kuralı:** Araştırma önerisinde proje yürütücüsü tek etiketleyicidir. Yürütücü Öğrenci 1 değilse ilk ve kör tekrar etiketlemeleri yürütücüye aktarılır; Öğrenci 1 veri yönetimi ve değerlendirmeden sorumlu kalır. Etiketleme üç kişiye bölünmez. Rol isimleri ve kesin iş yükü ekip toplantısında belirlenir.

## 3. Öğrenci 1 — Veri ve bilimsel değerlendirme

### 3.1 Veri seti hazırlama
- [ ] D01 — LUCID veri setini temin et, erişim ve lisans koşullarını kaydet.
- [ ] D02 — Görüntü, açıklama, soru-cevap ve metadata yapısını incele; geometrik etiket bulunup bulunmadığını doğrula.
- [ ] D03 — Yayımlanan eğitim/değerlendirme ayrımlarını ve görüntü kimliklerini karşılaştır; veri sızıntısı durumunu kaydet.
- [ ] D04 — Krater yoğunluğu ve aydınlatma koşullarına göre katmanlı örnekleme planı oluştur.
- [ ] D05 — 10 pilot görüntü ve en az 150 nihai değerlendirme görüntüsü seç; ayarlama görüntülerini nihai testten ayır.
- [ ] D06 — Görüntü kimlikleri, boyutları, kaynakları, katmanları ve eğitimde görülme durumunu manifest dosyasına kaydet.

### 3.2 Etiketleme (proje yürütücüsü)
- [ ] D07 — CVAT veya LabelMe kur.
- [ ] D08 — Krater sınırı, küçük/silik/örtüşen ve görüntü kenarındaki kraterler için etiketleme protokolü oluştur.
- [ ] D09 — 10 görüntülük pilot etiketlemeyi gerçekleştir; gerçek süre ve krater yoğunluğunu ölç.
- [ ] D10 — 150 görüntünün krater merkezlerini ve yarıçaplarını etiketle.
- [ ] D11 — Rastgele seçilmiş en az 30 görüntüyü farklı oturumda, ilk etiketlere bakmadan yeniden etiketle.
- [ ] D12 — IoU ≥ 0,5 ile tekrarlar arası eşleşmeyi ölç; hedef en az %80; uyuşmazlıkları protokole göre çöz ve kaydet.
- [ ] D13 — Coğrafi koordinatı bulunan uygun görüntüleri Robbins kataloğuyla çapraz kontrol et; manuel etiketleri esas al.
- [ ] D14 — Referans etiketleri ve oracle geometrisini ortak JSON biçimine aktar; doğrula ve sürümle.

### 3.3 Değerlendirme modülü
- [ ] D15 — Analitik daire IoU fonksiyonunu geliştir ve birim testlerini yaz.
- [ ] D16 — Macar algoritması ile bire bir eşleştirme ve IoU ≥ 0,5 eşiğini uygula.
- [ ] D17 — Yumuşak eşleşmeyi uygula: merkez uzaklığı ≤ 0,25 × referans yarıçapı; yarıçap bağıl farkı ≤ %25.
- [ ] D18 — Sayım MAE, precision, recall ve F1 fonksiyonlarını yaz.
- [ ] D19 — Eşleşen kraterlerde yarıçap bağıl hatasını hesapla.
- [ ] D20 — Halüsinasyon oranını, görüntü düzeyinde ikili göstergesini ve toplam oranı hesapla; tahminsiz görüntülerde oranı tanımsız kaydet.
- [ ] D21 — B2, B3 ve K2 için kopyalama oranını hesapla; merkez eşleşmesi tanımını pilotta kesinleştir.
- [ ] D22 — Ayrıştırılamayan yanıtların dahil edildiği birincil ve dışlandığı ikincil analizleri uygula.
- [ ] D23 — K1 dedektör sonuçlarını ve model sonuçlarını aynı değerlendirme arayüzünden geçir.

### 3.4 İstatistik ve hata analizi
- [ ] D24 — Görüntü düzeyinde eşleştirilmiş Wilcoxon işaretli sıra testini uygula.
- [ ] D25 — İkili halüsinasyon göstergesi için McNemar testini uygula.
- [ ] D26 — Dokuz doğrulayıcı test için Holm düzeltmesini uygula (α = 0,05).
- [ ] D27 — Görüntü düzeyinde 1.000 bootstrap örneklemesiyle %95 güven aralıkları ve uygun etki büyüklükleri hesapla.
- [ ] D28 — Küçük ve örtüşen krater alt grupları, hata tipleri, K1/K2 ve B3/B3-T keşfedici karşılaştırmalarını yap.
- [ ] D29 — Öğrenci 3 ile morfoloji hakem rubriğini oluştur ve sonuçları değerlendir.
- [ ] D30 — Sonuç tablolarını, grafikleri, analiz betiklerini ve kalite kontrol kayıtlarını teslim et.

**Teslimatlar:** `dataset_manifest.csv`, `annotations.json`, `oracle_geometry.json`, `circle_iou.py`, `matching.py`, `metrics.py`, `statistical_analysis.py`, etiketleme protokolü ve sonuç tabloları.

## 4. Öğrenci 2 — Görüntü işleme ve geometri

### 4.1 Ön işleme
- [ ] V01 — OpenCV ortamını kur; ortak görüntü okuyucusunu kullan.
- [ ] V02 — Gri tonlama, kontrast iyileştirme ve gürültü azaltma işlemlerini uygula.
- [ ] V03 — Canny kenar algılamayı uygula.
- [ ] V04 — Morfolojik işlemleri uygula ve işlem sırasını pilotta belirle.
- [ ] V05 — Ara görüntüleri ve parametreleri kaydet.

### 4.2 Krater aday üreticisi
- [ ] V06 — Hough Circle Transform ile aday çemberleri üret.
- [ ] V07 — En az üç parametre kümesini ayrı geliştirme görüntülerinde karşılaştır; nihai testten önce parametreleri sabitle.
- [ ] V08 — Tekrarlanan/örtüşen hatalı adayları boyut ve kenar belirginliği ölçütleriyle ele; gerçek örtüşen kraterleri korumaya dikkat et.
- [ ] V09 — Merkez, yarıçap, kenar güveni ve en yakın komşu uzaklığını hesapla.
- [ ] V10 — Küçük, silik ve örtüşen kraterlerde hata örneklerini incele.
- [ ] V11 — Öğrenci 1'in değerlendirme modülüyle aday F1 ve konum hatasını ölç.

### 4.3 Geometrik kanıt
- [ ] V12 — Her adaya benzersiz kimlik ver; ortak koordinat sistemine dönüştür.
- [ ] V13 — İşaretli görüntü (çember, merkez, aday kimliği) oluştur.
- [ ] V14 — Yapılandırılmış geometri JSON ve metin gösterimini üret.
- [ ] V15 — Oracle geometrisinden kontrollü yapay konum ve boyut hatalarıyla bozulmuş geometri üret.
- [ ] V16 — Bozulma dağılımı, büyüklüğü, sınır davranışı ve rastgelelik tohumunu kaydet; pilot sonrası sabitle.

### 4.4 Çok ölçekli işleme ve kontrol
- [ ] V17 — Aday çevresinde farklı ölçeklerde kırpımlar oluştur.
- [ ] V18 — Ham görüntü ve işaretli kırpımları sabit düzenli tek görüntü mozaiğinde birleştir.
- [ ] V19 — Kırpım/mozaik koordinatlarını orijinal görüntüyle eşleştir; dönüşümleri kaydet.
- [ ] V20 — B1, B2, B3 ve B3-T girdi üretim fonksiyonlarını hazırla.
- [ ] V21 — K1 için VLM kullanmadan sayım, konum ve en büyük üç kraterin boyut çıktısını üret.
- [ ] V22 — Tüm geometri kaynaklarında şema, koordinat, boş aday ve sınır durumlarını test et.

**Teslimatlar:** `preprocessing.py`, `crater_detector.py`, `geometry_generator.py`, `geometry_corruption.py`, `visual_marker.py`, `multiscale_crop.py`, K1 çıktıları ve parametre karşılaştırma kayıtları.

## 5. Öğrenci 3 — VLM ve entegrasyon

### 5.1 Model fizibilitesi
- [ ] M01 — LLaVA-LE yayımlanmış kodunu, ağırlıklarını, lisansını ve uyumlu bağımlılıkları doğrula.
- [ ] M02 — Python, PyTorch, CUDA ve Transformers ortamını kur; sürümleri kaydet.
- [ ] M03 — RTX 4070 8 GB VRAM / 48 GB RAM üzerinde 4 bit nicemleme ve CPU offload dene.
- [ ] M04 — Pilot görüntülerde tepe VRAM/RAM, yanıt süresi ve çıktı tutarlılığını ölç.
- [ ] M05 — Gerekirse 15–16 GB T4 sınıfı GPU ortamında 4 bit çalıştırmayı dene; 8 bit yalnızca ölçüm uygunsa denensin.
- [ ] M06 — Model ve donanım konfigürasyonunu deneyler öncesinde sabitle.

### 5.2 Prompt ve ayrıştırma
- [ ] M07 — Sayım, konum, boyut ve morfolojik açıklama için dört sabit soru şablonu oluştur.
- [ ] M08 — İlk üç soru için JSON çıktı şemasını belirle; konum/IoU uyumsuzluğunu danışmanla çöz.
- [ ] M09 — Sıcaklık 0 / deterministik üretim ayarını uygula ve diğer üretim parametrelerini kaydet.
- [ ] M10 — JSON ayrıştırıcı ve şema doğrulaması geliştir.
- [ ] M11 — Regex tabanlı yedek ayrıştırıcı geliştir.
- [ ] M12 — Ayrıştırılamayan yanıtı aynı komutla bir kez yeniden sor; iki ham yanıtı ve durum kodunu kaydet.
- [ ] M13 — Morfolojik açıklama yanıtlarını serbest metin olarak sakla.

### 5.3 Deney otomasyonu
- [ ] M14 — B0 ham görüntü başlangıç koşulunu çalıştır; nihai kümede 600 soru üret.
- [ ] M15 — B1 yalnızca işaretlenmiş görüntü koşulunu entegre et.
- [ ] M16 — B2 ham görüntü ve geometri metni koşulunu entegre et.
- [ ] M17 — B3 ham görüntü, işaretli kırpım mozaiği ve geometri metni koşulunu entegre et.
- [ ] M18 — B3-T işaretli tam görüntü ve Hough metni koşulunu entegre et.
- [ ] M19 — K2 yalnız metin koşulunu test et; görüntü zorunluysa sabit nötr görüntü kullan ve sınırlılığı kaydet.
- [ ] M20 — K1 çıktılarını model çalıştırmadan deney sistemine dahil et.
- [ ] M21 — 14 VLM çalıştırmasını konfigürasyon dosyasından otomatik yönet.
- [ ] M22 — Checkpoint/resume, benzersiz örnek anahtarı, hata kaydı ve tekrar çalıştırma desteği ekle.
- [ ] M23 — Ham yanıt, ayrıştırılmış yanıt, prompt, model sürümü, geometri kaynağı, süre ve kaynak kullanımını kaydet.
- [ ] M24 — Nihai 8.400 soru örneğini tamamla; eksik ve yinelenen kayıtları kontrol et.
- [ ] M25 — Öğrenci 1 ile hakem değerlendirmesini gerçekleştir; model ve prompt sürümlerini kaydet.

### 5.4 Prototip
- [ ] M26 — Gradio arayüzünü geliştir.
- [ ] M27 — Kullanıcının Ay görüntüsü yüklemesini sağla.
- [ ] M28 — İşaretli Hough adaylarını ve B0–B3 yanıtlarını karşılaştırmalı göster.
- [ ] M29 — Hata durumları, boş aday, geçersiz dosya ve model bellek hatalarını yönet.
- [ ] M30 — Uçtan uca sistem ve temiz ortam kurulum testini gerçekleştir.

**Teslimatlar:** `model_loader.py`, `inference.py`, `prompts.py`, `response_parser.py`, `experiment_runner.py`, `checkpoint.py`, `app.py`, ham deney kayıtları ve fizibilite raporu.

## 6. Deney matrisi

| Koşul | Girdi | Geometri | VLM yanıt hedefi |
|---|---|---|---:|
| B0 | Ham görüntü | Yok | 600 |
| B1 | İşaretli görüntü | Oracle, Hough, bozulmuş | 1.800 |
| B2 | Ham görüntü + geometri metni | Oracle, Hough, bozulmuş | 1.800 |
| B3 | Ham görüntü + işaretli kırpımlar + metin | Oracle, Hough, bozulmuş | 1.800 |
| B3-T | İşaretli tam görüntü + metin, kırpımsız | Hough | 600 |
| K1 | Yalnızca dedektör çıktısı, VLM yok | Hough | 0 |
| K2 | Yalnızca geometri metni | Oracle, Hough, bozulmuş | 1.800 |
| **Toplam** | **14 VLM çalıştırması** | | **8.400** |

Her çalıştırma: 150 görüntü × 4 soru = 600 soru. K1 için morfolojik serbest metin üretimi uygulanmaz; geometrik sonuçlar doğrudan hesaplanır. Yeniden sorgulamalar 8.400 hedefinin dışında ek model çağrısı oluşturabilir.

### Deney tamamlanma listesi

| Çalıştırma | Oracle | Hough | Bozulmuş | Geometrisiz |
|---|---|---|---|---|
| B0 | — | — | — | [ ] |
| B1 | [ ] | [ ] | [ ] | — |
| B2 | [ ] | [ ] | [ ] | — |
| B3 | [ ] | [ ] | [ ] | — |
| B3-T | — | [ ] | — | — |
| K2 | [ ] | [ ] | [ ] | — |

- [ ] K1 doğrudan dedektör değerlendirmesi tamamlandı.

### Doğrulayıcı testler

| Hipotez | Karşılaştırma | Metrik | Test |
|---|---|---|---|
| H1 | B2-Hough / B0; B3-Hough / B0 | Sayım mutlak hatası, görüntü F1 | Wilcoxon (4 test) |
| H2 | B2-Hough / B0; B3-Hough / B0 | İkili halüsinasyon | McNemar (2 test) |
| H3 | B3 oracle/Hough/bozulmuş üç ikili karşılaştırma | Sayım mutlak hatası | Wilcoxon (3 test) |

K1, K2, B3-T ve krater alt grupları keşfedicidir. Morfoloji açıklamaları 0–2 puanlık üç boyutlu rubrikle (jeolojik/görsel doğruluk, kanıtla tutarlılık, kanıtsız iddiadan kaçınma) değerlendirilir. Hakem skorları geometrik metriklerin yerine geçmez.

## 7. Ortak veri sözleşmesi (önerilen v1)

```json
{
  "image_id": "lunar_0001",
  "width": 1024,
  "height": 1024,
  "geometry_source": "hough",
  "crater_count": 2,
  "craters": [
    {"id": 1, "x": 0.25, "y": 0.42, "r": 0.08, "edge_confidence": null, "nearest_neighbor_distance_px": null},
    {"id": 2, "x": 0.72, "y": 0.61, "r": 0.04, "edge_confidence": null, "nearest_neighbor_distance_px": null}
  ]
}
```

- Orijin sol üst; x sağa, y aşağı artar. `x = x_px / width`, `y = y_px / height`, `r = r_px / width`.
- IoU ve uzaklık hesapları öncesinde koordinatları piksel uzayına geri dönüştür. Kare olmayan görüntülerde normalize x ve y üzerinde doğrudan Öklid uzaklığı veya daire IoU hesaplama.
- `geometry_source`: `oracle`, `hough`, `corrupted`. Bilinmeyen ek alanlar `null`; değer uydurulmaz.
- `crater_count` aday listesinin uzunluğuna eşit olmalıdır. Boş liste geçerli bir çıktıdır.
- Örnek değerler temsilidir, gerçek ölçüm değildir.

### Model yanıt şemaları

```json
{"sayi": 2}
```
```json
{"craters": [{"x": 0.25, "y": 0.42}]}
```
```json
{"craters": [{"x": 0.25, "y": 0.42, "r": 0.08}]}
```

İkinci örnek önerideki konum sorusudur; üçüncü örnek boyut sorusudur (en büyük üç krater, daha az varsa mevcut kraterler). Konum sorusuna `r` eklenmesi ya da farklı eşleştirme ölçütü kullanılması danışman kararı gerektirir. Boyut yanıtındaki örnek tek kraterlidir ve şemayı gösterir.

Deney kayıtlarının benzersiz anahtarı: `run_id + image_id + question_type`. JSONL kayıtlarında `condition`, `geometry_source`, `model_version`, `prompt_version`, `raw_response`, `parsed_response`, `parse_status`, `attempt`, `latency_ms`, `config_hash` ve hata bilgileri tutulur. Ham çıktılar üzerine yazılmaz.

## 8. Önerilen proje dizini

```text
lunar-crater-vlm/
├── PROJECT_PLAN.md
├── README.md
├── requirements.txt
├── .gitignore
├── configs/
│   ├── model.yaml
│   ├── detector.yaml
│   └── experiments.yaml
├── data/
│   ├── raw/
│   ├── pilot/
│   ├── images/
│   ├── annotations/
│   └── metadata/
├── src/
│   ├── dataset/
│   │   ├── loader.py
│   │   ├── sampling.py
│   │   └── annotation_utils.py
│   ├── vision/
│   │   ├── preprocessing.py
│   │   ├── crater_detector.py
│   │   ├── geometry_generator.py
│   │   ├── geometry_corruption.py
│   │   ├── visual_marker.py
│   │   └── multiscale_crop.py
│   ├── vlm/
│   │   ├── model_loader.py
│   │   ├── inference.py
│   │   ├── prompts.py
│   │   └── response_parser.py
│   └── evaluation/
│       ├── circle_iou.py
│       ├── matching.py
│       ├── metrics.py
│       └── statistical_analysis.py
├── experiments/
│   ├── experiment_runner.py
│   ├── conditions.py
│   └── checkpoint.py
├── results/
│   ├── predictions/
│   ├── geometry/
│   ├── metrics/
│   ├── figures/
│   └── logs/
├── tests/
├── docs/
│   ├── ANNOTATION_GUIDE.md
│   ├── EXPERIMENT_PROTOCOL.md
│   └── PROGRESS.md
└── app.py
```

Bu yapı başlangıç önerisidir; henüz bu dosyaların oluşturulduğu veya kodların çalıştığı anlamına gelmez.

## 9. Git ve ortak çalışma

| Branch | Sorumlu |
|---|---|
| `main` | Ortak, test edilmiş sürüm |
| `develop` | Ortak entegrasyon |
| `feature/dataset`, `feature/evaluation` | Öğrenci 1 |
| `feature/vision` | Öğrenci 2 |
| `feature/vlm`, `feature/interface` | Öğrenci 3 |

- [ ] C01 — Ortak GitHub deposunu oluştur, erişimleri tanımla.
- [ ] C02 — Klasörleri, `.gitignore`, bağımlılık dosyasını ve README'yi hazırla.
- [ ] C03 — Veri/çıktı şemasını ve modül arayüzlerini birlikte onayla.
- [ ] C04 — Küçük örnek veriyle entegrasyon testleri oluştur; PR ile birleştir ve başka bir öğrenciye incelet.
- [ ] C05 — Büyük görüntüleri, ağırlıkları, sanal ortamı, API anahtarlarını ve geçici sonuçları Git'e ekleme; ayrı erişim/yedekleme yöntemi belirle.
- [ ] C06 — Haftalık ekip toplantısı ve iki haftada bir danışman kontrolü planla.
- [ ] C07 — Her deneyde kod commit'i, konfigürasyon, veri manifest sürümü ve ortam bilgisini kaydet.
- [ ] C08 — Lisansların izin verdiği etiket ve kodları paylaş; kurulum ve yeniden üretim adımlarını doğrula.

## 10. Çalışma takvimi

### Resmî iş paketleri (araştırma önerisi)

| İP | İş paketi | Ay | Ağırlık |
|---|---|---|---:|
| 1 | Değerlendirme alt kümesi | 1–3 | %20 |
| 2 | Başlangıç ölçümü | 2–4 | %10 |
| 3 | Krater aday üreticisi | 3–6 | %20 |
| 4 | Geometri entegrasyonu | 5–8 | %20 |
| 5 | Deneyler, ablation, istatistik | 7–10 | %20 |
| 6 | Prototip ve yayın | 9–12 | %10 |

İP6'nın yayın yazımı bu teknik dosyanın kapsamı dışındadır, resmî taahhütten çıkarılmış değildir.

### Hızlandırılmış iç plan (24 hafta)

| Görev | Hafta | Sorumlu |
|---|---|---|
| Ortam kurulumu ve pilot | 1–2 | Ortak |
| Veri seti hazırlama | 1–3 | Ö1 |
| Etiketleme | 2–7 | Yürütücü |
| Metrik modülü | 3–8 | Ö1 |
| Görüntü işleme | 1–5 | Ö2 |
| Hough parametre çalışması | 5–9 | Ö2 |
| Geometri ve kırpımlar | 8–12 | Ö2 |
| Model kurulumu | 1–3 | Ö3 |
| Prompt ve B0 | 3–8 | Ö3 |
| Sistem entegrasyonu | 9–13 | Ortak |
| Nihai deneyler | 13–19 | Ö3, ortak destek |
| İstatistiksel analiz | 17–22 | Ö1 |
| Gradio prototipi | 19–23 | Ö3 |
| Nihai sistem testleri | 22–24 | Ortak |

24 hafta kesin teslim süresi değildir. Nihai değerlendirme ancak protokol ve test kümesi sabitlendikten sonra başlar. Pilot ortalama yanıt süresi `t` saniye ise seri çıkarım yaklaşık `8400*t/3600` saattir; tekrarlar ve kesintiler dahil değildir.

## 11. İlk iki hafta — başlangıç sprinti

### Hafta 1
- [ ] Ö1: LUCID erişimini doğrula, 10 pilot görüntü seç, etiketleme aracını kur.
- [ ] Ö2: OpenCV ortamı kur, bir Ay görüntüsünde Canny ve Hough çalıştır.
- [ ] Ö3: LLaVA-LE ağırlıklarını doğrula, 4 bit model yükle, ilk görüntü yanıtını al ve kaynak kullanımını ölç.
- [ ] Ortak: GitHub, dizin, veri şeması ve danışmanla çözülmesi gereken yöntem kararlarını belirle.

### Hafta 2
- [ ] Yürütücü/Ö1: 10 pilot görüntüyü etiketle, JSON referansları ve sayım metriğini hazırla.
- [ ] Ö2: Aynı 10 görüntü için aday, işaretli görüntü ve geometri JSON üret.
- [ ] Ö3: Aynı görüntüler için B0, B1 ve B2 pilot çıkarımlarını çalıştır; yanıtları kaydet.
- [ ] Ortak: İlk sonuçları karşılaştır, darboğazları ve iş yükünü ölç, danışmana pilot çıktıları göster.

Pilot başarı ölçütü performans artışı değil; modelin çalışması, ortak veri formatının doğrulanması ve en az bir uçtan uca deneyin yeniden üretilebilmesidir.

## 12. Danışmanla karara bağlanacak konular

- [ ] P01 — Konum sorusu yalnızca x/y döndürüyor, daire IoU için r gerekiyor. Konum şemasına r eklemek veya ayrı merkez metriği belirlemek seçeneklerini değerlendir; tüm koşullara aynı karar uygulansın.
- [ ] P02 — B1 yalnızca görsel işaret içermeli; önerinin 2.6 bölümündeki metinsel geometri ifadesiyle çelişkiyi gider.
- [ ] P03 — Pilot/geliştirme ve nihai test görüntülerini ayır; Hough, prompt ve diğer ayarları test sonuçlarına bakarak değiştirme.
- [ ] P04 — Bozulmuş geometri parametrelerini ve kopyalama eşleşme ölçütünü kesinleştir.
- [ ] P05 — B3 mozaiği, K2 görüntüsüz/nötr görüntü uygulaması ve K1'in morfoloji için uygulanamazlığını kayda geçir.
- [ ] P06 — Boş referans/tahmin durumlarında F1 ve ayrıştırma hatası kurallarını, eşleştirme algoritmasının eşik uygulamasını netleştir.
- [ ] P07 — Hakem rubriği, alt grup tanımları ve eğitim verisi sızıntısı belirsizliklerini protokole ekle.
- [ ] P08 — Nihai deneylerden önce protokolü ve konfigürasyonları sürümleyerek dondur.

## 13. Riskler ve B planları

| Risk | Müdahale |
|---|---|
| 13B model 8 GB VRAM'e sığmıyor | 4 bit ve CPU offload; ardından T4 sınıfı GPU; gerekiyorsa kuralları ve bütçeyi doğrulayarak bulut GPU. Son çare 7B LLaVA-1.5 ayrı model çalışmasıdır. |
| LLaVA-LE ağırlıkları erişilemiyor | Uyumlu kod/sürüm kontrolü; temel LLaVA kullanılırsa LLaVA-LE'ye özgü çıkarımları geri çek. |
| Hough küçük/silik kraterleri kaçırıyor | Önce klasik hattı iyileştir; OWLv2/YOLO gibi alternatifleri yalnızca ayrı deney olarak değerlendir. |
| Etiketleme çok uzun sürüyor | Pilot ölçümlerine göre iş yükünü düzenle. 100 görüntüye zorunlu düşüş olursa hedef sapmasını, güç analizini ve keşfedici niteliği raporla. |
| Model çoklu görüntüyü işleyemiyor | Sabit tek mozaik; sıralı sorguyu yalnızca pilotta alternatif olarak incele. |
| Geometri metni kullanılamıyor | Önce prompt ve ayrıştırma kontrolü; LoRA yalnızca danışman onayıyla, testten ayrı eğitim verisi ve ayrı B3 varyantı olarak. |
| JSON ayrıştırma başarısız | Örnek şema, gerekirse kısıtlı çözümleme; yedek ayrıştırıcı, tek yeniden sorgulama ve hata oranı kaydı. |
| Hakem modeline erişim/maliyet | Aynı rubrikle açık kaynaklı model; birincil geometrik değerlendirmeyi sürdür. |
| Deney kesiliyor | Checkpoint, benzersiz anahtar, periyodik yedek ve eksik örnek denetimi. |

## 14. Kilometre taşları ve tamamlanma kriterleri

- [ ] G1 — Veri ve model erişimi doğrulandı, ilk pilot çıkarım başarılı.
- [ ] G2 — 150 referans görüntüsü ve en az 30 kör tekrar etiketlemesi hazır; %80 hedefi ölçüldü.
- [ ] G3 — Dedektör en az üç parametre kümesiyle karşılaştırıldı; F1 ve konum hatası kaydedildi.
- [ ] G4 — B0 için 600 soru ve tüm uygun metrikler hazır.
- [ ] G5 — Tüm koşullar entegre, yöntem kararları ve konfigürasyonlar sabit.
- [ ] G6 — 14 çalıştırma, 8.400 benzersiz soru örneği ve K1 çıktıları tamamlandı; ayrıştırma hataları raporlandı.
- [ ] G7 — Dokuz doğrulayıcı test, Holm düzeltmesi, %95 güven aralıkları ve keşfedici analizler hazır.
- [ ] G8 — Gradio prototipi kullanıcı görüntüsünde çalışıyor ve B0–B3 karşılaştırması gösteriyor.
- [ ] G9 — Temiz ortamda yeniden üretim testi, kod incelemesi, lisans kontrolü ve nihai teknik arşiv tamamlandı.

## 15. İlerleme kaydı

Her hafta `docs/PROGRESS.md` dosyasını güncelleyin. Örnek:

```markdown
## Hafta 01 — YYYY-AA-GG
- Tamamlanan görevler: D01, V01, M01
- Test ve kanıt: [commit / sonuç dosyası / log]
- Karşılaşılan sorunlar:
- Alınan kararlar:
- Sonraki hafta hedefleri:
- Danışman geri bildirimi:
```

Görevlerin zorlukları farklı olduğundan yalnızca tamamlanan kutu sayısı gerçek proje yüzdesi değildir. Kilometre taşları ve resmî iş paketi ağırlıkları ayrıca izlenmelidir.

## 16. Kaynak ve kapsam notu

Temel belge: `TUBITAK_2209A_Arastirma_Onerisi_Nihai(1).docx`, özellikle §2.1–2.10, §3.1–3.3 ve §4. LLaVA-LE için öneride verilen kaynak: https://arxiv.org/abs/2603.24696 ; Robbins kataloğu: https://doi.org/10.1029/2018JE005592 . Dış kaynakların erişilebilirliği bu plan hazırlanırken ayrıca doğrulanmamıştır; ilk sprintte kontrol edilecektir.

Rol atamaları, dosya isimleri, JSON kayıt alanları, Git düzeni ve 24 haftalık iç takvim öneri niteliğindedir. Araştırma önerisindeki yöntemsel belirsizlikler §12'de açıkça bırakılmıştır. Bu plan tamamlanmış deney veya doğrulanmış performans sonucu içermez.
