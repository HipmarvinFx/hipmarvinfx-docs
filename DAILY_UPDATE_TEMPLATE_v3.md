# HipMarvin FX — Daily Update Template v3

**v3 — Liquidity/Flow Regime sync · 2026-08-19**

One entry per week, appended as events resolve. This version adds the Rule 23 fields without changing the existing Verdict / Trade impact / Thesis update semantics.

---

## Week __ · Day __ · [Day, Date]

**Status:** Pending / Partial / Closed

- Event(s) today: [name, time WAT]
- Actual: [from FF screenshot] or "no actual yet"
- Price note: [pair, level, time captured]
- Verdict: Pending / Confirmed / Invalidated / Mixed

### Liquidity / Flow Regime Check

*(Required when today's price action materially tests or changes the execution regime; otherwise write `N/A — no material flow-regime change today`.)*

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

**Rule 23 discipline:** A 20D Premium or Discount location does not automatically reverse a directional move. A continuation classification requires actual displacement plus acceptance/follow-through; a wick or narrative alone is insufficient.

### Macro Regime Check

*(Conditional — use when a major macro release occurred today. If none, write `N/A — no macro release today`.)*

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
[Confirm / strengthen / weaken / partially change / invalidate. Do not create a new weekly thesis merely because today's price reaction is large.]

**FX Impact**
[Which pairs are affected and whether the setup is strengthened, weakened, reprioritized, or only has timing affected.]

**Macro Regime Confidence:** [Low / Moderate / High] — [why]

### Trade impact

[pair/direction — still valid / stopped / target hit / invalidated] or `none`

### Thesis update

[Only if today's outcome materially changes the weekly thesis; otherwise `none`.]

### Anything unscheduled that happened today

[or `none`]

---

## PRE-GENERATION SANITY CHECK

Run once at day close. If an unscheduled move is logged, search for a plausible cited cause. If nothing reliable appears, write `no source identified for this move`. If multiple unrelated pairs move together, test a broad/dollar-wide/risk/liquidity cause before assigning a single-currency explanation.

## v3 rule note

The Daily Update records **what price actually did**. It may change the observed Flow Regime, but it does not rewrite the week's original thesis merely because price moved. A regime change is an evidence record, not an automatic trade instruction.
