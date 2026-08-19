# HipMarvin FX — Standing Protocol v6

**v6 — Liquidity/Flow Regime upgrade · 2026-08-19**

This version preserves Rules 1–22 and adds **Rule 23 — Liquidity/Flow Regime**. It formalizes a methodological distinction established in v6 review: the 20-day range is a **location map**, not a standalone directional signal. Liquidity and price-flow behavior determine whether that location should be traded as mean reversion, transition, or directional continuation.

## Canonical hierarchy

**Macro Regime → Weekly Thesis → 20D Structural Location → Liquidity Map → Liquidity/Flow Regime → Displacement + Acceptance/Rejection → Entry.**

The 20-day range answers **where price is**. Liquidity analysis answers **where price is likely reaching**. Flow/acceptance answers **whether that destination is being pursued or rejected**. Macro remains the broader contextual filter.

Rules 1–22 retain their existing meaning unless Rule 23 explicitly changes the interpretation of the 20-day zone. Rule 20 permits this as deliberate, versioned schema evolution.

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

## REQUIRED WEEKLY TRADE FIELDS IN v6

Each Priority List idea now carries:

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
- **Liquidity/Flow Regime**
- **Liquidity State**
- **Zone/Flow Relationship**
- Reasoning

### Reasoning requirement

If a Buy is in Premium or a Sell is in Discount, state the location explicitly, then explain whether the flow regime is **continuation aligned** or whether reversal evidence exists. Do not describe the zone itself as proof of reversal.

---

## Migration note

v6 intentionally does not renumber Rules 1–22. Rule 23 is appended as the next cross-phase rule, following the established v3→v4 and v4→v5 evolution pattern. Existing research files remain historical records. New weekly/daily files should use the v6 fields. A parser implementation must treat the v6 fields as additive schema fields and must not reinterpret historical zone flags retroactively.

**Core principle:**

> **The 20-day range establishes the battlefield; liquidity flow determines the direction of engagement.**
