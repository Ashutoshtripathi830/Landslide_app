# YAKSHA Landslide Monitor — PWA

Installable app version of the receiver dashboard. Same live data, dark tactical
theme, admin panel, map, geofence control, SD log download, recalibrate, WiFi reset.
Difference from the on-device page: it's a separate app with its own icon on your
home screen, and it remembers the receiver's address so you don't retype an IP.

## Files
- `1_index.html` — the app (rename to `index.html` when hosting)
- `2_manifest.json` — PWA install manifest
- `3_service-worker.js` — caches the app shell for offline install, never caches live data
- `4_icon-192.png`, `5_icon-512.png` — app icons
- `6_receiver_alert_relay_soil_CORS.ino` — your firmware + one CORS header added to
  `/data`, `/recalibrate`, `/setgeofence`, `/resetwifi`. **Required** — flash this
  instead of the original, or the PWA's fetch calls will be blocked by the browser
  (the PWA is a different origin than the ESP32, so it needs the ESP32's permission
  to read the responses).

## Where to host the PWA files
Rename `1_index.html` → `index.html`, `2_manifest.json` → `manifest.json`,
`3_service-worker.js` → `service-worker.js`. Keep all 5 files in the same folder.
Any static host works:
- GitHub Pages (free, easiest — push these 5 files to a repo, enable Pages)
- Netlify/Vercel drag-and-drop deploy
- Or even the ESP32 itself later if you want zero external dependency — LittleFS
  can serve these as static files, but a free static host is simpler to iterate on

PWAs need HTTPS to install (except localhost), so plain `file://` won't offer the
"Add to Home Screen" prompt — any of the hosts above give you HTTPS automatically.

## First run
1. Open the hosted URL on your phone.
2. Tap the ⚙ gear top-right, enter the receiver's address:
   - `landslide.local` if your phone is on the same WiFi as the receiver, or
   - the receiver's IP shown on its OLED / dashboard, or
   - `192.168.4.1` if the receiver is still in its own "Landslide-Setup" AP mode
3. Save. It's remembered from then on (localStorage), no need to re-enter.
4. Browser menu → "Add to Home Screen" / "Install app" → launches full-screen,
   no browser chrome, just the dashboard.

## Notes
- Data only flows when your phone and the receiver are on the same network (or
  you're connected to the receiver's own AP). This is a local dashboard, not a
  cloud service — nothing leaves your network.
- If "Cannot reach ___" shows up, it's almost always the address in settings or
  being on a different WiFi/AP than the receiver.
