# DETONATE for Android
 
**A focus timer that doesn't stop when the time runs out.**
 
Most timers go off and forget about it. This one keeps counting — upward, in red — until you actually stop. Then it hands you a break sized to the block you just finished, and takes back some of it if you ran late.
 
It installs as a real Android app: its own launcher icon, its own entry in the app drawer, no browser UI, works offline. No account, no server, no analytics, no dependencies, no Play Store.
 
<img src="shots/01-timer.png" width="250"> <img src="shots/02-overdue.png" width="250"> <img src="shots/03-rest.png" width="250">
 
---
 
## What it does
 
**Set a block, name it, arm it.** Anything from 1 to 60 minutes. Type `Draft the intro 15` and it parses the 15 for you.
 
**Then it counts down like a bomb.** Seven-segment digits, a blinking colon, a click every second that doubles in rate and jumps in pitch for the final ten — and the phone ticks in your hand along with it.
 
**When it hits zero, it doesn't stop.** A klaxon sounds, the screen flashes, the phone hammers a five-pulse pattern, the minus sign lights, and the clock counts *up*. `-02:47` means you're nearly three minutes into work you said would already be done. A heartbeat buzzes every two seconds until you stop it. There's no auto-stop, because the whole point is to see the overrun.
 
**Your break is earned, not fixed.** Finish a block and you get a rest worth 20% of it — a 10-minute block earns 2 minutes, a 50-minute block earns 10. Every fourth block earns 30. The display turns amber. Run over into your break and it goes red again.
 
**Overtime is a debt.** Minutes you ran late come out of your rest. It never eats more than three-quarters of it, so a break can't vanish entirely.
 
**Queue up a session.** Add tasks with durations and they run in order, each one auto-arming when the previous rest ends. Unfinished blocks stay in the queue.
 
**Turn the phone sideways** and the timer takes over the whole screen — digits only, black everywhere else. If you keep rotation lock on, tap the display instead: it rotates in software so you can stand the phone on its side against a monitor. Controls fade out after four seconds; the back gesture returns to the app instead of quitting it. The digits drift a few pixels every minute to spare the OLED.
 
<img src="shots/04-clock.png" width="620">
**It keeps running with the screen off.** The countdown survives backgrounding, the alarm still fires, and while a block runs the remaining time and task name appear on your lock screen and in the notification shade with a progress bar. The moment a block ends you get a notification.
 
**Long-press the launcher icon** for shortcuts: start a 25-minute block, start a 50-minute block, or jump straight to the sideways clock.
 
**It logs what actually happened.** Every block records what you predicted against what you spent, and the Log tab turns that into a calibration figure. If it reads 140%, your instinct is packing two hours of work into ninety minutes.
 
<img src="shots/05-queue.png" width="250"> <img src="shots/06-log.png" width="250">
 
---
 
## Why it's built this way
 
Four ideas from the literature, and one of them is the reason the breaks aren't optional.
 
**Timeboxing beats working-until-done.** Committing a fixed amount of time to a task forces the scope decision up front and closes off the drift into "just a bit longer."
 
**The break is the mechanism, not the reward.** Performance on a single continuous task decays on its own — the *vigilance decrement*. Ariga and Lleras argued in 2011 that this isn't a fuel tank emptying but **goal habituation**: the brain stops registering a goal that never changes, the way it tunes out a constant sound. Their fix was brief interruptions, and participants given two short diversions during a fifty-minute task held their accuracy flat while the control group's fell away. So the break exists to *deactivate* the goal so it can be reactivated fresh.
<sub>Ariga & Lleras, *Cognition* 118(3), 2011</sub>
 
**Which is why rest scales with the block.** A fixed five minutes is Pomodoro's convention, not a law. A 2022 meta-analysis of 22 micro-break studies found short breaks reliably cut fatigue and raised vigor, but the performance benefit grew with break length — and the authors concluded that recovering from heavy cognitive work likely needs more than ten minutes. A 50-minute block and a 10-minute block should not earn the same pause.
<sub>Albulescu et al., *PLOS ONE* 17(8), 2022</sub>
 
**The overtime counter exists to fix your estimates.** You're systematically bad at predicting how long your own work takes — the *planning fallacy* — and it doesn't improve from doing more work, only from feedback. Every block here records predicted against actual, which is the raw material for recalibration.
 
**And there's a limit to how much urgency helps.** Amabile's diary study of 177 people found that time pressure generally *reduced* creative thinking. The exception mattered: high-pressure days still produced creative work when people felt they were on a mission — focused, uninterrupted, clear on why the deadline existed — and collapsed when they felt like they were on a treadmill. Use tight blocks for defined execution work. For genuinely open-ended thinking, set a long block and ignore the countdown.
<sub>Amabile, Hadley & Kramer, *Harvard Business Review*, Aug 2002</sub>
 
The full version of this lives in the app's **Brief** tab, so it's there when you need the argument rather than buried in a repo.
 
---
 
## Install
 
1. Create a public GitHub repo and upload everything in this folder, **keeping the structure** — including the hidden `.nojekyll` file and the `.well-known` folder. Drag the whole folder into **Add file → Upload files**.
2. **Settings → Pages** → Source: *Deploy from a branch* → Branch `main`, folder `/ (root)` → Save.
3. Open `https://YOURNAME.github.io/REPO/` in **Chrome on Android**.
4. Tap **Install** in the app's Configuration list, or Chrome's ⋮ menu → **Add to Home screen → Install**.
Chrome builds a **WebAPK** from this — a genuine installed app with a splash screen, an adaptive icon, and an entry in Settings → Apps. Not a bookmark.
 
### Optional: a real APK
 
The WebAPK is already a real app for everyday use. If you specifically want an `.apk` to sideload or publish:
 
```bash
npm install -g @bubblewrap/cli
bubblewrap init --manifest https://YOURNAME.github.io/REPO/manifest.json
bubblewrap build
```
 
Bubblewrap installs the JDK and Android SDK on first run and outputs `app-release-signed.apk`. To make it open without a URL bar, paste the SHA-256 fingerprint Bubblewrap prints into `.well-known/assetlinks.json`, set `package_name` to match, and push. The `.nojekyll` file is what makes GitHub Pages serve that dot-folder at all.
 
---
 
## Settings
 
| | |
|---|---|
| **Rest ratio** | Share of the finished block that becomes a short rest. Default 20%, adjustable 5–60%. |
| **Long rest** | Length of the rest after every fourth block. Default 30 minutes. |
| **Overtime debt** | Late minutes come out of your rest. Floored at a quarter of the earned break. |
| **Auto-advance** | Arm the next queued block automatically when a rest ends. |
| **Ticking** | Per-second clicks, accelerating in the last ten. |
| **Haptics** | Vibration through the countdown, at zero, and while overdue. |
| **Run in background** | Keeps the clock alive with the screen off; lock-screen display; end-of-block notifications. |
| **Sideways clock** | Landscape takes over the screen; tap the display to rotate in software. |
| **Keep screen awake** | Holds the display on while a block runs, via the Wake Lock API. |
 
---
 
## How "Run in background" works, and when it doesn't
 
Android freezes background pages, which would stall the timer. The app works around this by playing a silent looping track, which marks it as playing media — that's also what puts the countdown on your lock screen. Some phones will show a small media notification while a block runs. That's the mechanism, not a bug.
 
Manufacturer battery optimisation can still kill it, Samsung, Xiaomi and OnePlus especially. If the alarm becomes unreliable, set **Settings → Apps → DETONATE → Battery → Unrestricted**.
 
The clock itself never drifts either way: it's derived from a stored timestamp, not from counting ticks, so it shows the correct time the instant you return regardless of what the OS did while you were away.
 
---
 
## Under the hood
 
| File | |
|---|---|
| `index.html` | The entire app — markup, styles, logic. No build step, no dependencies, no framework. |
| `manifest.json` | App identity, adaptive icons, launcher shortcuts, unlocked orientation. |
| `sw.js` | Service worker; offline cache and notification handling. |
| `icon-*.png` | Standard, maskable (adaptive) and monochrome (themed / badge) icons. |
| `.well-known/assetlinks.json` | Digital Asset Links template, only needed for the APK route. |
| `.nojekyll` | Makes GitHub Pages serve the `.well-known` folder. |
 
The seven-segment digits are drawn in CSS — each digit is seven clipped `<span>`s with unlit ghost segments sitting behind at 5% opacity, which is what gives it the LED-panel look without shipping a font. Sound is synthesised with the Web Audio API and the keep-alive track is generated as a WAV in JavaScript, so there are no audio assets either.
 
Every Android-specific capability is feature-detected, so this build also runs correctly on iPhone — the haptics row simply hides itself. You can use one repo for both platforms if maintaining two gets annoying.
 
**Editing it:** open `index.html` in any editor. Colours are CSS custom properties at the top (`--red`, `--amber`, `--void`). Rest logic is the `restFor()` function. If you change anything, bump `CACHE = "detonate-a1"` in `sw.js` or installed copies will keep serving the cached old version.
 
---
 
## Privacy
 
Everything is stored on the device. No account, no sync, no network requests of any kind after the first load. The app has no idea what you're working on.
 
---
 
The bomb is decoration. Nothing here is a real deadline unless you decide it is, and running a punishing schedule for weeks costs more than it earns.
