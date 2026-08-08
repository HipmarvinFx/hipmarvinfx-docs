# HipMarvin FX — Weekly Research File

## Week __ · W/C __ 2026 · [Event Label]

Only what this week's HTML needs. No rule bookkeeping, no carryover audit — that lives in POSITION_LEDGER.md.

---

## Price Strip (as of [day/date])

| Pair    | Level | Bias |
| ------- | ----- | ---- |
| DXY     |       |      |
| EUR/USD |       |      |
| GBP/USD |       |      |
| USD/JPY |       |      |
| XAU/USD |       |      |
| AUD/USD |       |      |
| NZD/USD |       |      |
| USD/CAD |       |      |

## Weekly Thesis

[2-4 sentences: what's the week's core question, and what's the one-line answer per pair]

## Macro Drivers (up to 4)

**Driver 1**

- Headline / Time (WAT)
- Forecast · Prior
- Analysis: [what confirms vs breaks the thesis]

## Trade Priority List

**Priority 1 — [Pair] [Direction] — ★★★★★**

- Entry / TP1 / TP2 / Stop / R:R
- Correlation Flagged: [Yes/No — per Rule 5's correlation check below: does this specific idea sit inside the flagged, crowded-basket group, or is it independent of it?]
- Reasoning: [1-2 sentences, why this ranks here]

*(repeat per idea, ranked by conviction — for conditional/dual-trigger ideas, state Correlation Flagged separately per side, since a long trigger and short trigger on the same pair can carry different correlation exposure)*

**Correlation check (Rule 5):** [state plainly if 2+ ideas are really one bet, and whether the list leans against the scenario matrix's own base case — this is the narrative explanation; the per-idea Yes/No fields above are the parser-readable summary of the same judgment, not a separate decision]

## Scenario Matrix — [Event Name, exact FF wording]

- Branch 1 (X% probability): [outcome] → [what it does to which pairs] → [action]
- Branch 2 (Y% probability): [outcome] → [action]
(probabilities sum to 100%)

*(repeat one matrix per red-folder event)*

## COT Positioning

| Pair | Net Position | Direction | Read |
| ---- | ------------ | --------- | ---- |

## Daily Game Plan

- **Monday:** [what to set/watch, max open positions if relevant]
- **Tuesday:** [pre-event discipline, e.g. no new trades N minutes pre-release]
- **Wednesday:** [size reduction windows, re-entry conditions]
- **Thursday:** [reassessment triggers, profit-booking guidance]
- **Friday:** [risk management only, or note if a red-folder event still applies]

*(one line per weekday — this is forward guidance only; actual day-by-day resolution lives in the Daily Update file, don't duplicate that content here)*

---

**Sources:** [FF screenshot date/time · web search sources for speeches/panels · chart timestamps]

---

## Template Changelog

**[today's date]:** Added `Correlation Flagged: Yes/No` as a required per-idea field inside each Trade Priority List entry. Previously, Rule 5's correlation check existed only as a single free-text paragraph covering the whole list at once — this made it impossible for the Admin CMS importer to reliably determine which specific pair(s) a correlation warning applied to, especially for conditional/dual-trigger ideas where only one side (e.g. a long trigger, not the short) is actually part of the flagged basket. The free-text Correlation check paragraph is unchanged and still required — it remains the narrative explanation for readers. The new per-idea field is the structured, parser-safe version of the same judgment, following the same precedent already set by Tier, Correlation Class, and Daily Zone.
