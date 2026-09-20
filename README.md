# 08-site — GitHub Pages sitesi

Bu klasörün **tamamı** yayınlanacak içeriktir. Elle düzenlenmez: `06-uretici/site.py`
her çalıştığında klasörü siler ve `build/firms.json`'dan yeniden kurar.

```bash
cd 06-uretici && ./venv/bin/python site.py
```

## QR ne taşıyor

Her üyenin QR'ı **kısa bir adres** taşır — `…/u/01/` — ve o adres üyenin sayfasına
yönlendirir. Kartın üstüne vCard gömülmedi; gerekçesi şu: vCard'ı QR'a gömseydin
bir telefon değiştiğinde **basılmış bütün kartlar ölürdü**. Adresle basılan kart
kalıcıdır, bilgi sitede güncellenir. Rehbere ekleme yine tek dokunuş: üye
sayfasındaki **"Rehbere ekle"** düğmesi `.vcf` dosyasını indirir.

## Yapı

| Yol | Ne |
|---|---|
| `index.html` | 38 üyenin dizini, arama kutusu dahil (Türkçe küçültme doğru: I→ı, İ→i) |
| `member/<no>-<slug>/index.html` | üye sayfası — QR'ın vardığı yer |
| `member/<no>-<slug>/<slug>.vcf` | rehbere ekleme kartı |
| `u/<no>/index.html` | QR'daki kısa adres; üye sayfasına yönlendirir |
| `qr/<no>-<slug>.png` | baskıya hazır QR (≈1200 px) |
| `qr/index.html` | bütün QR'lar tek sayfada + PNG indirme |
| `member/<no>-<slug>/profil/` | A4 üye iş profili — HTML görünüm + `belge.html` + PDF |
| `member/<no>-<slug>/kartvizit/` | kartvizit — HTML görünüm + `kart.html` + PDF |
| `assets/logo/` | üyelerin kendi logoları (18 üyede var) |
| `assets/logo-platform.png` | platform mührü (gerçek logodan hazırlandı) |
| `.nojekyll` | alt çizgiyle başlayan yolları Jekyll'in yutmaması için |

Tek harici bağımlılık **Google Fonts** (Spectral + Archivo). Font gelmezse sayfa
Georgia / sistem groteskine düşer; düzen bozulmaz. Başka script, CDN veya izleyici yok.

## Belgeler

Her üye sayfasından A4 **üye iş profili** ve **kartvizit** açılır: ekranda HTML
olarak görüntülenir (kap genişliğine göre ölçeklenir), "PDF indir" düğmesiyle
baskı dosyası iner. Kaynak dosyalar `02-kartlar/`, `04-pdf/`, `09-cards/` ve
`09-cards/pdf/` klasörlerinden kopyalanır — `site.py` çalışmadan önce onların
üretilmiş olması gerekir, yoksa eksik olanları isim isim uyarır.

HTML belgeler kendi kendine yeter: firma logoları data-URI olarak gömülü,
ikonlar satır içi SVG. Bu yüzden başka bir klasöre kopyalanınca bozulmuyorlar.

## Platform mührü

`assets/logo-platform.png`, `logo.jpg`'den hazırlandı: daire dışı saydam, içi
beyaz bırakıldı (beyazı topyekûn saydamlaştırmak lacivert yazıyı lacivert
künye çubuğunda görünmez yapardı). Kaynak fotoğrafta mühür üstten ~48 px
kesikti; dış halka aynı renk ve kalınlıkta tamamlandı. Yeniden üretmek
gerekirse bu iki ayrıntıyı atlama.

## Tasarım

Sayfa bir uygulama değil, **bir kurumun kaydı** gibi kurulur: kart, gölge, yuvarlak
köşe ve renkli rozet yok; hiyerarşiyi saç teli çizgiler, boşluk ve tipografi taşır.
Dizin bir **üye kütüğü** (numara · işletme · faaliyet dalı · merkez), üye sayfası bir
**dosya** (künye bloğu + ana sütun + yapışkan yan ray). Tek tema: aydınlık.
Firma başına vurgu rengi (`belge.ACCENT`) bilerek kullanılmıyor — 38 ayrı vurgu
rengi kurum kimliğiyle yarışıyordu; tek vurgu altın ve yalnız çizgi olarak görünür.

## Yayınlama

Depo kuruldu ve site yayında: **https://isinineniyisi.github.io/**
(`github.com/isinineniyisi/isinineniyisi.github.io`, dal `main`, kök klasör, HTTPS zorunlu.)

`08-site/` hem üretim çıktısı hem deponun çalışma kopyası. Güncellemek tek komut:

```bash
cd 06-uretici && ./publish.sh "ne değiştiyse"
```

Bu betik `site.py`'yi çalıştırır, değişen dosyaları commit'ler ve push'lar.
GitHub Pages derlemesi 1-2 dakika sürer.

> **Deponun adı neden `…​.github.io`?** Organizasyon kök sitesi ancak bu adla
> çalışır. `isinineniyisi/isinineniyisi` olsaydı adres
> `isinineniyisi.github.io/isinineniyisi/` olurdu — QR'lar uzar ve alt yol
> yüzünden bağlantılar kırılırdı.

> **`site.py` klasörü temizlerken `.git`, `.gitignore` ve `CNAME` dosyalarına
> dokunmaz** (`site.py:temizle`). Bu koruma kaldırılırsa ilk üretimde depo silinir.

## Adresi değiştirmek

Adres tek yerde duruyor: `06-uretici/site-config.json` → `taban_adres`.
Değiştirip iki betiği yeniden çalıştır; **38 QR ve 38 kartvizit** yeni adrese göre
yeniden basılır:

```bash
cd 06-uretici
./venv/bin/python site.py && ./venv/bin/python card.py
```

Kendi alan adını bağlarsan (ör. `isinineniyisi.com`) depoya `CNAME` dosyası ekle
ve `taban_adres`i o adrese çevir — o zaman `github.io` adı hiç görünmez.

## Doğrulama

Üretilen QR'lar rastgele kontrol edilmedi, **hepsi uçtan uca** sınandı:

- 38/38 PNG çözülüyor ve doğru adresi veriyor
- 38/38 kod, **canlı sitede** üyenin sayfasını açıyor; sayfada firma adı
  ve telefon bulunduğu ayrıca doğrulandı
- 38/38 kartvizit QR'ı **300 dpi baskı görüntüsünden** okunuyor
  (modül ≈ 0,58 mm; telefonla güvenli okuma sınırı ~0,40 mm)
