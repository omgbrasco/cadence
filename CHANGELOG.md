# Changelog

Plain-English record of every version of this app. Newest first.

Every entry here has a matching commit in git, so any version can be
recovered exactly. To go back: `git log` shows the commit list, and
`git checkout <commit-id> -- .` restores the files from that point
(ask before running anything like that — it changes files on disk).
The synced data itself (your routines, logs, etc.) also has its own
history on github.com under the gist's "Revisions" link, separate
from this file.

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
