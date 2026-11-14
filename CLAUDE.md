# menizstudio-web — Meniz Studio sitesi

## Proje Özeti
menizstudio.com için statik site (derleme yok, düz HTML). Amaç: Play Store'un istediği **gizlilik politikası adresi**,
AdMob'un istediği **app-ads.txt** (alan adının KÖKÜNDE olmalı) ve stüdyo / oyun tanıtım sayfası. İlk oyun: Haggle Motors
(galeri projesi, paket com.suleyman.cardealers).

## Dosya Yapısı
```
menizstudio-web/
├── index.html          ← stüdyo + oyun kartı (yayından sonra "Coming soon" → Play bağlantısı; satırda yorum var)
├── privacy/index.html  ← gizlilik politikası (KAYNAK: PROJELER/galeri/tasarim/play-store/gizlilik-politikasi.html — orada değiştir, buraya kopyala)
├── app-ads.txt         ← HENÜZ YOK: AdMob → Uygulamalar → app-ads.txt satırı gelince eklenir (pub-XXXX kimliğiyle tek satır)
├── favicon.png, img/   ← MENIZ monogramı (WCLAUDE kökündeki logo), oyun logosu (galeri art/ui/logo.png'den webp)
└── CLAUDE.md
```

## Kritik Kurallar
1. Gizlilik politikasının tek kaynağı galeri projesindeki dosya; değişince `cp ../galeri/tasarim/play-store/gizlilik-politikasi.html privacy/index.html`.
2. app-ads.txt alan adının kökünde (`https://menizstudio.com/app-ads.txt`) düz metin olarak açılmalı. Google Sites bunu yapamaz — kullanma.
3. Barındırma: GitHub Pages ya da Cloudflare Pages (ücretsiz, özel alan adı + HTTPS). Play Console → Mağaza ayarları → Web sitesi = `https://menizstudio.com`
   (AdMob app-ads.txt'yi bu alandan doğrular).
4. Harici kaynak yalnız Google Fonts (Baloo 2, oyunla aynı yazı tipi).

## Canlı kurulum (2026-11-14)
- Depo: **github.com/Smenizz/menizstudio-web** (herkese açık), GitHub Pages main / kök, CNAME dosyası = menizstudio.com, .nojekyll.
  Güncelleme: dosyayı değiştir → `git add -A && git commit -m … && git push` → Pages 1–2 dk'da yayınlar. gh oturumu: Smenizz.
- Alan adı Squarespace Domains'te (NS nsc1–4.squarespacedns.com). E-posta Google Workspace (MX smtp.google.com + SPF TXT) — DNS'te
  "Google Workspace" grubuna DOKUNMA. Web: "Squarespace Defaults" grubu silinir, Custom Records: @ A 185.199.108–111.153 (4 kayıt),
  www CNAME smenizz.github.io. Sonra `gh api -X PUT repos/Smenizz/menizstudio-web/pages -F https_enforced=true`.

## Yayın adımları (genel)
- **GitHub Pages:** yeni herkese açık depo → bu klasörün içeriğini yükle → Settings → Pages → Branch main / root →
  Custom domain `menizstudio.com` → alan adı firmasının DNS panelinde: `A` kayıtları 185.199.108.153, 185.199.109.153,
  185.199.110.153, 185.199.111.153 ve `www` için `CNAME <kullanici>.github.io` → "Enforce HTTPS".
- **Cloudflare Pages (alternatif):** Workers & Pages → Create → Pages → bu klasörü sürükle-bırak yükle → Custom domains → menizstudio.com.
- Kontrol: `https://menizstudio.com/privacy/` ve (eklenince) `https://menizstudio.com/app-ads.txt` tarayıcıda açılır.
