# Changelog

Plain-English record of every version of this app. Newest first.

## 2026-09-02 — More bug fixes: syncing and routine cards

**What changed:** A second pass over the sync logic and the newer
screens (Feedback tab, routine cards). Fixed six things:
- Your "push automatically" on/off setting could get silently flipped
  back on by a sync from another device, if that device had never
  touched the setting itself.
- Syncing with the cloud in the background (e.g. switching back to the
  app) could wipe out something you were mid-typing — a journal entry,
  a new routine, edited numbers — with no warning. Now a background
  sync won't redraw the screen while you're actively typing in a box;
  it still saves what came in, you just see it once you're done typing.
- On a routine with a "times per period" target above 1, tapping the
  `+` button one time too many after reaching the target reset that
  whole period's count back to zero instead of just adding one more.
  Now `+`/`-` on those routines behave like a plain counter.
- Deleting one routine could silently close another routine's
  half-finished edit form (if you had one open) without saving or
  warning you.
- A routine imported from pasted JSON (e.g. an AI's response) with an
  unrecognized repeat cycle spelled wrong could crash the Today and
  Routines screens until the bad entry was removed by hand. Now it
  falls back to a sane default instead of crashing.
- A routine's "Reference link" is only shown as a clickable link if it
  actually starts with `http://` or `https://` — closes off a way a
  pasted-in link could otherwise run script code when tapped.

**Couldn't do:** The underlying sync design (whichever device saved
most recently "wins" for anything it touched) can still bring back a
routine, goal, journal entry, or feedback note that was deleted on one
device if the other device didn't have that deletion yet when it
synced. Making deletions sync properly needs a bigger change to how
the data is stored (tracking "this was deleted" instead of just
"this exists"), which is more than a bug fix — flagging it here as
something to plan for on purpose next, not fixing it as a side effect
of this pass. No browser was available in this environment, so these
fixes are verified by careful reading and tracing through the code
by hand, not by clicking through the real app — worth a look next
time you have it open, especially the mid-typing sync behavior.

Every entry here has a matching commit in git, so any version can be
recovered exactly. To go back: `git log` shows the commit list, and
`git checkout <commit-id> -- .` restores the files from that point
(ask before running anything like that — it changes files on disk).
The synced data itself (your routines, logs, etc.) also has its own
history on github.com under the gist's "Revisions" link, separate
from this file.

## 2026-09-02 — Health data mailbox

**What changed:** The app now checks for a file called `health-inbox.txt`
in the synced gist every time it syncs. If a phone Shortcut (or anything
else) drops a line like `2026-09-02 steps 8400 sleep 7.1 rhr 54` in
there, the app reads it, files the numbers under that date the same way
manual paste-import already does, then clears the mailbox so it's not
imported twice. Nothing changes if the file is empty or missing - the
manual paste-import box still works exactly as before, this is just a
second, automatic way in.

**Couldn't do:** The phone-side half (the actual Shortcuts automation
that reads Health and writes to this file) still needs to be built on
the phone - that can't be done from here.

## 2026-09-02 — Bug fix: wrong streaks/history near month-end

**What changed:** Found and fixed a real bug in the date math for
monthly, every-3-months, every-6-months, and yearly routines. If
"today" fell on a day that doesn't exist in the previous month (like
the 29th–31st, or Feb 29 for yearly routines made on a leap day), the
app's "go back one period" math landed back in the *same* month
instead of the one before it. That made streak counts too high and
could scramble the little history dots on a routine's detail view. I
wrote a standalone test script (not part of the app) that checked this
and the ISO week-number math against known-correct values across the
last 5+ years and several timezones — the week-number math was fine,
this month math was not. Fixed by clamping the day to a valid one in
the target month instead of letting it silently roll into the wrong
month. Re-ran the same tests after the fix to confirm it's correct
and that daily/weekly/every-2-weeks routines (which didn't have this
problem) are unaffected.

**Couldn't do:** Nothing outstanding from this pass.

## 2026-09-02 — Feedback and Last Run

**What changed:** Added a "Feedback" tab. It has two parts on one screen:
- **Last run** at the top, showing the most recent report written by an
  AI session that worked on this project (date, what changed, anything
  it couldn't do).
- **Feedback** below, where you can leave notes for whichever AI works
  on this next. Entries save into the synced gist as `feedback.json`,
  the same way routines sync today. A small dot appears on the Feedback
  tab when there's a report you haven't seen yet.

**Couldn't do:** Nothing yet writes to `reports.json` automatically —
that's the other half of this loop, and it only fills in once an AI
session actually writes a report there after finishing work.

## 2026-09-02 — Liquid glass redesign

**What changed:** Full visual redesign. Dark, frosted "liquid glass"
look, bigger text and tap targets throughout, high contrast (asked for
specifically due to astigmatism). Cut the 7 top tabs down to a bottom
navigation bar with 4: Today, Routines, Insights (folds in Signals,
Goals, and Journal), and More (folds in AI bridge and Sync). Routine
editing moved from browser popup boxes to an on-screen form.

**Couldn't do:** Nothing outstanding from this pass.

## 2026-09-01 — Initial commit

**What changed:** First version pushed to GitHub. Static routine
tracker: seven cadences (daily through yearly), health Signals,
Goals, Journal, gist-based sync across devices, installable as a
PWA, calendar export for reminders.

**Couldn't do:** GitHub Pages and device installs were still manual
follow-up steps for Braedon to do himself.
