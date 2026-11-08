# Danger Danger Timer

A blind 60-second timer for the card game **Danger Danger**, built as an installable
Progressive Web App (PWA). No countdown is ever shown — you tap **Start**, a hazard
indicator pulses, and at *exactly* 60 seconds the screen flashes red with a loud,
looping alarm (and the phone vibrates on Android). Four alarm sounds to choose from.

## Files

| File | Purpose |
|------|---------|
| `index.html` | The whole app (UI, timer logic, alarm synth). |
| `manifest.webmanifest` | Makes it installable (name, icons, standalone display). |
| `service-worker.js` | Caches the app so it works offline once loaded. |
| `icons/` | App icons (192, 512, maskable, Apple touch, favicon.ico). |

The timer fires at a fixed **60000 ms**. Only the countdown display is hidden — the
duration itself is locked to the game rule, with no rounding or randomness.

## Run it locally (on this PC) to test

A PWA needs to be served over `http://localhost` or HTTPS — opening the file directly
(`file://`) won't let the service worker or "Install" work. From this folder:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000` in Chrome.

## Put it on your Android phone

You need to host the folder on a free HTTPS host, then "Add to Home Screen" on the phone.
The easiest option is **GitHub Pages**:

1. Create a new GitHub repo and upload every file in this folder (keep the `icons/` folder).
2. In the repo: **Settings → Pages → Build and deployment → Source: Deploy from a branch**,
   pick `main` and `/ (root)`, then **Save**.
3. Wait ~1 minute. GitHub gives you a URL like `https://<you>.github.io/<repo>/`.
4. On your Android phone, open that URL in **Chrome**.
5. Tap the **⋮** menu → **Add to Home screen** (or **Install app** if it's offered).
6. Launch it from the new icon — it opens fullscreen, no browser bars, and works offline.

Other free hosts work the same way (Netlify Drop, Cloudflare Pages, Vercel): upload the
folder, open the HTTPS URL on your phone, Add to Home Screen.

## Notes

- **Sound** only starts after you tap Start — browsers block audio until you interact.
- **Keep the app in the foreground** while the timer runs. A screen wake-lock keeps the
  display on, but a backgrounded phone browser may throttle timers; the PWA (launched from
  the home-screen icon) is the most reliable way to run it.
- To change the app later, edit `index.html`, then bump `CACHE = "dd-timer-v1"` in
  `service-worker.js` (e.g. to `-v2`) so installed copies pull the update.
