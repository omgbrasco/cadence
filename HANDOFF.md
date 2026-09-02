# Handoff

This file exists so nothing is lost. Cadence was designed in a conversation with Claude in the Claude app on 1 September 2026. That chat is gone or going. Everything that mattered from it is written down here.

Read this first, then `README.md`, then `AI-CONTEXT.md`.

---

## Who this is for

Braedon Scott. GitHub username `omgbrasco`.

**Devices:** Windows PC is the main machine. Also an iPhone and a MacBook Pro. Everything has to work on all three.

**How he wants to be talked to — this matters, follow it:**
- Plain English. No jargon.
- One step at a time. Stop and wait for confirmation before moving to the next step.
- No dense bulleted summaries. Explain things.
- If something can't be done, say so directly and say why, rather than working around it silently.

---

## What he asked for, in his own framing

A single place that helps him see and improve his routines across every timescale — daily, weekly, biweekly, monthly, quarterly, every six months, yearly. With tracking, progress, goals, references, pictures, data, and memory.

Then, specifically:

- One folder, reachable from the phone, the Windows PC, and the MacBook Pro
- Continuity — the context is always there, nothing has to be re-explained
- Other AI can plug in, read it, and consult on it — but **cannot break anything**
- Installable on the phone like a real app, with notifications
- Able to take in Apple Watch, phone, fitness, and location data
- Fast changes on the fly while he's using it
- Possibly a real product someday. Nothing should block that.

---

## What exists right now

A static web app. No server, no build step, no framework. Just files.

```
cadence/
├── index.html              the entire app, one file
├── manifest.webmanifest    makes it installable
├── sw.js                   offline support, notification clicks
├── icons/                  four PNGs, concentric-ring mark
├── data/cadence.json       seed data + the canonical schema
├── README.md               setup and data model
├── AI-CONTEXT.md           primer to hand any AI before giving it the data
└── HANDOFF.md              this file
```

**The app has:** seven cadence tracks with completion percentages and time-remaining, routines with targets, streaks, an 8-period history strip, notes/links/images per routine, a Signals tab for health numbers with a 30-day trend, goals with progress bars, a journal, an AI bridge tab, and a Sync tab.

**It has been tested.** Period math, streak counting, the merge function, ICS generation, and health-row parsing all pass.

---

## The architecture, and why

**One JSON file is the whole system.** Routines, logs, metrics, goals, journal. Everything an AI needs to reason about him is in one readable object. This was deliberate — no database, nothing an AI has to be taught to query.

**Sync is a private GitHub gist.** Every device reads and writes the same gist. Not set up yet. The app pulls on open and on window focus, pushes after changes.

**Merge, not overwrite.** Two devices editing at once union their routines, metrics, and journal entries; the more recently edited state wins direct conflicts. Written so a check-off on the phone is never lost to a stale desktop tab.

**Read/write asymmetry is the safety model.** Other AIs get the raw gist URL — read-only. Writing needs a GitHub token that lives only in the browser storage of his own devices and is never written into the synced file. This is the "AI can look but not fuck anything up" requirement, and it should not be weakened.

**Reminders go through a calendar file, not the browser.** The app generates an `.ics` with real RRULEs per cadence. Browser notifications are unreliable on iOS; Apple Calendar is not. In-app notifications exist as a secondary path.

---

## Decisions already made — don't relitigate without asking

- **Static files over a framework.** He should be able to open `index.html` and have it work. No npm, no build.
- **GitHub Pages for hosting.** Free, no account beyond GitHub, and the repo doubles as the backup.
- **The token is his to create.** Credentials are never handled on his behalf.
- **Health data comes in by paste, not API.** No web app can read HealthKit. The path is: export from Health → ask any AI to reformat to `YYYY-MM-DD, steps N, sleep N, rhr N` → paste into the app's import box. An iOS Shortcuts automation could push to the gist later, but only after the daily habit sticks.
- **Design is deliberate.** Warm grey ground, pine and brass accents, serif for numbers. Concentric rings as the mark, one ring per cadence. Not a template.

---

## Where things stand

Done:
- App built and verified working locally in Chrome on the Windows PC
- Folder unzipped to the desktop
- GitHub account confirmed (`omgbrasco`)
- Claude Code opened in the folder

Not done yet:
- Repo created and pushed
- GitHub Pages turned on
- App installed to the iPhone home screen and to the MacBook
- Gist created, token made, Sync tab filled in on each device
- Calendar file generated and added to Apple Calendar
- Routines edited from the seeded defaults into his actual life
- Any real data logged

---

## Things worth knowing that came up

- The Claude app's built-in GitHub integration is **read-only** — file names, contents, branch content. It cannot create repos or push. That's why the work moved to Claude Code.
- Claude app memory is per-project and doesn't reach Claude Code. That's what this file and `CLAUDE.md` are for.
- He had an older global preference set to "extremely minimal, TLDR" that fought against the slower style he actually wants. He replaced it. If replies start getting terse and clipped, that's the cause.

---

## Where to take it

Ideas raised but not built: habit stacking, location-triggered routines, a weekly review template, an iOS Shortcut that posts health data to the gist automatically.

The bigger point: he mentioned making this "a real thing" eventually. Nothing in the current setup prevents that. Keep the data model stable, keep it dependency-free, and keep the read/write asymmetry intact.
