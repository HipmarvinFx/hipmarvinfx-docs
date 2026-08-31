# HipMarvinFX v7 — QMR / Higher-Timeframe Master Implementation Brief

**Status:** APPROVED / CANONICAL TEAM HANDOFF
**Date:** 31 August 2026
**Purpose:** Persistent roadmap and shift-to-shift implementation authority

> **READ THIS FIRST BEFORE STARTING ANY V7 QMR/HTF WORK.**
>
> This document exists so that development does not drift when a session expires, a developer changes, or a new AI/developer session begins.

---

# 1. NON-NEGOTIABLE GOVERNING PRINCIPLE

> **Trade with the prevailing higher-timeframe trend by default. Countertrend trades are permitted only after a confirmed higher-timeframe structural break. Patterns never override structure.**

QMR/QM is not a standalone reversal strategy.

The intended HipMarvinFX model is:

```text
MACRO REGIME
    ↓
CURRENCY RELATIVE STRENGTH
    ↓
PAIR DISCOVERY
    ↓
BEST EXPRESSION
    ↓
DAILY HTF STRUCTURE
    ↓
4H ACTIONABLE TREND
    ↓
20D STRUCTURAL LOCATION
    ↓
LIQUIDITY MAP
    ↓
QMR
    ↓
REACTION / STRUCTURAL CONFIRMATION
    ↓
QM/QML ENTRY REFINEMENT
    ↓
LIQUIDITY TARGET
    ↓
RISK / EXECUTION
```

Do not allow a lower-level pattern or signal to override a higher-level structural decision.

---

# 2. TIMEFRAME POLICY

HipMarvinFX v7 is a higher-timeframe framework.

## Permitted

- **Daily:** broad structural regime and major swings
- **4H:** primary actionable trend and structure
- **1H:** minimum execution/setup confirmation timeframe

## Explicitly excluded

- 30M
- 15M
- 5M
- 1M
- Any lower-timeframe execution logic

**No directional or execution decision may be generated from a timeframe below 1H.**

---

# 3. HTF TREND FIRST

The system must determine:

```text
Daily Trend = BULLISH / BEARISH / TRANSITION / CONFLICT
4H Trend    = BULLISH / BEARISH / TRANSITION / CONFLICT
```

Meaningful swing structure must be used rather than a single candle.

The engine must identify:

- Higher Highs
- Higher Lows
- Lower Highs
- Lower Lows
- Protected swing highs
- Protected swing lows
- Structural continuation
- Structural failure

---

# 4. TREND ALIGNMENT

### Daily bullish + 4H bullish

Prefer longs.

### Daily bearish + 4H bearish

Prefer shorts.

### Daily/4H conflict

Do not manufacture a direction.

Classify as:

```text
TRANSITION / CONFLICT
```

and require stronger confirmation.

---

# 5. CONTINUATION IS THE DEFAULT

If HTF structure is bullish, the default direction is LONG.

If HTF structure is bearish, the default direction is SHORT.

The following do NOT independently reverse the bias:

- Premium
- Discount
- 20D range extension
- Previous high/low
- Liquidity wick/sweep
- Single rejection candle
- Apparent overextension
- QM/QML pattern alone

---

# 6. 20D RANGE IS LOCATION, NOT DIRECTION

Retain the existing 20D location engine.

```text
Premium ≠ automatic SHORT
Discount ≠ automatic LONG
```

The system must be capable of identifying valid continuation through Premium or Discount when HTF structure and liquidity/flow support it.

This preserves the previously identified GBP/USD trend-extension lesson.

---

# 7. LIQUIDITY IS NOT AN AUTOMATIC REVERSAL SIGNAL

The system must distinguish between:

```text
LIQUIDITY SWEPT + REJECTED
LIQUIDITY SWEPT + ACCEPTED
SEQUENTIAL LIQUIDITY CONSUMPTION
```

A sweep is evidence requiring reaction/confirmation. It is not by itself a sell or buy signal.

---

# 8. QMR DEFINITION

QMR = **Quality → Manipulation → Reaction**

QMR is the setup framework, not a standalone reversal engine.

## Q — QUALITY

Evaluate:

- Macro context
- HTF trend
- Daily/4H structure
- 20D location
- Major liquidity
- Structural levels
- Relevant price zone
- Expected liquidity destination

## M — MANIPULATION

Identify the liquidity interaction/pullback/event that creates the opportunity.

A sweep does not automatically mean reversal.

## R — REACTION

Require actual evidence such as:

- Displacement
- Structural recovery
- Structural break
- Acceptance
- Rejection
- Follow-through
- Failed continuation

A wick alone is insufficient.

---

# 9. QMR CONTINUATION IS THE PRIMARY USE

Preferred continuation sequence:

```text
4H BULLISH
    ↓
Pullback
    ↓
Sell-side liquidity interaction
    ↓
Bullish reaction
    ↓
Displacement
    ↓
Retracement
    ↓
LONG
    ↓
Next liquidity target
```

Bearish continuation follows the inverse logic.

---

# 10. COUNTERTREND HARD GATE

Countertrend trading is NOT permitted merely because:

- price is in Premium/Discount
- price is extended
- liquidity was swept
- a QM pattern appeared
- a previous high/low was touched
- a rejection candle appeared

A countertrend setup becomes eligible only after a **meaningful HTF structural break**.

---

# 11. STRUCTURAL BREAK REQUIREMENTS

A valid countertrend structural break requires:

1. A meaningful protected HTF swing is identified.
2. Price decisively breaks that structure.
3. The break demonstrates displacement/acceptance rather than a wick.
4. Follow-through or structural confirmation exists.
5. QMR then confirms the new direction through reaction/retest.

A wick through structure followed by immediate reclamation is NOT a confirmed reversal.

Therefore:

```text
NO STRUCTURAL BREAK
        ↓
NO COUNTERTREND TRADE
```

---

# 12. QM / QML ROLE

QM/QML is an **entry refinement**, not a bias engine.

Never implement:

```text
QM detected → SELL
```

Instead:

```text
HTF TREND
    ↓
VALID QMR
    ↓
REACTION
    ↓
STRUCTURAL CONFIRMATION
    ↓
QM/QML PRESENT?
    ↓
YES → refine entry
NO  → use valid structural entry logic
```

Do not require a perfectly symmetrical textbook Quasimodo pattern.

The underlying structural sequence is more important than pattern symmetry.

---

# 13. PATTERN HIERARCHY

The mandatory hierarchy is:

```text
MACRO
  ↓
HTF STRUCTURE
  ↓
LIQUIDITY / FLOW
  ↓
QMR
  ↓
REACTION
  ↓
QM/QML
```

Patterns never override structure.

---

# 14. 20D EXTENSION / TREND EXTENSION

Do not fade a market merely because it has moved beyond its normal 20D range.

Ask:

- Is HTF structure still intact?
- Is liquidity still being accepted?
- Is directional flow continuing?
- Has protected structure actually broken?

If structure and flow remain directional, continuation remains eligible.

If meaningful structure breaks, reassess for transition/reversal.

---

# 15. NO-TRADE CONDITIONS

Return **WAIT / NO TRADE** when:

- Daily/4H structure is unclear
- Daily and 4H materially conflict
- Liquidity state is ambiguous
- Reaction is incomplete
- Structural break is unconfirmed
- QMR sequence is incomplete
- Required price evidence is unavailable
- Entry/stop/target cannot be supported deterministically

Never manufacture a trade merely to fill a priority list.

---

# 16. REQUIRED TRADE FIELDS

Every v7 trade idea must ultimately expose:

- Pair
- Direction
- Entry
- Stop
- TP1
- TP2
- R:R
- Execution TF
- Daily Zone
- Tier
- Macro Regime
- Macro Alignment
- HTF Trend
- Trend Alignment
- Structural Break
- QMR Phase
- QM/QML Refinement
- Liquidity/Flow Regime
- Liquidity State
- Liquidity Target
- Zone/Flow Relationship
- Reasoning

---

# 17. REQUIRED ENUMS

## HTF Trend

```text
BULLISH
BEARISH
TRANSITION
CONFLICT
```

## Trend Alignment

```text
WITH-TREND
COUNTERTREND-AFTER-STRUCTURAL-BREAK
NOT-ELIGIBLE
```

## Structural Break

```text
NONE
CONFIRMED-BULLISH
CONFIRMED-BEARISH
UNCONFIRMED
```

## QMR Phase

```text
QUALITY
MANIPULATION
REACTION
CONFIRMED-CONTINUATION
CONFIRMED-REVERSAL
```

## QM/QML

```text
PRESENT
NOT-PRESENT
NOT-APPLICABLE
```

---

# 18. AI BOUNDARY

The AI must not invent:

- prices
- HTF trend
- structural breaks
- liquidity states
- QM patterns
- entries
- stops
- targets
- R:R

where deterministic evidence is required or available.

The deterministic engines calculate factual/derived values first.

The AI interprets, explains, ranks and summarizes those values.

**AI is not the source of truth for market data or structural calculations.**

---

# 19. EXISTING ENGINE INTEGRATION

Do not duplicate existing engines unnecessarily.

Reuse and integrate the existing:

- currency strength
- pair discovery
- best expression
- 20D location
- liquidity
- flow regime
- evidence infrastructure
- parser firewall

The v7 QMR/HTF work is an additional structural decision layer, not a replacement for these systems.

---

# 20. IMPLEMENTATION ORDER

Follow this order unless an explicitly approved architectural change supersedes it:

```text
1. Reliable market-data pipeline
        ↓
2. Daily / 4H / 1H OHLC
        ↓
3. HTF structure engine
        ↓
4. Trend alignment engine
        ↓
5. Protected swing / structural-break engine
        ↓
6. Integrate 20D + liquidity + flow
        ↓
7. QMR engine
        ↓
8. QM/QML entry refinement
        ↓
9. Structured Evidence Packet
        ↓
10. AI prompt integration
        ↓
11. Parser v7 fields
        ↓
12. Weekly/Daily/Ledger synchronization
        ↓
13. Test suite
        ↓
14. End-to-end publication test
```

Do NOT jump directly to QM pattern detection before HTF structure and trend gating are operational.

---

# 21. TEST CASES REQUIRED

At minimum test:

1. Bullish HTF continuation → LONG eligible.
2. Bearish HTF continuation → SHORT eligible.
3. Bullish HTF + Premium + accepted flow → continuation remains eligible.
4. Bearish HTF + Discount + accepted flow → continuation remains eligible.
5. Bullish HTF + liquidity sweep without structural break → NO countertrend short.
6. Bullish HTF + confirmed protected-low break + acceptance + bearish QMR → countertrend short eligible.
7. Bullish HTF + bearish QM without structural break → reject countertrend signal.
8. Daily/4H conflict → TRANSITION/CONFLICT, no manufactured direction.
9. Any sub-1H signal → rejected.
10. 20D range extension while HTF flow remains directional → do not automatically fade.

---

# 22. DOCUMENTATION / VERSIONING RULE

Do not silently rewrite historical v5/v6 methodology or historical research.

v7 changes must be versioned and explicit.

The following documents are part of the v7 synchronization path:

- `STANDING_PROTOCOL_v7.md`
- `QMR_QM_HTF_V7_MIGRATION.md`
- `WEEKLY_RESEARCH_TEMPLATE_v5.md`
- `DAILY_UPDATE_TEMPLATE_v4.md`
- `POSITION_LEDGER_TEMPLATE_v3.md`
- `HipMarvinFX_Generation_Prompts_v7_0.md`

If any of these disagree with this master brief, stop and resolve the documentation conflict before implementing behavior.

---

# 23. SESSION / SHIFT CONTINUITY RULE

At the start of every development session or team shift:

1. Read this file first.
2. Read `STANDING_PROTOCOL_v7.md`.
3. Check the current GitHub implementation state before coding.
4. Identify the last completed implementation step.
5. Continue from the roadmap rather than redesigning the methodology.
6. Do not introduce new strategy rules without explicit approval.
7. Record any architectural deviation in the repository.

At the end of every shift:

1. State what was actually implemented.
2. State what remains incomplete.
3. Record tests run and their result.
4. Record any blockers.
5. Identify the exact next implementation step.
6. Commit documentation/code changes with a clear commit message.

A new session must be able to continue from the repository without relying on chat history.

---

# 24. DEFINITION OF DONE

The v7 QMR/HTF implementation is NOT complete merely because documentation exists.

It is complete only when real market data can flow through:

```text
REAL PRICE DATA
      ↓
DAILY STRUCTURE
      ↓
4H STRUCTURE
      ↓
HTF TREND
      ↓
TREND ALIGNMENT
      ↓
20D LOCATION
      ↓
LIQUIDITY
      ↓
FLOW
      ↓
QMR
      ↓
STRUCTURAL CONFIRMATION
      ↓
QM/QML REFINEMENT
      ↓
VALID ENTRY / STOP / TARGET
      ↓
PARSER VALIDATION
      ↓
PUBLICATION
```

with deterministic evidence available at every required stage.

---

# 25. FINAL LOCKED PRINCIPLE

> **Trend first. Liquidity second. Structure confirms. QMR organizes the setup. QM/QML refines the entry. Patterns never override structure.**

And:

> **Do not predict the reversal. Follow the prevailing trend until the market proves that the trend has changed.**

**This is the persistent v7 roadmap. Future developers and AI sessions must use this document as the continuity anchor unless an explicitly approved version supersedes it.**
