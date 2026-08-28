# HipMarvinFX Macro Engine Specification v1

**Status:** Canonical v7 foundation contract  
**Implementation status:** Contract only  
**Date:** 28 August 2026

## 1. Purpose

The Macro Engine converts validated macro FACT evidence into deterministic DERIVED outputs used by pair discovery, weekly research, and the Evidence Packet.

It is a calculation engine, not an AI analyst.

> **Verified facts in → reproducible macro state out.**

## 2. Position in architecture

```text
Approved Macro Sources
        ↓
Source Adapters
        ↓
Evidence Validation
        ↓
Evidence Store
        ↓
Macro Engine
        ↓
DERIVED Macro Outputs
        ↓
Evidence Packet
        ↓
AI Interpretation
```

## 3. Inputs

The engine may consume validated evidence such as:

```text
inflation
employment
growth
retail/activity
trade/current account
policy rates
central-bank decisions/statements
market-implied policy information where explicitly supported
COT positioning
scheduled economic events
verified macro surprises
```

The exact source/provider is determined by the Source Registry, not this specification.

## 4. Input requirements

Only evidence with an acceptable validation status may be used for deterministic calculations.

Default eligible status:

```text
VERIFIED
```

`PENDING`, `STALE`, `INVALID`, and `NOT_AVAILABLE` evidence MUST NOT silently become inputs to a current-state calculation.

If required inputs are missing, the engine MUST return an explicit incomplete/degraded result rather than inventing a value.

## 5. Core outputs

The v1 Macro Engine contract supports:

```text
Macro Regime
Currency Relative Strength
Macro Driver Ranking
Event Impact
COT Positioning
Policy Direction
Macro Surprise
Pair Candidate Ranking
```

These are DERIVED outputs and must be distinguishable from source-reported FACTS.

## 6. Macro Regime

The engine MUST classify the current macro state using deterministic rules and documented inputs.

The classification vocabulary MUST be versioned and stable.

Example conceptual states may include:

```text
BULLISH
BEARISH
NEUTRAL
TRANSITION
INSUFFICIENT_DATA
```

The exact scoring thresholds must be defined in implementation rules before production activation. They must not be invented dynamically by AI.

## 7. Currency relative strength

Currency strength MUST be derived from validated inputs using a reproducible calculation.

The result SHOULD provide:

```text
currency
rank
score
direction
confidence/data quality
input evidence references
```

Relative strength is an analytical input, not a direct market-price claim.

## 8. Macro driver ranking

The engine MUST distinguish current drivers from background information.

Each driver SHOULD include:

```text
currency
factor
direction
magnitude/score where defined
recency
supporting evidence IDs
status
```

The ranking MUST be deterministic for identical inputs and configuration.

## 9. Macro surprise

Where actual, forecast, and previous values exist and are comparable, the engine may calculate a surprise measure.

The engine MUST preserve:

```text
actual
forecast
previous
release/effective time
source evidence IDs
calculation version
```

No forecast or actual may be created when the source did not provide it.

## 10. Policy direction

Policy direction MUST be derived from verified central-bank evidence and explicitly documented rules.

The engine MUST distinguish:

```text
current policy state
policy direction
historical policy events
```

A historical decision MUST NOT automatically be represented as the current policy state.

## 11. COT positioning

COT values are source facts; positioning interpretation is derived.

The engine may calculate outputs such as:

```text
net positioning
change in positioning
relative positioning
directional positioning state
```

All calculations MUST reference the applicable report date and source evidence.

COT must not be presented as a live intraday positioning feed.

## 12. Event impact

Scheduled events may be classified using deterministic metadata and rules.

Potential fields include:

```text
event
currency
scheduled time
importance
actual
forecast
previous
surprise
state
impact classification
```

Before an event occurs, actual MUST remain unavailable. The engine MUST NOT infer an actual outcome from market movement.

## 13. Pair candidate ranking

Pair discovery may combine macro outputs with deterministic technical outputs.

Candidate ranking MUST be reproducible and MUST preserve the v7 principle that:

> A cross pair is not disqualified merely because it is a cross.

The engine MUST also avoid automatically fading a trend because price is in Premium or automatically buying because price is in Discount.

Technical eligibility remains governed by the v7 trading doctrine and its deterministic engines.

## 14. Macro versus technical separation

The Macro Engine MUST NOT directly decide:

- entry price;
- stop loss;
- take profit;
- QMR pattern validity;
- structural break validity;
- liquidity sweep confirmation.

Those belong to the appropriate deterministic technical/execution layer.

The Macro Engine provides context and candidate ranking inputs.

## 15. Missing-data behavior

If a required macro input is unavailable:

```text
Missing evidence
    ↓
Mark affected output INSUFFICIENT_DATA / degraded
    ↓
Retain available verified outputs
    ↓
Do not invent substitute values
```

The downstream packet must expose the limitation.

## 16. Versioning and reproducibility

Every derived macro result SHOULD identify:

```text
engine_version
rule_set_version
input evidence IDs
calculation timestamp
```

Given identical validated inputs and identical rule versions, the engine SHOULD produce identical outputs.

## 17. AI boundary

AI may explain Macro Engine outputs.

AI MUST NOT:

- recalculate official actuals;
- invent missing forecasts;
- invent COT figures;
- override the engine's data-quality status;
- silently change macro regime classifications;
- present its own macro calculation as a verified system-derived value.

## 18. Provider independence

The Macro Engine MUST consume normalized evidence interfaces rather than provider-specific payloads.

Changing a provider must not require rewriting the macro methodology.

## 19. Initial implementation rule

The first vertical slice may use deterministic fixtures for COT and economic-calendar inputs.

Real production ingestion is a separate implementation phase and must pass the Evidence Infrastructure and Source Registry contracts first.

## 20. Non-goals

This specification does NOT define:

- a specific data vendor;
- API credentials;
- AI prompts;
- parser implementation;
- website schema;
- execution order/entry logic;
- final scoring thresholds before those thresholds are formally approved and tested.
