# ttl-8bit-mac

[English](README.md) | **Türkçe**

Ayrık TTL lojik entegrelerle breadboard üzerinde kurulan 8-bit bir bilgisayar için donanımsal çarp-biriktir (MAC) hızlandırıcısı. MAC birimleri işlemciye komut seti uzantısı olarak (`MAC`, `CLRACC`, `RDACC`) eklenir ve MAC işleminin üç farklı gerçekleştirme yöntemi saat çevrimi, donanım maliyeti (entegre sayısı) ve sınıflandırma doğruluğu açısından karşılaştırılır.

Bu depo bir TÜBİTAK 2209-A araştırma önerisinin ön çalışmalarını ve ileride proje çıktılarını içerir.

## Araştırma sorusu

MAC biriminin **gerçekleştirme yöntemi** (yazılım, kaydır-ve-topla donanımı, EEPROM tablosu), **paralellik derecesi** (1 veya 2 birim) ve **veri bit genişliği** (2–8 bit) değiştikçe, sabit bir iş yükünde **uçtan uca çevrim sayısı**, **donanım maliyeti** ve **sınıflandırma doğruluğu** arasında nasıl bir ödünleşim oluşur?

## Kıyaslama görevi: 5×5 renk sınıflandırma

- 25 pikselin her birinin R, G ve B değeri 4 bittir.
- Her piksel tek katmanlı bir yapay sinir ağı olan doğrusal bir sınıflandırıcıyla 4 sınıftan birine (kırmızı, yeşil, mavi, sarı) atanır: `puan_k = w_k1·R + w_k2·G + w_k3·B + b_k`. En yüksek puan kazanır.
- Ağırlıklar işaretli 4 bitlik tamsayılardır; akümülatör 12 bittir.
- İşlemci renkleri sayar ve en çok bulunan rengin LED'ini yakar.
- Bir görüntü **300 MAC işlemi** gerektirir (25 piksel × 4 sınıf × 3 kanal).

Görevin kendisi amaç değildir; MAC mimarisini görünür ve ölçülebilir kılan sabit bir kıyaslama yüküdür.

## Ön çalışma sonuçları (20.10.2026, sentetik veri)

| Ölçüm | Sonuç |
|---|---|
| Test doğruluğu, kayan noktalı | %91,9 |
| Test doğruluğu, 4 bit | %88,5 |
| 5×5 sahne testi | 11 / 12 doğru LED |
| Akümülatör aralığı | −92 … +91 |

Veri sentetiktir ve test kümesi küçüktür; sonuçlar birkaç puan oynayabilir. Ayrıntılar: [pre-training/](pre-training/)

![Bit genişliğine göre doğruluk](pre-training/results/2026-10-20/bit_taramasi.png)

## Modeli kendi görsellerinle eğit

Sonuçları tekrar üretebilir ya da modeli kendi oluşturduğun verilerle eğitebilirsin. İki yol var:

**A. Veri üreticiyle sentetik görsel oluştur**
1. [tools/data-generator/index.html](tools/data-generator/index.html) dosyasını tarayıcında aç (indirip çift tıkla) ya da çevrim içi sürümünü kullan: https://yazzsshn.github.io/ttl-8bit-mac/tools/data-generator/
2. Her renkten eğitim görseli sayısını, görsel boyutunu, ton çeşitliliğini ve 5×5 test sahnesi sayısını seç.
3. **foto.zip indir** düğmesine bas.

**B. Kendi fotoğraflarını kullan**
Kırmızı, yeşil, mavi ve sarı nesnelerin fotoğraflarını çek ve şöyle adlandır (Türkçe veya İngilizce renk adı olabilir):
- Eğitim görselleri: `kirmizi_1.jpg`, `kirmizi_2.jpg`, `yesil_1.png`, `mavi_1.png`, `sari_1.png` … (her renkten en az 2, önerilen 5 veya daha fazla; her rengin son görseli teste ayrılır)
- Test sahneleri: adı `sahne` veya `test` ile başlar ve en çok bulunan renkle biter, ör. `sahne_01_mavi.png`

Hepsini tek bir `foto.zip` dosyasında topla.

**Sonra eğit:**
1. [pre-training/renk_on_deneme.ipynb](pre-training/renk_on_deneme.ipynb) defterini Google Colab'da aç (Dosya → Not defteri yükle).
2. Hücreleri sırayla çalıştır. Yükleme düğmesi çıkınca `foto.zip` dosyanı seç.
3. Defter modeli eğitir, 4 bite nicemler, donanımdaki MAC döngüsünü adım adım taklit eder, 2–8 bit taraması yapar, 5×5 sahneleri test eder ve sonunda Digital benzetimi için test tablosu olan `mac_test.txt` dosyasını üretir.

## Depo yapısı

```
ttl-8bit-mac/
├── README.md / README.tr.md
├── LICENSE                     Apache License 2.0
├── tools/
│   └── data-generator/         sentetik eğitim görselleri ve test sahneleri üreten tarayıcı sayfası
├── pre-training/
│   ├── renk_on_deneme.ipynb    model, 4 bit nicemleme, MAC döngüsü, bit taraması, test tablosu
│   ├── data/                   yayımlanan sonuçlarda kullanılan foto.zip
│   └── results/2026-10-20/     sonuclar.txt, bit_taramasi.png, sahne_sonuclari.png
├── simulation/                 Digital devre dosyaları (.dig), test tabloları, ROM içerikleri, ekran görüntüleri
├── hardware/                   breadboard şemaları, mikrokod, EEPROM içerikleri, ölçümler (proje süresince)
└── docs/                       deney föyleri ve eğitim materyali (proje süresince)
```

## Benzetim

Devreler [Digital](https://github.com/hneemann/Digital) programında (ücretsiz, Java ile çalışır) kurulur. `simulation/mac_birimi.dig` dosyasını açıp test düğmesine bas; `simulation/tests/mac_test.txt` içindeki 912 satırın tamamı geçmelidir. Ayrıntılar: [simulation/](simulation/)

## Durum

| Bölüm | Durum |
|---|---|
| Python modeli ve ön çalışma sonuçları | Tamamlandı |
| Digital'de MAC birimi | Devam ediyor |
| Benzetimde tam işlemci, breadboard kurulumu, paralel MAC | Planlandı (proje süresi) |

## Lisans

Apache License 2.0. Ayrıntılar için [LICENSE](LICENSE) dosyasına bakın.
