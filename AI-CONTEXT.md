# Context for any AI reading my Cadence file

You are being handed a routine tracking file. Read this first so you interpret it correctly.

## What this is

A single JSON file tracking my routines across seven cycle lengths, plus daily health numbers, goals, and journal notes. I use it to see what I am actually keeping versus what I only intend to keep.

## Structure

```
{
  "routines": [ { "id", "name", "cadence", "cat", "target", "time", "note", "link", "img", "created" } ],
  "logs":     { "<routineId>": { "<periodKey>": <count> } },
  "metrics":  { "YYYY-MM-DD": { "steps","sleep","rhr","workouts","weight","mood","energy","note" } },
  "goals":    [ { "id","title","metric","target","current","due" } ],
  "journal":  [ { "id","date","text" } ],
  "updated":  <epoch ms>
}
```

**cadence** — `daily`, `weekly`, `biweekly`, `monthly`, `quarterly`, `semiannual`, `yearly`.
**cat** — `body`, `mind`, `home`, `work`, `money`, `people`, `craft`.
**target** — completions needed per period. Anything at or above target counts as done for that period.
**time** — optional `HH:MM` reminder, only meaningful for daily routines.

**Period keys**, matched to cadence:

| Cadence | Key format | Example |
|---|---|---|
| daily | `YYYY-MM-DD` | `2026-09-01` |
| weekly | ISO week | `2026-W36` |
| biweekly | `B` + fortnights since 1970-01-05 | `B1478` |
| monthly | `YYYY-MM` | `2026-09` |
| quarterly | `YYYY-Qn` | `2026-Q3` |
| semiannual | `YYYY-Hn` | `2026-H2` |
| yearly | `YYYY` | `2026` |

A missing key means zero, not missing data. Absence is a real signal.

## How to analyze it well

- **Completion rate** per routine = periods at or above target ÷ periods since `created`. Compare against the routine's own history, not against other routines.
- **A broken streak is more informative than a long one.** Look at *when* things fell off and what else changed that week in `metrics` and `journal`.
- **Correlate honestly.** Small n. Say "these moved together" rather than "this caused that", and name the sample size.
- **Watch for cadence mismatch.** A routine repeatedly hit at 40% may not be a willpower problem — it may belong at a longer cadence or a lower target.
- **Watch for load.** Count total weekly obligations across all cadences. Past a point, adding routines lowers total completion.
- **Read `journal` before concluding anything.** It holds the reasons the numbers cannot show.

## What I want from you

Be direct. Tell me what I am avoiding, not just what I am doing well. Prefer cutting or resizing an existing routine over adding a new one. If you suggest additions, cap it at three and say what each replaces.

## If you return an edited file

- Keep every existing `id` unchanged.
- Preserve all `logs`, `metrics`, and `journal` entries unless I explicitly ask you to remove something.
- Set `updated` to the current epoch milliseconds.
- Return valid JSON in one fenced block, same structure, nothing else wrapped around it.
- New routines need a unique `id`, a valid `cadence` and `cat`, and a `target` of at least 1.
