# HipMarvin FX — Daily Update Template v4

**v4 — HTF Trend / QMR sync · 2026-08-28**

One entry per week, appended as events resolve. This version adds a
Rules-24–33 check block without changing the existing Verdict / Trade
impact / Thesis update semantics, and without changing the v3 Liquidity /
Flow Regime Check block, which stays exactly as written.

---

## Week __ · Day __ · [Day, Date]

**Status:** Pending / Partial / Closed

- Event(s) today: [name, time WAT]
- Actual: [from FF screenshot] or "no actual yet"
- Price note: [pair, level, time captured]
- Verdict: Pending / Confirmed / Invalidated / Mixed

### Liquidity / Flow Regime Check

*(Required when today's price action materially tests or changes the
execution regime; otherwise write `N/A — no material flow-regime change
today`. Unchanged from v3.)*

- **Pair(s):** [pair(s)]
- **20D Structural Location:** [X% up 20D range — Discount / Equilibrium / Premium]
- **Prior Flow Regime:** [RANGE / TRANSITION / DIRECTIONAL]
- **Current Flow Regime:** [RANGE / TRANSITION / DIRECTIONAL]
- **Regime Change:** [NONE / RANGE → TRANSITION / RANGE → DIRECTIONAL / DIRECTIONAL → TRANSITION / DIRECTIONAL → RANGE / other]
- **Liquidity State:** [Untaken liquidity / Liquidity swept + rejected / Liquidity swept + accepted / Sequential liquidity consumption / Transition / confirmation pending]
- **Liquidity Target / Pool:** [level or area, sourced from chart]
- **Acceptance / Rejection Evidence:** [what price actually did]
- **Zone/Flow Relationship:** [Mean-reversion aligned / Continuation aligned / Location vs flow conflict — continuation justified / Location vs flow conflict — reversal not yet confirmed / Neutral / insufficient evidence]
- **Execution Implication:** [fade / continuation / wait for confirmation / no actionable change]

**Rule 23 discipline:** A 20D Premium or Discount location does not
automatically reverse a directional move. A continuation classification
requires actual displacement plus acceptance/follow-through; a wick or
narrative alone is insufficient.

### HTF Trend / Structural Break Check (new in v4)

*(Rules 24–33. Required when today's price action tests, confirms, or
potentially breaks a Daily/4H structural level relevant to an open or
candidate idea; otherwise write `N/A — no material HTF structural change
today`.)*

- **Pair(s):** [pair(s)]
- **Daily Trend (going into today):** [Bullish / Bearish / Choppy]
- **4H Trend (going into today):** [Bullish / Bearish / Choppy]
- **HTF Structural Level Tested:** [level, sourced from chart]
- **Break Outcome:** [No break / Wick only, not accepted / Decisive break, follow-through pending / Decisive break, follow-through confirmed]
- **Structural Break (per Rule 26):** [None / Confirmed bullish / Confirmed bearish / Not applicable]
- **QMR Phase Observed:** [Quality / Manipulation / Reaction / Confirmed continuation / Confirmed reversal / Not applicable]
- **Pattern vs. Structure Note (Rule 31):** [If a QM/QML-looking pattern
  appeared today, state plainly whether it was accepted (structure agreed)
  or rejected (structure disagreed and the pattern was overridden). A
  textbook pattern that conflicts with unbroken HTF structure is void per
  Rule 31 — record that it was seen and disregarded, not omit it.]
- **Execution Implication:** [With-trend idea strengthened / countertrend
  idea now eligible per confirmed break / countertrend idea still not
  eligible, wick only / no change]

**Rule 26 discipline:** a wick through structure followed by immediate
reclamation is not a confirmed break. Do not upgrade Structural Break to
"Confirmed" on a single candle without follow-through evidence — if
follow-through is still pending, say so and leave the field at "Decisive
break, follow-through pending" rather than rounding up.

**Rule 25 discipline:** none of the following, by themselves, are
sufficient to change Structural Break away from "None": Premium/Discount
location, 20D range extension, a prior high/low being touched, a liquidity
wick, a single rejection candle, or an aesthetically clean QM pattern.

### Macro Regime Check

*(Conditional — use when a major macro release occurred today. If none,
write `N/A — no macro release today`. Unchanged from v3.)*

- **Currency / Macro Area:** [currency]
- **Today's Macro Release:** [release name]
- **Actual:** [actual]
- **Forecast:** [forecast]
- **Previous:** [previous]
- **Inflation vs Target:** [Above / At / Below target / N/A]
- **Inflation Direction:** [↑ / → / ↓ / N/A]
- **Labour-Market Implication:** [Strengthening / Stable / Weakening / N/A]
- **Central-Bank Implication:** [More hawkish / Unchanged / More dovish / Mixed]

**Regime Status**
- **Previous:** [Strong / Constructive / Neutral / Weakening / Weak]
- **Current:** [Strong / Constructive / Neutral / Weakening / Weak]
- **Status:** [STRENGTHENED / UNCHANGED / WEAKENED / INVALIDATED]

**Weekly Thesis Impact**
[Confirm / strengthen / weaken / partially change / invalidate. Do not
create a new weekly thesis merely because today's price reaction is large.]

**FX Impact**
[Which pairs are affected and whether the setup is strengthened, weakened,
reprioritized, or only has timing affected.]

**Macro Regime Confidence:** [Low / Moderate / High] — [why]

### Trade impact

[pair/direction — still valid / stopped / target hit / invalidated /
now eligible (structural break confirmed) / remains gated (no confirmed
break)] or `none`

### Thesis update

[Only if today's outcome materially changes the weekly thesis; otherwise
`none`.]

### Anything unscheduled that happened today

[or `none`]

---

## PRE-GENERATION SANITY CHECK

Run once at day close. If an unscheduled move is logged, search for a
plausible cited cause. If nothing reliable appears, write `no source
identified for this move`. If multiple unrelated pairs move together, test
a broad/dollar-wide/risk/liquidity cause before assigning a single-currency
explanation. Unchanged from v3.

## v4 rule note

The Daily Update records **what price actually did**, on both the Flow
Regime axis (v3, Rule 23) and now the HTF Trend/Structural Break axis (v4,
Rules 24–33). Neither block rewrites the week's original thesis or a
position's Status merely because today's move was large — a confirmed
structural break changes what's *eligible* to be published or acted on
going forward; it does not, by itself, close or flip an already-open
position. That stays governed entirely by Rule 3 and, where relevant, the
Thesis Invalidation Flag (Rule 22).

## Compatibility note

v3 files remain the historical reference for prior weeks. New Daily
Updates should use this v4 schema together with `STANDING_PROTOCOL_v7.md`.
