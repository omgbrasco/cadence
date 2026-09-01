# Cadence - working notes for AI

## What this is

Cadence is Braedon's routine tracking app. It tracks routines on seven
different repeat cycles (daily, weekly, every two weeks, monthly, quarterly,
every six months, yearly), plus daily health numbers, goals, and journal
notes. It is fully static - just HTML/JS files, no server, no build step,
no account beyond GitHub. Full details are in README.md and AI-CONTEXT.md
in this folder - read those for the setup steps and the full data model.

## The data model, in short

Everything lives in one JSON file (`data/cadence.json`):

- `routines[]` - each routine has an id, name, cadence, category, target
  count per period, optional reminder time, note, link, image, created date.
- `logs` - how many times each routine was done, per period. A missing
  entry means zero, not "unknown."
- `metrics` - one entry per day: steps, sleep, resting heart rate,
  workouts, weight, mood, energy, note.
- `goals[]` - bigger targets than a single routine, with a due date.
- `journal[]` - free text notes, dated. This is where the "why" behind the
  numbers lives.
- `updated` - timestamp used to resolve sync conflicts between devices.

Rule: never change or drop an existing `id`. New ids just need to be unique.

## How Braedon wants to work

- Plain English. No jargon. If a technical word is unavoidable, explain it
  in the same sentence.
- One step at a time. Do the step, explain what happened, then stop and
  wait for confirmation before doing the next step.
- Assume non-technical. He is the product/vision person; AI does the
  engineering.

## His setup

- Main machine: Windows 11.
- Also uses: an iPhone and a MacBook Pro.
- Goal: one folder that stays in sync across all three, so his routine
  data and this app are always current no matter which device he opens.

## Who gets to touch this folder

Other AI tools (ChatGPT, Gemini, etc.) can be handed this data to read and
analyze - that is the point of AI-CONTEXT.md. But they should not modify
files in this project folder directly. Treat this repo as read-only for
other AI unless Braedon explicitly says otherwise.
