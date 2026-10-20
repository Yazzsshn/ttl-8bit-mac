# Benzetim (Digital)

Devreler [Digital](https://github.com/hneemann/Digital) programında kurulur. Programın kendisi bu depoda yoktur; yalnızca bu projede üretilen dosyalar buradadır.

| Dosya | Açıklama |
|---|---|
| `mac_birimi.dig` | MAC birimi, yöntem A (ilk sürümde hazır çarpıcı parçasıyla) |
| `mac_tablo.dig` | MAC birimi, yöntem B (ROM tablosuyla çarpma) |
| `tests/mac_test.txt` | Defterin ürettiği 912 satırlık test tablosu (256 çarpım + 1 sahnenin 300 MAC işlemi) |
| `rom/carpim_tablosu.hex` | Yöntem B için 256 baytlık çarpım tablosu |
| `screenshots/` | Devre ve geçen testlerin ekran görüntüleri |

Doğrulama ölçütü: her iki devrede de `tests/mac_test.txt` içindeki 912 satırın tamamı geçmelidir.
