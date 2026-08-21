# HipMarvin FX — Standing Protocol v6.1

**v6.1 — Pair Discovery + Liquidity/Flow architecture · 2026-08-21**

This version preserves Rules 1–23 and adds the v6.1 **pair-selection layer** before Rule 23. The methodology now discovers the strongest currency relationship and best pair expression before applying the existing 20D location, liquidity, flow, acceptance/rejection, and execution framework.

## Canonical hierarchy

**Macro Regime → Currency Relative Strength → Pair Discovery (Majors + Liquid Crosses) → Best Expression → 20D Location → Liquidity → Flow Regime → Acceptance/Rejection → Premium/Discount Context → Catalyst → Execution.**

Pair discovery expands the opportunity set; it does not weaken Rule 23. Rule 23 remains the execution/validation gate.

Rules 1–23 retain their existing meaning. Rules 24–28 below are additive v6.1 selection rules.

---

## GLOBAL RULES (1–4)

**Rule 1 — Never fabricate.** Never fabricate numbers, forecasts, actuals, COT figures, prices, closes, results, or resolutions. No source = Pending / not sourced / no actual yet.

**Rule 2 — Sourcing separation.** Forecasts/priors come from the attached FF evidence; speech/testimony from cited web sources; entry/stop/target from charts or clearly labelled technical inference.

**Rule 3 — Status only from actual recorded price action.** Status/verdict/outcome changes require actual recorded price action or a sourced actual.

**Rule 4 — Every trade idea gets a stop.** No exception.

## WEEKLY OUTLOOK RULES (5–7)

**Rule 5 — Correlation check.** Identify correlated bets and Scenario Matrix conflicts before finalizing the Trade Priority List.

**Rule 6 — Daily Game Plan is forward-only.** It does not resolve or verdict completed events.

**Rule 7 — Carryover is noted, not resolved, here.** Resolution belongs in the Position Ledger.

## DAILY UPDATE RULES (8–13)

**Rule 8 — Partial vs Closed discipline.** A day is Closed only when scheduled events have happened and been sourced.

**Rule 9 — Only add what's actually supported.** Do not rewrite unaffected content.

**Rule 10 — Verdict values.** Pending / Confirmed / Invalidated / Mixed.

**Rule 11 — Flag unscheduled developments immediately.**

**Rule 12 — Trade impact and Thesis update are conditional.** Only update them when the day's evidence materially affects an existing setup or thesis.

**Rule 13 — Pre-generation sanity check.** Search for a plausible cited cause when an unscheduled move is observed; do not force an attribution.

## WEEK-END REVIEW RULES (14–16)

**Rule 14 — Re-read before writing.** Re-read the full week and Position Ledger.

**Rule 15 — Don't average away a messy week.** State repeated losses plainly.

**Rule 16 — Name the biggest error plainly.** One sentence.

## POSITION LEDGER RULES (17–19)

**Rule 17 — New row timing.** Add the row when the idea is published; stop is present at the same time.

**Rule 18 — Same-pair opposite-direction flag.** Note simultaneous opposing rows.

**Rule 19 — Single source of truth.** The Ledger is the authoritative record of what is open.

## CROSS-PHASE RULES

**Rule 20 — Parser integrity.** Research files remain parser-visible canonical records. Deliberate, versioned schema changes are permitted; silent structural drift is not.

### Rule 21 — 20D Zone / Structural Location

**Part A — Calculation.** Calculate the most recent 20-trading-day High/Low backward from the most recent Friday. Entry location is:

`zone % = (entry − 20D Low) / (20D High − 20D Low) × 100`

Labels: 0–20% Discount; 20–40% Lower Equilibrium; 40–60% Equilibrium; 60–80% Mild Premium; 80–100% Deep Premium.

**Part B — v6 interpretation.** The 20D zone is a **location descriptor first**. It does **not**, by itself, establish a reversal direction. A Deep Premium location can remain bullish in a directional regime; a Discount location can remain bearish in a directional regime.

The old Premium=Sell / Discount=Buy relationship remains a **mean-reversion tendency**, not a universal directional rule.

### Rule 22 — Thesis invalidation flag (disclosure only)

If new research materially contradicts the analytical basis for an OPEN position, record the contradiction in Notes with a pointer to the research. It does not change Status, stop, or close the position.

### Rule 23 — Liquidity/Flow Regime

Every new Trade Priority List idea must be classified into one of three flow regimes:

**RANGE** — liquidity sweeps are repeatedly rejected; price returns into the established range; displacement lacks persistent follow-through; two-sided trade dominates.

**TRANSITION** — the market is shifting between range and directional behavior; a meaningful liquidity sweep/displacement has occurred but acceptance or continuation is incomplete or conflicting.

**DIRECTIONAL** — liquidity is being consumed sequentially in one direction; displacement has follow-through; broken liquidity/structure is accepted rather than immediately reclaimed; retracements fail to recover the prior level; related instruments provide confirmation where available.

For each idea record a **Liquidity State** using one of:
- `Untaken liquidity`
- `Liquidity swept + rejected`
- `Liquidity swept + accepted`
- `Sequential liquidity consumption`
- `Transition / confirmation pending`

Then record a **Zone/Flow Relationship**:
- `Mean-reversion aligned`
- `Continuation aligned`
- `Location vs flow conflict — continuation justified`
- `Location vs flow conflict — reversal not yet confirmed`
- `Neutral / insufficient evidence`

### Rule 23A — Execution hierarchy

When the 20D location conflicts with directional flow, do **not** automatically fade the move. First determine whether liquidity was swept and rejected or swept and accepted.

**Sweep → rejection → return into range** supports mean reversion.

**Sweep → acceptance → shallow retracement → next liquidity target** supports continuation.

The latter can justify a Buy in Premium or Sell in Discount, provided the directional evidence is explicitly stated and the risk remains valid.

### Rule 23B — Liquidity overrides location only conditionally

Liquidity/Flow does not automatically override the 20D range. It overrides the range's **reversal implication** only when acceptance/continuation evidence is present. A mere wick through a level is not enough.

### Rule 23C — Daily regime transitions

The Daily Update must record a material change such as `RANGE → DIRECTIONAL BULLISH`, `DIRECTIONAL BEARISH → TRANSITION`, or `TRANSITION → RANGE` when the day's actual price action establishes that change. Do not backfill a regime change solely because price moved strongly.

### Rule 23D — Cross-market sanity check

When multiple unrelated pairs exhibit the same directional behavior, test for a broad macro/dollar/risk/liquidity driver before attributing the move to a single currency. Cross-market confirmation strengthens a flow classification but does not substitute for chart evidence.

### Rule 23E — No automatic trade from regime alone

A DIRECTIONAL label does not create an entry. Entry still requires a defined level, stop, risk/reward, timeframe, and valid thesis/context. A RANGE label does not require a fade.

---

## v6.1 PAIR DISCOVERY / BEST EXPRESSION RULES (24–28)

### Rule 24 — Currency-relative pair discovery

The research universe includes major USD pairs and liquid G10 crosses. A pair must **never be excluded solely because it is a cross**.

First rank currencies by macro regime and observable relative strength/weakness. Then construct candidate pair expressions from the strongest-vs-weakest relationships.

When major pairs are compressed, range-bound, or provide poor directional expression, actively test liquid crosses for a cleaner expression of the same currency-relative thesis.

A cross remains a candidate, not an automatic trade. It must survive liquidity, structure, flow, catalyst, execution, and risk checks.

### Rule 25 — Best-expression selection

For each material currency thesis, compare available pair expressions rather than defaulting to a major.

The Best Expression is the pair that most cleanly converts the macro/relative-strength thesis into an executable opportunity after considering:

1. Relative-strength differential
2. Macro alignment
3. Price structure and 20D location
4. Liquidity map and target availability
5. Flow regime
6. Acceptance/rejection evidence
7. Catalyst timing
8. Correlation/concentration
9. Spread/liquidity/execution quality
10. Risk/reward

The best expression may be a major or a cross.

### Rule 26 — Premium/discount is contextual

Premium and Discount remain location descriptors under Rule 21. They are not automatic directional filters.

**Never exclude a bullish expression solely because it is in Premium. Never exclude a bearish expression solely because it is in Discount. Never fade a trend merely because price is stretched within the 20D range.**

A Premium Buy or Discount Sell requires explicit continuation justification under Rule 23, especially liquidity acceptance, displacement/follow-through, and a credible next liquidity objective.

### Rule 27 — Pair Discovery Matrix

Before the Trade Priority List, produce a compact matrix comparing material candidate expressions:

| Currency Thesis | Candidate Pair | Major/Cross | Relative Strength | 20D Location | Liquidity | Flow | Catalyst | Expression Rank | Decision |
|---|---|---|---|---|---|---|---|---:|---|
| [strong vs weak] | [pair] | [Major/Cross] | [High/Med/Low] | [zone] | [quality/target] | [regime] | [event] | [1..n] | [Select / Watch / Reject] |

**Select** = best expression and eligible for Rule 23 evaluation.

**Watch** = interesting relationship but missing confirmation, catalyst, or execution quality.

**Reject** = actual analytical/execution deficiency. Being a cross is not a rejection reason.

### Rule 28 — Handoff to Rule 23

The discovery handoff is:

**Pair discovered → Best Expression selected → 20D location established → liquidity mapped → flow regime classified → acceptance/rejection assessed → catalyst checked → Rule 23 execution gate applied.**

Rule 23 remains intact and governs whether the selected expression is tradable. v6.1 changes the **selection universe and order of analysis**, not the Rule 23 validation standard.

---

## REQUIRED WEEKLY TRADE FIELDS IN v6.1

Each Priority List idea carries:

- Entry
- Stop
- TP1
- TP2
- R:R
- Timeframe
- Correlation Class
- Daily Zone
- Tier
- Macro Regime
- Macro Alignment
- **Candidate Universe:** Major / Liquid Cross
- **Pair Discovery Thesis:** [strong currency vs weak currency]
- **Best Expression:** [pair + concise reason]
- **Liquidity/Flow Regime**
- **Liquidity State**
- **Zone/Flow Relationship**
- Reasoning

### Reasoning requirement

If a Buy is in Premium or a Sell is in Discount, state the location explicitly, then explain whether flow is continuation aligned or whether reversal evidence exists. Do not describe the zone itself as proof of reversal.

---

## Core principle

> **Find the strongest currency relationship first; choose the cleanest pair expression second; then let location, liquidity, flow, acceptance/rejection, catalyst, and Rule 23 determine whether the expression is tradable.**
