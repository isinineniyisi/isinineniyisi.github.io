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
| `assets/logo/` | üyelerin kendi logoları (18 üyede var) |
| `.nojekyll` | alt çizgiyle başlayan yolları Jekyll'in yutmaması için |

Tek harici bağımlılık **Google Fonts** (Spectral + Archivo). Font gelmezse sayfa
Georgia / sistem groteskine düşer; düzen bozulmaz. Başka script, CDN veya izleyici yok.

## Tasarım

Sayfa bir uygulama değil, **bir kurumun kaydı** gibi kurulur: kart, gölge, yuvarlak
köşe ve renkli rozet yok; hiyerarşiyi saç teli çizgiler, boşluk ve tipografi taşır.
Dizin bir **üye kütüğü** (numara · işletme · faaliyet dalı · merkez), üye sayfası bir
**dosya** (künye bloğu + ana sütun + yapışkan yan ray). Tek tema: aydınlık.
Firma başına vurgu rengi (`belge.ACCENT`) bilerek kullanılmıyor — 38 ayrı vurgu
rengi kurum kimliğiyle yarışıyordu; tek vurgu altın ve yalnız çizgi olarak görünür.

## Yayınlama

1. GitHub'da **`isinin-en-iyisi`** adıyla hesap/organizasyon aç.
   (Kullanıcı adında alt çizgi kullanılamaz; yalnız harf, rakam ve tire.)
2. `isinin-en-iyisi.github.io` adında **public** bir depo aç.
3. Bu klasörün içeriğini deponun **köküne** kopyala (klasörün kendisini değil,
   içindekileri) ve push'la.
4. Settings → Pages → Source: `Deploy from a branch`, branch `main`, klasör `/ (root)`.
5. Birkaç dakika sonra `https://isinin-en-iyisi.github.io/` yayında olur.

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

Üretilen QR'lar rastgele kontrol edilmedi, **hepsi çözülerek** sınandı:
38/38 PNG doğru adresi veriyor, 38/38 kartvizit QR'ı 300 dpi baskı
görüntüsünden okunuyor (modül ≈ 0,58 mm; telefonla okuma sınırı ~0,40 mm).
