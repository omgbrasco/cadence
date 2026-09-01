# Cadence

A routine system that runs across every cycle — daily, weekly, every two weeks, monthly, quarterly, every six months, yearly — with tracking, streaks, goals, health signals, notes, and a plain JSON file any AI can read.

Everything is static files. No server, no build step, no account beyond GitHub.

```
cadence/
├── index.html              the whole app
├── manifest.webmanifest    makes it installable as an app
├── sw.js                   offline support + notification clicks
├── icons/                  app icons
├── data/cadence.json       starter data, and the shape every AI should expect
├── AI-CONTEXT.md           paste this into any AI before handing it your data
└── README.md               this file
```

---

## 1. Put it online (once, ~5 minutes)

You need it hosted so the phone, PC, and MacBook all reach the same URL.

1. Go to github.com and create a new **public** repository named `cadence`.
2. Click **uploading an existing file** and drag in everything from this folder, keeping the `icons/` and `data/` folders intact.
3. Repo **Settings → Pages**. Under Source pick **Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
4. Wait about a minute. Your app is at `https://YOURNAME.github.io/cadence/`.

The repo being public only exposes the app code. Your actual routine data lives somewhere else — see step 3.

## 2. Install it as an app

- **iPhone / iPad** — open the URL in **Safari** (not Chrome), tap Share, then **Add to Home Screen**. It launches full screen with its own icon.
- **Android** — open in Chrome, menu, **Install app**.
- **Windows / Mac** — open in Chrome or Edge, click the install icon in the address bar. It gets a dock or taskbar icon and its own window.

It works offline after the first load. Changes save instantly on the device you are using.

## 3. Sync across all your devices

Your data lives in one private gist that every device reads and writes.

1. Go to **gist.github.com**. Filename `cadence.json`, paste in the contents of `data/cadence.json`. Click **Create secret gist**.
2. Copy the **gist ID** from the URL — the long string after your username.
3. Go to **github.com/settings/tokens** → Tokens (classic) → **Generate new token**. Name it `cadence`, tick only the **gist** scope, no expiry or a long one. Copy the token.
4. Open the app, go to **Sync**, paste the gist ID and token, hit **Save connection**.
5. Repeat step 4 on every other device. Same gist ID, same token.

From then on: the app pulls when you open or switch back to it, and pushes after every change. Edits from two devices merge rather than overwrite — completions, metrics, and notes union together, and the more recently edited device wins any direct conflict.

**About the token.** It stays in that device's browser storage, is never written into the synced file, and can only touch gists. If you lose a device, delete the token on github.com and make a new one.

## 4. Reminders on your phone

Two ways, and the second is the reliable one.

- **In-app reminders** — Sync tab → *Turn on reminders*. Fires for daily routines that have a time set. Works while the app has been open recently. iOS is stingy about this.
- **Calendar file (recommended)** — AI bridge tab → *Download calendar file*. Open the `.ics` on your Mac or phone and add it to Apple Calendar or import it into Google Calendar. Every routine lands on its real repeat schedule with an alert, on every device you already own. Re-download it whenever you change your routines.

## 5. Plug AI into it

Pick whichever fits the moment.

- **Fast, any AI** — AI bridge tab → *Copy data + review prompt*. Paste into Claude, ChatGPT, Gemini, whatever. It gets your whole file plus a coaching brief and returns updated JSON. Paste that back into the import box.
- **Hands-free, for AIs that browse** — give the AI the raw gist URL: open your gist, click **Raw**, copy that URL. Any AI that can fetch a page reads your live data with no copy-paste. Anyone with that URL can read it too, so treat it as semi-private.
- **Claude with the GitHub connector** — point it at the repo and gist directly, and it can propose edits to the file.

Whatever you use, hand it `AI-CONTEXT.md` first so it knows what the fields mean.

## 6. Getting your Apple Watch and phone data in

There is no legal way for a web app to read HealthKit directly. Two paths that work today:

- **Manual** — Signals tab, ten seconds a day. Steps, sleep, resting HR, workouts, weight, mood, energy.
- **Bulk** — export from Health (Profile → Export All Health Data) or an app like Health Auto Export, then hand the file to an AI with this instruction: *"Convert this to one line per day in the format `YYYY-MM-DD, steps N, sleep N, rhr N, workouts N, weight N`."* Paste the result into AI bridge → Import. It merges straight into Signals.
- **Automatic, if you want to go further** — an iOS Shortcuts automation can read Health values on a schedule and POST them to the gist. Worth building once the daily habit is stuck, not before.

---

## The data model

One JSON object. Stable enough that an AI can edit it without breaking anything.

| Field | Shape | Meaning |
|---|---|---|
| `routines[]` | `{id, name, cadence, cat, target, time, note, link, img, created}` | `cadence` is one of `daily, weekly, biweekly, monthly, quarterly, semiannual, yearly`. `target` is how many times per period. `cat` is one of `body, mind, home, work, money, people, craft`. |
| `logs` | `{routineId: {periodKey: count}}` | Period keys: `2026-09-01`, `2026-W35`, `B1478`, `2026-09`, `2026-Q3`, `2026-H2`, `2026`. |
| `metrics` | `{"YYYY-MM-DD": {steps, sleep, rhr, workouts, weight, mood, energy, note}}` | One record per day. |
| `goals[]` | `{id, title, metric, target, current, due}` | Bigger than a routine. |
| `journal[]` | `{id, date, text}` | The memory. Context the numbers miss. |
| `updated` | epoch ms | Drives sync conflict resolution. Bump it when you edit the file by hand. |

Keep existing `id` values when editing. New ones just need to be unique.

## If something breaks

- **Blank page after an update** — hard refresh (Ctrl/Cmd + Shift + R). The service worker caches aggressively.
- **Sync error 401** — the token was rejected. Regenerate it and re-save.
- **Sync error 404** — wrong gist ID, or the token lacks gist scope.
- **Lost data** — AI bridge → Download backup, regularly. The gist also keeps full revision history; open it on github.com and click Revisions.
