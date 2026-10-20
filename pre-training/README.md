# Ön çalışma: renk sınıflandırma modeli

`renk_on_deneme.ipynb` defteri sentetik renk görsellerinden pikselleri 4 renge ayıran doğrusal bir sınıflandırıcı eğitir, ağırlıkları 4 bite nicemler, breadboard'daki MAC döngüsünü adım adım taklit eder ve Digital benzetimi için test tablosu üretir.

## Klasörler

- `data/`: eğitim ve test görselleri (`foto.zip`). Görseller [veri üretici](../tools/data-generator/) sayfasıyla oluşturulmuş **sentetik** görüntülerdir: renk başına 5 eğitim + 1 test görseli, 20×20 piksel.
- `results/2026-10-20/`: 20 Ekim 2026 çalıştırmasının çıktıları
  - `sonuclar.txt`: doğruluklar, 4 bitlik ağırlıklar ve sapmalar
  - `bit_taramasi.png`: 2–8 bit için doğruluk eğrisi
  - `sahne_sonuclari.png`: 5×5 sahne testleri ve yanan LED

## 4 bitlik ağırlıklar (R, G, B) ve sapmalar

| Renk | w_R | w_G | w_B | b |
|---|---|---|---|---|
| Kırmızı | 6 | −7 | 1 | 1 |
| Yeşil | −5 | 7 | −2 | −4 |
| Mavi | −4 | −2 | 6 | 7 |
| Sarı | 3 | 2 | −4 | −5 |

Defteri yeniden çalıştırırsanız yeni sonuçları yeni tarihli bir klasöre (`results/YYYY-MM-DD/`) koyun; eski sonuçların üzerine yazmayın.
