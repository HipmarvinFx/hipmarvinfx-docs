# HipMarvin FX — Weekly Research Template v5

**v5 — HTF Trend / QMR schema · 2026-08-28**

This is the v7-compatible weekly research schema. It preserves the full v4
architecture (Macro Regime, Weekly Thesis, Liquidity/Flow Regime fields from
Rule 23) and adds the Rules 24–35 fields required to state, for every idea,
whether it is trend-following or countertrend-after-structural-break, and
where it sits in the QMR sequence.

> **Core rule (Rule 25):** Trend continuation is the default. A countertrend
> idea is not eligible for this list unless Rule 26 (confirmed HTF
> structural break) is satisfied — this is a hard gate, evaluated before an
> idea is written up, not a disclosure added afterward.

> **Unchanged from v4:** the 20-day range is still location, not direction
> (Rule 21). Liquidity/Flow Regime, Liquidity State, and Zone/Flow
> Relationship (Rule 23) are still required and still copied verbatim to the
> Position Ledger. Nothing below removes or renames a v4 field.

---

## RESEARCH CYCLE

- **Week Label:** [Week __ · Month YYYY]
- **Week Start ISO:** [YYYY-MM-DD]
- **Week End ISO:** [YYYY-MM-DD]
- **Event Label:** [event label]
- **Impact Level:** [High / Medium / Low]
- **Thematic Focus:** [theme]
- **Overall Bias:** [bias]
- **Macro Thesis:** [one concise thesis]
- **Analyst:** Elijah Agom / MarvinX
- **Published:** [date]
- **Status:** [Draft / Published / Closed]
- **Import Status Note:** [completeness / missing inputs]

## PRICE STRIP

[Weekly price strip using sourced/chart values only.]

## G10 MACRO REGIME

### Macro Regime Snapshot

| Currency | Inflation vs Target | Inflation Trend | Labour Trend | Policy Pressure | Macro Regime |
|---|---|---|---|---|---|
| USD | | | | | |
| EUR | | | | | |
| GBP | | | | | |
| JPY | | | | | |
| CHF | | | | | |
| CAD | | | | | |
| AUD | | | | | |
| NZD | | | | | |
| NOK | | | | | |
| SEK | | | | | |

### Macro Regime Ranking

- Strongest → [currencies]
- Constructive / Improving → [currencies]
- Neutral / Transitional → [currencies]
- Weakening → [currencies]
- Weakest → [currencies]

### Institutional Macro Read

[3–5 sentences.]

### FX Implication

[Structural currency preference; this section does not create a trade by itself.]

### Macro Regime Changes From Previous Week

- [Currency]: [Previous] → [Current] — [reason]

## HIGHER-TIMEFRAME TREND MAP (new in v5)

*(Rule 24. Complete once per pair with an active or candidate idea this
week — not required for every G10 pair if no idea touches it.)*

| Pair | Daily Trend | 4H Trend | Trend Agreement | Classification |
|---|---|---|---|---|
| [PAIR] | [Bullish/Bearish/Choppy] | [Bullish/Bearish/Choppy] | [Agree/Conflict] | [Directional / Transition-Conflict] |

**Rule 24 discipline:** if Daily and 4H disagree, the pair is classified
Transition/Conflict, not forced into a direction from a lower timeframe.
No directional or execution decision may be sourced from below 1H.

## WEEKLY THESIS

### Core Macro Question

[One sentence.]

### Institutional Answer

[2–4 sentences.]

### Pair-Level Translation

- **USD:** [bias + reason]
- **EUR:** [bias + reason]
- **GBP:** [bias + reason]
- **JPY:** [bias + reason]
- **CHF:** [bias + reason]
- **CAD:** [bias + reason]
- **AUD:** [bias + reason]
- **NZD:** [bias + reason]

## MACRO DRIVERS

**Driver N**
- **Tag:** [RELEASED / SCHEDULED]
- **Headline:** [event]
- **Subline:** [date/time WAT — actual/forecast/prior]
- **Analysis:**
  - Macro regime being tested: [currency / regime]
  - Confirms regime if: [condition]
  - Strengthens regime if: [condition]
  - Weakens regime if: [condition]
  - Breaks / invalidates regime if: [condition]
  - FX implication: [pairs / direction / consequence]

## TRADE PRIORITY LIST

Each standard idea uses this exact field order. Fields marked **(v5)** are
new — do not omit them; do not silently rename an existing field to
accommodate them (Rule 20).

**Priority N — Pair Direction — ★★★★★**

- **Entry:** [level / zone]
- **Stop:** [level]
- **TP1:** [level]
- **TP2:** [level]
- **R:R:** [ratio]
- **Timeframe:** [chart timeframe used for the idea's general context]
- **Execution TF (v5):** [1H / 4H / Daily — per Rule 24, the timeframe the
  entry itself is actually confirmed on; never below 1H]
- **Correlation Class:** [USD-quote group / USD-base group / Proxy-standalone / Other]
- **Daily Zone:** [X% up the 20D range — label]
- **Tier:** [1 / 2]
- **Macro Regime:** [currency regime / currency regime]
- **Macro Alignment:** [Aligned / Mixed / Contrarian]
- **HTF Trend (v5):** [Bullish / Bearish / Transition / Conflict]
- **Trend Alignment (v5):** [With-trend / Countertrend after structural break / Not eligible]
- **Structural Break (v5):** [None / Confirmed bullish / Confirmed bearish / Not applicable]
- **QMR Phase (v5):** [Quality / Manipulation / Reaction / Confirmed continuation / Confirmed reversal]
- **QM/QML Refinement (v5):** [Yes / No / Not applicable]
- **Liquidity/Flow Regime:** [RANGE / TRANSITION / DIRECTIONAL]
- **Liquidity State:** [Untaken liquidity / Liquidity swept + rejected / Liquidity swept + accepted / Sequential liquidity consumption / Transition / confirmation pending]
- **Liquidity Target (v5):** [level or pool the idea is actually targeting — sourced from chart]
- **Zone/Flow Relationship:** [Mean-reversion aligned / Continuation aligned / Location vs flow conflict — continuation justified / Location vs flow conflict — reversal not yet confirmed / Neutral / insufficient evidence]
- **Reasoning:** [Use the full v7 sequence below. If Trend Alignment is
  "Countertrend after structural break," the Reasoning must cite the
  specific broken HTF level and the follow-through evidence per Rule 26 —
  this is not optional disclosure, it's the eligibility evidence itself.]

**Reasoning sequence (Rule 27's canonical hierarchy, restated for this
template):**

`Macro Regime → Weekly Thesis → HTF Trend/Structure → 20D Structural Location → Liquidity Map → QMR → Displacement/Acceptance or Rejection → QM/QML Entry Refinement → Liquidity Target → Risk`

If any step conflicts with the one before it (e.g. HTF trend says bullish
but Liquidity/Flow Regime reads RANGE), say so plainly rather than picking
the more convenient reading.

### Conditional / Dual-Trigger Format

**Priority N — Pair conditional, both sides — ★★★☆☆**

- **Long trigger / TP / Stop:** close above [price] → target [price] → stop [price]
- **Short trigger / TP / Stop:** close below [price] → target [price] → stop [price]
- **Timeframe / Execution TF / Correlation Class / Daily Zone / Tier:** [fields]
- **Macro Regime (long side):** [regimes]
- **Macro Regime (short side):** [regimes]
- **Macro Alignment:** [per side]
- **HTF Trend / Trend Alignment / Structural Break (per side):** [state each
  side separately — a conditional idea may have one side eligible
  (with-trend) and the other side gated (countertrend, no confirmed break)]
- **QMR Phase (per side):** [state]
- **QM/QML Refinement (per side):** [state]
- **Liquidity/Flow Regime:** [RANGE / TRANSITION / DIRECTIONAL]
- **Liquidity State:** [state]
- **Liquidity Target:** [level/pool]
- **Zone/Flow Relationship:** [relationship]
- **Reasoning:** [what confirmation would convert each conditional side into
  an eligible trade — for the countertrend side, this must include what a
  qualifying structural break would look like]

### HTF Trend & Structural Break Check (new in v5)

*(Rules 24–33. Complete once per idea on the Trade Priority List.)*

- Confirm Daily/4H trend agreement per the Higher-Timeframe Trend Map above.
- Any idea marked **Countertrend after structural break** must show: (1) the
  specific protected HTF swing/level that broke, (2) evidence of decisive
  displacement/acceptance rather than a wick, (3) follow-through evidence,
  (4) a QMR reaction or retest supporting the new direction. Missing any of
  the four means the idea is not eligible — hold it, don't publish it with
  a caveat.
- A textbook QM/QML pattern that conflicts with unbroken HTF structure is
  **rejected outright** (Rule 31) — note that it was seen and rejected,
  rather than omitting it silently, so the record shows the discipline was
  applied.
- If most eligible ideas (after any countertrend rejections) still express
  one correlated directional bet, the existing Rule 5 Correlation Check
  still applies — state it plainly, same as before.

### Zone + Liquidity Alignment Check (unchanged from v4)

- 20D range is a **structural location map**, not an automatic reversal
  signal (Rule 21).
- Any Premium buy / Discount sell must be explicitly explained.
- Identify whether the relevant liquidity was **rejected or accepted**.
- A directional continuation classification requires actual
  displacement/acceptance evidence, not merely a wick or macro narrative.

## SCENARIO MATRIX

[Existing scenario structure — parser header contract unchanged from v6:
`## SCENARIO MATRIX — <exact FF event name>`, disambiguating words in the
header text itself, not a trailing parenthetical.]

| Scenario | Probability | Macro Trigger | Liquidity Target | Confirmation | Invalidation | Trade Implication |
|---|---:|---|---|---|---|---|
| A | | | | | | |
| B | | | | | | |
| C | | | | | | |

## DAILY GAME PLAN

Forward-only. For each day, state:

- Watch window / catalyst
- Liquidity pool to monitor
- Expected flow regime
- HTF structure to watch for a possible break (v5 — only if a countertrend
  candidate exists this week)
- Confirmation required
- Reassessment trigger
- Discipline / no-trade condition

## COT POSITIONING

[Existing COT structure; sourced figures only.]

## RESEARCH CYCLE / CARRYOVER

[Existing carryover fields.]

## POSITION LEDGER LINK

When a new idea is published, Rule 17 requires the corresponding Ledger row
to be created at the same time. Carry over **Daily Zone, Liquidity/Flow
Regime, Liquidity State, Zone/Flow Relationship, HTF Trend, Trend
Alignment, Structural Break, QMR Phase, QM/QML Refinement, and Execution
TF** exactly as written — do not re-derive any of them downstream.

---

## v5 interpretation note

v4's Zone/Flow framework (location vs. flow) and v5's Trend/Structure
framework (direction vs. eligibility) are independent checks on the same
idea, evaluated in this order: **Rule 24–26 first** (is the idea eligible
at all — trend-following, or countertrend with a confirmed break), **then**
Rule 21's zone check and Rule 23's flow-regime check on whatever survives.
An idea can fail the first gate and never reach the second. An idea that
passes the first gate can still carry a zone or flow-regime caveat — those
remain soft flags, disclosed in Reasoning, not blockers.

## Compatibility note

v4 files remain the historical reference for prior weeks. New research
should use this v5 schema together with `STANDING_PROTOCOL_v7.md`.
