# menizstudio-web — Meniz Studio sitesi

## Proje Özeti
menizstudio.com için statik site (derleme yok, düz HTML). Amaç: Play Store'un istediği **gizlilik politikası adresi**,
AdMob'un istediği **app-ads.txt** (alan adının KÖKÜNDE olmalı) ve stüdyo / oyun tanıtım sayfası. İlk oyun: Haggle Motors
(galeri projesi, paket com.menizstudio.hagglemotors).

## Dosya Yapısı
```
menizstudio-web/
├── index.html          ← stüdyo + oyun kartı (yayından sonra "Coming soon" → Play bağlantısı; satırda yorum var)
├── privacy/index.html  ← gizlilik politikası (KAYNAK: PROJELER/galeri/tasarim/play-store/gizlilik-politikasi.html — orada değiştir, buraya kopyala)
├── app-ads.txt         ← AdMob satırı (2026-11-19, pub-8854224726115216); AdMob → Uygulamalar → app-ads.txt doğrular
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
- **HTTPS YAPILDI (2026-11-15):** sertifika (Let's Encrypt, menizstudio.com + www) onaylı, https zorunlu, www → kök 301.
  **Ders:** alan adı GitHub'a DNS'ten ÖNCE verildiği için sertifika hiç istenmedi; eski Squarespace kayıtlarının TTL'i 4 saat,
  GitHub'ın denetimi o süre eski IP'yi gördü (pages/health → is_non_github_pages_ip_present). Denetim düzelince aynı cname'i
  yeniden yazmak İŞE YARAMAZ: `-f "cname="` ile kaldır, sonra yeniden ekle → 20 sn'de approved. Her kaldır/ekle depoya
  "Delete/Create CNAME" commit'i atar → yerelde `git pull` yap.

## Yayın adımları (genel)
- **GitHub Pages:** yeni herkese açık depo → bu klasörün içeriğini yükle → Settings → Pages → Branch main / root →
  Custom domain `menizstudio.com` → alan adı firmasının DNS panelinde: `A` kayıtları 185.199.108.153, 185.199.109.153,
  185.199.110.153, 185.199.111.153 ve `www` için `CNAME <kullanici>.github.io` → "Enforce HTTPS".
- **Cloudflare Pages (alternatif):** Workers & Pages → Create → Pages → bu klasörü sürükle-bırak yükle → Custom domains → menizstudio.com.
- Kontrol: `https://menizstudio.com/privacy/` ve (eklenince) `https://menizstudio.com/app-ads.txt` tarayıcıda açılır.

## Yeni marka (2026-11-18) — HENÜZ UYGULANMADI
- Yeni stüdyo logosu "Sonsuz Oyun" (renkle M çizen üçgen döngülü ∞, mercan #FF4F3A + açık mercan + lacivert #14162B).
  Tam kit: `tasarim/marka/` (README.md + 16 sayfalık marka-kitabi.pdf; logolar, Android/iOS simge setleri, web favicon+manifest,
  oyun açılış/rozet, logo animasyonu MP4, desen, kartvizit, çıkartma, e-posta imzası). Siteye geçerken `web/head-etiketleri.html`. Eski MENIZ altın monogramın
  yerine geçecek; sitedeki favicon.png / img/ henüz eski logoda. Play geliştirici profili 2026-11-18'de yeni logoya geçti
  (Play 24-bit şeffafsız PNG ister → `tasarim/marka/uygulama/play-24bit/`).
- Logo şeklini değiştirme; renk değişikliği `tasarim/marka/araclar/kit.cjs` ile yeniden üretilir (ChatGPT'ye yeniden çizdirme).

## Tasarım taslağı (tasarim/, 2026-11-15) — YAYINDA DEĞİL
- `tasarim/` git'e girmez (.git/info/exclude). CD çalar temalı yeni ana sayfa: ortada çalar, üst yarıda yarım ay disk halkası
  (seçince halka döner, disk çalara iner), her oyunun 5 dk demosu (`?hizli` = 20 sn).
- Demolar `node tasarim/oyunlari-hazirla.mjs [korkosu] [soygun]` ile üretilir (oyun projelerine dokunmaz; Soygun geçici kopyadan
  Godot "Web" dışa aktarımı, threadsiz → GitHub Pages'te özel başlık gerekmez) → tasarim/oyun/. Önizleme: `Onizle.bat`
  (yerel sunucu; Godot dosyaya çift tıklayınca çalışmaz).
- Haggle demosu (`oyunlari-hazirla.mjs haggle`) KÜÇÜLTÜLMÜŞ kopya: yalnız 36 anime müşteri (HAGGLE_KEEP_PEOPLE), kayıplı görsel kalitesi
  0.6 (önbellek silinip yeniden içe aktarılır), tarayıcıda enerji sınırsız, kullanım verisi kapalı → pck ~75 MB (sınır 100 MB). Asıl galeri
  projesine dokunulmaz; galeri değişince betiği yeniden koş (betik pck 95 MB üstüne çıkarsa uyarır).
