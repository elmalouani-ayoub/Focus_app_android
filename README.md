# DETONATE — Android

Same app as the iOS build, with Android's capabilities switched on. Everything is feature-detected, so this version also runs correctly on iPhone if you'd rather keep one repository.

## Install (2 minutes)

1. Create a new public GitHub repo, e.g. `detonate-android`.
2. Upload everything in this folder, keeping the structure — including the hidden `.nojekyll` file and the `.well-known` folder. On github.com use **Add file → Upload files** and drag the whole folder in.
3. Repo → **Settings → Pages** → Source: *Deploy from a branch* → Branch `main`, folder `/ (root)` → Save.
4. Open `https://YOURNAME.github.io/detonate-android/` in **Chrome on Android**.
5. Either tap **Install** in the app's Configuration list, or use Chrome's ⋮ menu → **Add to Home screen → Install**.

Chrome builds a real WebAPK from this: its own launcher icon, its own entry in the app drawer and in Settings → Apps, a splash screen, no browser UI, and it works with no signal.

**Long-press the launcher icon** for shortcuts: start a 25-minute block, start a 50-minute block, or jump straight to the sideways clock.

## What's different from the iOS build

| | |
|---|---|
| **Haptics** | The phone ticks with the clock through the last ten seconds, hammers a five-pulse pattern at zero, and thumps a heartbeat while you're overdue. Toggle in Configuration. |
| **Run in background** | Keeps the clock alive with the screen off or the app backgrounded, so the alarm still fires. Also posts a notification the instant a block ends. |
| **Lock screen** | While a block runs, the remaining time and the task name appear in your notification shade and on the lock screen, with a progress bar. The stop control there defuses the block. |
| **Back gesture** | Backing out of the sideways clock returns to the app instead of quitting it. |
| **Adaptive icon** | Proper maskable icon, plus a monochrome version for Android 13+ themed icons and the notification badge. |
| **Orientation** | Unlocked in the manifest — without this, an installed Android PWA is pinned to portrait and the sideways clock can never open. |

*If you already pushed the iOS repo:* its `manifest.json` has `"orientation": "portrait"`. iOS ignores that field, so nothing is broken today, but change it to `"any"` if you ever install that build on Android.

**A note on "Run in background":** it works by playing a silent looping track, which is what stops Android freezing the timer. Some phones will show a small media notification while a block runs. That's the mechanism, not a bug — and manufacturer battery optimisation (Samsung, Xiaomi, OnePlus especially) can still kill it. If the alarm gets unreliable, exempt DETONATE from battery optimisation in Settings → Apps → DETONATE → Battery → Unrestricted.

## Optional: a real installable APK

The WebAPK above is already a real app for everyday use. If you specifically want an `.apk` file to sideload or to publish on Play, wrap it as a Trusted Web Activity:

```bash
npm install -g @bubblewrap/cli
bubblewrap init --manifest https://YOURNAME.github.io/detonate-android/manifest.json
bubblewrap build
```

Bubblewrap installs the JDK and Android SDK on first run and outputs `app-release-signed.apk`.

To make the APK open with no URL bar, take the SHA-256 fingerprint that `bubblewrap` prints, paste it into `.well-known/assetlinks.json` (replacing the placeholder), set `package_name` to whatever you gave Bubblewrap, and push. The `.nojekyll` file is what makes GitHub Pages serve that dot-folder at all.

## Notes

- **Sound.** Android needs one tap before any audio plays; the first Arm press does it.
- **Data.** Everything is stored on the device. Nothing leaves your phone, and there's no account, no network call, no analytics.
- **Updating.** Edit the files on GitHub, then bump `CACHE = "detonate-a1"` in `sw.js` to `a2`. Without that, installed copies keep serving the cached old version.
