# HipMarvin FX — Daily Update File

**Changelog:**
- **v2 (2026-08-17):** Added optional `### Macro Regime Check` block, inserted after Verdict and before Trade impact. This block is **conditional** — it is only written when the day includes a major macro data release (CPI, GDP, employment, central bank decision, PCE, etc.). Days with no scheduled macro data release omit the block entirely or write "N/A — no macro release today." The block records the affected currency's regime state (previous → current), classifies the change as STRENGTHENED / UNCHANGED / WEAKENED / INVALIDATED, and states the Weekly Thesis Impact and FX Impact explicitly. It does not replace Verdict, Trade Impact, or Thesis Update — those existing fields are unchanged. Added `Macro Regime Rule` standing note.

---

## Week __ · Day __ · [Day, Date]

One of these per week, added to across the day as events actually resolve.
Feeds `daily_actionable_blocks`.

---

### DAY 1 · [Day, Date]
**Status:** Pending / Partial / Closed

- Event(s) today: [name, time WAT]
- Actual: [from FF screenshot] or "no actual yet"
- Price note: [pair, level, time captured]
- Verdict (only fill in once closing today): Pending / Confirmed / Invalidated / Mixed

*(If today included a major macro data release — CPI, GDP, employment, central bank decision, PCE, or similar — fill in the Macro Regime Check block below before Trade impact. If no major macro release occurred today, write "N/A — no macro release today" on one line and proceed directly to Trade impact.)*

### Macro Regime Check
*(Conditional — only present when a major macro release occurred today. One block per release if multiple releases affected different currency regimes.)*

- **Currency / Macro Area:** [currency]
- **Today's Macro Release:** [release name]
- **Actual:** [actual]
- **Forecast:** [forecast]
- **Previous:** [previous]
- **Inflation vs Target:** [Above / At / Below target / Not applicable]
- **Inflation Direction:** [↑ / → / ↓ / Not applicable]
- **Labour-Market Implication:** [Strengthening / Stable / Weakening / Not applicable]
- **Central-Bank Implication:** [More hawkish / Unchanged / More dovish / Mixed]

**Regime Status**
- **Previous:** [Strong / Constructive / Neutral / Weakening / Weak]
- **Current:** [Strong / Constructive / Neutral / Weakening / Weak]
- **Status:** [STRENGTHENED / UNCHANGED / WEAKENED / INVALIDATED]

**Weekly Thesis Impact**
[State whether today's release: confirms / strengthens / weakens / partially changes / invalidates the existing weekly macro thesis. Do not create a new weekly thesis merely because today's price reaction is large.]

**FX Impact**
[State which currency pairs are affected and whether today's development: strengthens the existing setup / weakens the existing setup / changes the priority / changes only the entry timing / creates no actionable change.]

**Macro Regime Confidence:** [Low / Moderate / High] — [one sentence explaining why; preserve uncertainty where the evidence is mixed or incomplete]

*(End of Macro Regime Check block — continue with Trade impact below)*

- Trade impact (if any): [pair/direction — still valid / stopped / target hit / invalidated] or "none"
- Thesis update (only if today changes it): [one sentence] or "none"
- Anything unscheduled that happened today: [or "none"]

*(repeat this block once per Day 1–5, appended as the week goes on — not written all at once)*

---

> **Macro Regime Rule:** The G10 Macro Regime establishes the fundamental backdrop; it does not create a trade by itself. A single day's release may adjust the regime read — it does not override the weekly thesis, the Scenario Matrix verdict, or the entry/stop levels already on file. Macro Regime affects Setup Quality, not Entry Quality. Entry Quality remains independently determined by chart-sourced price action per Rule 3.

---

## PRE-GENERATION SANITY CHECK (STANDING PROTOCOL)

Run this once, as a final step, right before generating or closing out each day's entry — not on every intermediate draft during the day.

- **Trigger:** any item logged under "Anything unscheduled that happened today" that isn't already explained by a scheduled FF event.
- **Action:** run a web search for a plausible cause tied to the pair(s) and approximate time window (news headline, data revision, central bank chatter, geopolitical development, etc.) before finalizing the entry.
- If a plausible, well-supported cause turns up: state it as the likely cause per available reporting, cited, and clearly hedged as probable rather than confirmed. This is not a Rule 1 violation — Rule 1 bans fabricating an actual/price; attributing a likely cause to a move that's already sourced from a chart is a separate thing and is allowed here as long as it's hedged and cited.
- If nothing turns up after checking: leave the line as "no source identified for this move" — don't force an attribution just to fill the field.
- **Scope check:** if the same move shows up across multiple unrelated pairs at once (e.g. JPY, EUR, AUD, NZD, DXY all moving together), that's a signal to look for a broad/dollar-wide cause rather than assuming it's specific to whichever single pair the move was first noticed on.
- This check runs once per day-closing entry, not retroactively rewriting earlier days unless something material turns up that changes a Verdict already logged.
