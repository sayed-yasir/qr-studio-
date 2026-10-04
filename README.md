# QR Studio v1 — Online-ready build

Static, client-side QR workspace. QR codes are generated in the browser; there is no backend, account system, analytics or QR API.

## Features
- 12 QR types: URL, Text, Wi-Fi, Email, Phone, Contact, Location, SMS, Event, Social, WhatsApp, App link
- Error correction L / M / Q / H (a logo always forces H)
- Designer: colors, whole-QR gradients, dot style, margin, frames, logo
- Export PNG / SVG / JPG / WebP, copy, print. Raster exports are enlarged automatically if needed so each module stays >= 6 px.
- Batch (up to 500, CSV first column or TXT) with ZIP download
- Local library (stored in this browser only), templates, QR inspector, dark mode, PWA (installable, works offline)

## Run locally
```bash
cd QR-Studio
python3 -m http.server 8080
# open http://localhost:8080/
```
Camera, service worker and clipboard need HTTPS or localhost.

## Scanner
1. Uses the browser's native `BarcodeDetector` when it supports QR (Chrome/Edge/Samsung on Android and desktop).
2. Otherwise it loads `vendor/jsQR.js` if present (Firefox, Safari/iOS).
   Install it once (Apache-2.0, ~130 KB):
   ```bash
   curl -L -o vendor/jsQR.js https://cdn.jsdelivr.net/npm/jsqr@1.4.0/dist/jsQR.js
   ```
   Keep jsQR's license notice with it. Without that file, those browsers see a clear "not available" message.

## Deploy
Any static host with HTTPS: Cloudflare Pages, Netlify, GitHub Pages (upload the contents of this folder).
- `_headers` (Netlify / Cloudflare Pages) adds security headers. GitHub Pages ignores it; the CSP `<meta>` in `index.html` still applies.
- On every release change `CACHE` in `sw.js`.
- Online the service worker always fetches fresh files; the cache is only used offline.

## Privacy
Everything stays in the browser. History is stored in `localStorage` on the user's device only. Wi-Fi passwords are hidden in the history list but are kept in that local storage so a code can be edited later; use "Clear all" on shared devices.

## Known limits
- Max content is about 1,270 bytes at level H (more at L). Dense codes need a large size or a good print.
- The inspector and color checks are aids, not a guarantee for every scanner or printer.
