# HipMarvin FX — Standing Protocol v2

This is the single canonical rule set referenced by WEEKLY_RESEARCH_TEMPLATE.md,
DAILY_UPDATE_TEMPLATE.md, POSITION_LEDGER_TEMPLATE.md, and
HipMarvinFX_Generation_Prompts_v4.md. If a rule is cited by number anywhere in
those files, this document is the definition it points to.

Rules are grouped by scope: Global (apply at every phase), then
phase-specific (Weekly Outlook, Daily Update, Week-End Review, Position
Ledger). Global rules are numbered first and never renumbered — phase-specific
rules are added after them, so a rule's number stays stable as this document
evolves.

---

## GLOBAL RULES (apply at every phase)

**Rule 1 — Never fabricate.**
Never fabricate a number, forecast, actual, COT figure, price, Friday close,
pip result, or resolution. No source = write "Pending," "not sourced," or
"no actual yet" in that exact field — don't estimate or guess to fill a gap.

**Rule 2 — Sourcing separation.**
Forecasts/priors come only from the attached FF screenshot. Speech/testimony
content comes only from web search, cited by source. Entry/stop/target levels
come only from charts, or are clearly labeled as an inferred technical read —
never presented as sourced when they're inferred.

**Rule 3 — Status only from actual recorded price action.**
Any status, verdict, or outcome (trade status, scenario verdict, ledger
status) is only ever changed based on actual recorded price action or a
sourced actual — checked against the full relevant range, not just the
latest print. Never inferred from vibes or narrative alone.

**Rule 4 — Every trade idea gets a stop.**
The moment a trade idea is written down — in the Weekly Outlook or the
Position Ledger — it gets a stop. No exceptions, no "stop to follow."

---

## WEEKLY OUTLOOK PHASE RULES

**Rule 5 — Correlation check.**
Before finalizing the Trade Priority List: check whether most entries bet
against the Scenario Matrix's own highest-probability branch, or whether
several "different" ideas are really one correlated bet (e.g. four
USD-direction trades wearing four pair names). If so, say that plainly in
the list itself — don't present it as independent, equally-weighted ideas.

**Rule 6 — Daily Game Plan is forward-only.**
The Daily Game Plan synthesizes the Trade Priority List and Scenario Matrix
into forward guidance only (what to watch, discipline windows, reassessment
triggers). It never resolves or verdicts anything — that's the Daily Update
file's job once the day actually happens. Don't duplicate Daily Update
content backward into this section.

**Rule 7 — Carryover is noted, not resolved, here.**
If a carried-over position from last week is running opposite this week's
new idea on the same pair, note it once in plain text in the Weekly Outlook.
Full resolution happens in the Position Ledger, not in this file.

---

## DAILY UPDATE PHASE RULES

**Rule 8 — Partial vs. Closed discipline.**
Don't mark a day "Closed" unless every scheduled event that day has happened
and been sourced. Otherwise mark "Pending" or "Partial" and say what's still
outstanding.

**Rule 9 — Only add what's actually supported.**
Only add or revise what today's new input actually supports — leave
everything else in the file exactly as it stands.

**Rule 10 — Verdict values.**
Verdict (only set on the run that actually closes the day) is one of:
Pending / Confirmed / Invalidated / Mixed.

**Rule 11 — Flag unscheduled developments immediately.**
Flag any genuinely new, unscheduled development the moment it's seen — don't
wait for day-close to log it.

**Rule 12 — Trade impact and Thesis update are conditional.**
- Trade impact: only fill in if today's outcome actually affects a
  pair/direction already on the current week's Trade Priority List (still
  valid / stopped / target hit / invalidated). Reference the existing
  entry/stop/target already on file — never invent a new number here. If no
  trade idea is affected, write "none."
- Thesis update: only fill in if today's outcome materially changes the
  weekly thesis. One sentence, no more. If nothing changed, write "none" —
  don't restate the thesis for the sake of filling the field.

**Rule 13 — Pre-generation sanity check.**
Run once, as the final step, right before generating or closing out each
day's entry — not on every intermediate draft. If anything is logged under
"Anything unscheduled that happened today" that isn't already explained by a
scheduled FF event, web search for a plausible cause tied to the pair(s) and
approximate time window (headline, data revision, central bank chatter,
geopolitical development) before finalizing the entry.
- If a plausible, well-supported cause turns up: state it as the likely
  cause per available reporting, cited, and clearly hedged as probable
  rather than confirmed. This is not a Rule 1 violation — Rule 1 bans
  fabricating an actual/price; attributing a likely cause to a move already
  sourced from a chart is a separate thing and is allowed here as long as
  it's hedged and cited.
- If nothing turns up: leave the line as "no source identified for this
  move" — don't force an attribution just to fill the field.
- Scope check: if the same move shows up across multiple unrelated pairs at
  once, look for a broad/dollar-wide cause rather than assuming it's
  specific to whichever single pair the move was first noticed on.
- This check runs once per day-closing entry, not retroactively rewriting
  earlier days unless something material turns up that changes a Verdict
  already logged.

---

## WEEK-END REVIEW PHASE RULES

**Rule 14 — Re-read before writing.**
Before writing anything: re-read the week's full file and the Position
Ledger top to bottom. Confirm every day marked "Closed" was actually
resolved, and every position's status matches its latest verdict elsewhere
in the documents — the newest call wins.

**Rule 15 — Don't average away a messy week.**
If most entries stopped out around -1R, say that plainly rather than
computing a flattering average.

**Rule 16 — Name the biggest error plainly.**
Name the week's single biggest analytical or process error plainly, in one
sentence — don't bury it in a longer paragraph.

---

## POSITION LEDGER RULES

**Rule 17 — New row timing.**
A new row is added the moment a new idea is published — per Rule 4, the
stop is filled in at the same time as the entry, never after.

**Rule 18 — Same-pair opposite-direction flag.**
If two rows exist for the same pair going opposite directions at once, note
it in "Notes" the day it happens — don't wait to discover it later.

**Rule 19 — Single source of truth.**
This file is the single source of truth for "what's open right now." The
Weekly Outlook does not repeat this — it only links to the Ledger if a new
idea overlaps with something already open here.

---

## KNOWN OPEN ITEMS (not yet resolved by this document)

- **Status vocabulary mismatch:** the Position Ledger uses 4 states (OPEN /
  STOPPED / CLOSED @TP / CLOSED @breakeven). The `/pairs` (now `/live-trades`)
  page's status field uses 7 values (Waiting/Triggered/Active/etc.). These
  have never been reconciled — this document does not resolve that, it only
  records that the conflict exists.
- **Correlation check's database home** is still undecided (`research_cycles`
  vs. `trade_ideas`) — Rule 5 above describes the check itself, not where
  its result is persisted.
- **Whether "Carryover Resolved This Week" is what writes to the Position
  Ledger** is still unanswered (see Rule 7 and the Week-End Review
  structure) — the Ledger is currently updated "directly" per the Week-End
  Review prompt, but the mechanics of that update aren't yet specified in
  code.

---

## MIGRATION NOTE

Prior to this document, each prompt file used its own independent 1–N rule
numbering. The Weekly Outlook prompt's old "Rule 4" for mandatory stops lines
up with this document's Global Rule 4. But the Position Ledger template's
references to "Rule 2" and "Rule 3" for stop-timing and status-from-price-action
do **not** line up with this document — those are now Rule 17 and Rule 3,
respectively. Any file still citing the old per-prompt numbering should be
updated to reference this document's numbers instead, or the reference will
point to the wrong rule.
