# HipMarvinFX v7.2 — Time Reactivity + ABC Implementation Directive

**Date:** 11 September 2026  
**Status:** APPROVED DEVELOPMENT EXTENSION / IMMEDIATE TEAM HANDOFF  
**Scope:** Extend the existing v7 evidence-first + HTF/QMR architecture. Do not rebuild or create parallel strategy engines.

## 1. Purpose

This document reconciles the existing v7 architecture with the newly approved extensions:

- multi-scale completed-period liquidity profiles;
- formal ABC formation/validation;
- integration of ABC with QMR for both continuation and reversal;
- a DST-aware Time Reactivity Engine;
- macro catalyst timing;
- unified TIME × PRICE × LIQUIDITY × STRUCTURE × REACTION execution state.

The existing v7 methodology remains authoritative unless explicitly changed below.

## 2. Existing Architecture — Retain

The current architecture remains:

```text
External Sources
→ Evidence Adapters
→ Validation
→ Evidence Store
→ Macro / Technical Engines
→ Evidence Packet
→ AI Interpretation
→ Parser Firewall
→ Publication
```

The existing trading hierarchy remains:

```text
Macro Regime
→ Weekly Thesis
→ HTF Trend / Structure
→ 20D Location
→ Liquidity
→ QMR
→ Reaction
→ QM/QML
→ Liquidity Target
→ Risk
```

Existing locked principles remain unchanged:

- trend first;
- Daily / 4H / 1H minimum framework;
- no sub-1H execution;
- continuation is default;
- countertrend requires confirmed HTF structural break;
- Premium/Discount is location, not direction;
- liquidity sweep alone is not reversal confirmation;
- QMR is Quality → Manipulation → Reaction;
- QM/QML is entry refinement, not bias engine;
- AI interprets verified/derived evidence and must not manufacture facts;
- explicit `VERIFIED / PENDING / STALE / NOT_AVAILABLE / INVALID` states remain mandatory.

## 3. Architectural Decision

**Do not create:**

- a second liquidity engine;
- a separate ABC strategy;
- a standalone kill-zone trading strategy;
- a second Evidence Packet;
- an AI prediction engine.

**Extend existing deterministic engines and the existing Evidence Packet.**

The new layers are evidence/context/state modules feeding the existing decision architecture.

## 4. Updated Machine Hierarchy

```text
MARKET DATA
    ↓
REFERENCE MAP
    ↓
QUARTERLY / MONTHLY / WEEKLY PROFILE
    ↓
HTF FLOW / STRUCTURE
    ↓
DEALING RANGE VALIDATOR
    ↓
LIQUIDITY MAP
    ↓
QMR
    ↓
ABC FORMATION / VALIDATION
    ↓
PRICE REACTION
    ↓
TIME REACTIVITY
    ↓
MACRO CATALYST (optional)
    ↓
TIME × PRICE × REACTION
    ↓
EXECUTION STATE
    ↓
EXISTING EVIDENCE PACKET
    ↓
AI EXPLANATION
```

The arrows represent reasoning hierarchy, not a requirement that every layer must independently generate a signal on every trade.

## 5. Multi-Scale Profile Extension

Implement deterministic completed-period reference profiles first:

### Previous completed quarter

- PQH
- PQL
- PQ midpoint

### Previous completed month

- PMH
- PML
- PM midpoint

### Previous completed week

- PWH
- PWL
- PW midpoint

These are **context/liquidity references**, not directional signals.

Initial implementation must use completed calendar periods. A rolling three-month profile is a separate research variant and must not be mixed into the initial canonical implementation.

Profiles answer **WHERE**. HTF structure/flow answers **WHAT DIRECTION/STATE**.

## 6. Dealing Range Validator

Create/extend one deterministic validator responsible for:

```text
DRH
DRL
Midpoint
Premium / Discount
Current percentile/location
Validity state
```

No ABC/QMR interpretation may rely on an invalid dealing range where a valid DR is required.

The validator is contextual and must not create direction by itself.

## 7. ABC Definition

ABC is a **formation/validation architecture**, not simply three sequential geometric points.

### A

`A = the relevant structural reference participating in the ABC formation.`

A may originate from:

- recognised liquidity reference;
- swing point;
- structural reference;
- other qualifying reference designated `X` when no predefined liquidity label applies.

Do **not** hard-code A to PWH/PWL/PMH/PML/PDH/PDL/PQH/PQL.

### C

`C = qualifying reaction/reference zone.`

Examples include:

- FVG;
- OB;
- liquidity zone;
- other approved structural reference.

### B

`B = subsequent reaction/validation after the relevant interaction.`

A and C may spatially overlap or be strongly aligned.

## 8. ABC — FVG Midpoint Alignment

Support an optional subtype:

`ABC-AFM = A / FVG Midpoint Alignment`

Example:

```text
DRH
 │
 │ 75%
 │
 │   FVG / C
 │      │
 │      A   ← A aligned with FVG midpoint
 │
 │ 50%
 │
 │ 25%
 │
DRL
```

This is a strong formation subtype but is **not yet a mandatory universal ABC condition**. It must be measured during research before being promoted to a hard gate.

## 9. ABC State Machine

Implement deterministic states:

```text
NONE
→ FORMING
→ ZONE_QUALIFIED
→ A_IDENTIFIED
→ INTERACTION
→ REACTION
→ VALIDATED
→ EXECUTION_READY
→ INVALIDATED
```

The engine must never jump from `NONE` directly to a directional trade decision.

## 10. QMR + ABC Integration

ABC is subordinate to the existing QMR/HTF hierarchy.

### Continuation path

```text
HTF trend
→ valid location
→ liquidity interaction / pullback
→ QMR manipulation
→ reaction
→ ABC validation
→ QM/QML if present
→ execution
```

### Reversal path

```text
HTF trend
→ meaningful protected swing break
→ displacement / acceptance
→ QMR reaction
→ ABC validation
→ QM/QML if present
→ countertrend execution eligibility
```

CISD may be one qualifying reaction mechanism where applicable, but **CISD is not a universal requirement**. QMR continuation must remain valid without CISD when the approved structural/reaction conditions are met.

## 11. Time Reactivity Engine

Do not implement an "ICT Kill Zone Strategy."

Implement a **Time Reactivity Engine** that determines when the asset historically deserves increased attention.

Inputs:

- DST-aware session windows;
- London session;
- New York session;
- London/New York overlap;
- day-of-week;
- scheduled macro events;
- pre-event/post-event windows;
- asset-specific historical reactivity;
- eventually empirical XAUUSD reaction statistics.

ICT-style session windows are initial research priors only. They are not directional rules and must not be hard-coded as guaranteed edge.

## 12. Time Reactivity Data Contract

```yaml
TIME_CONTEXT:
  asset: XAUUSD
  timestamp: ...
  timezone: ...
  day_of_week: ...

  session:
    name: ASIA | LONDON | NEW_YORK | OVERLAP | OTHER
    state: ACTIVE | INACTIVE

  reactivity:
    historical_score: ...
    current_score: ...
    classification: QUIET | NORMAL | REACTIVE | HIGHLY_REACTIVE

  scheduled_events: []

  event_state:
    NONE | PRE_EVENT | RELEASE | POST_EVENT | EVENT_RISK

  directional_edge:
    NONE | BULLISH | BEARISH | CONFLICTED
```

**Reactivity and direction must remain separate variables.** High activity does not automatically imply bullish or bearish direction.

## 13. Macro Catalyst Engine Integration

Extend the existing macro/event architecture rather than creating a parallel calendar engine.

Each event should support:

```text
event_id
event_name
currency
scheduled_time
source_timezone
importance
pre_window
post_window
actual
forecast
previous
surprise
state
impact_on_asset
provenance
```

Before release, `actual` must remain unavailable.

The system must support both:

### Catalyst-driven path

```text
TIME WINDOW
+ PRICE LOCATION
+ SCHEDULED EVENT
+ PRICE RESPONSE
→ qualification
```

### Non-catalyst path

```text
TIME WINDOW
+ PRICE LOCATION
+ LIQUIDITY / STRUCTURAL EVENT
+ PRICE RESPONSE
→ qualification
```

Therefore:

`Catalyst ≠ Reactive Window`  
`No Catalyst ≠ No Trade`  
`Catalyst ≠ Automatic Trade`

## 14. Unified Execution State

Extend the existing technical/evidence state with:

```yaml
MARKET_STATE:
  time:
    classification: ...

  location:
    profile: ...
    dealing_range: ...
    premium_discount: ...

  liquidity:
    relevant_reference: ...
    state: ...

  structure:
    daily: ...
    h4: ...
    alignment: ...
    structural_break: ...

  qmr:
    phase: ...

  abc:
    state: ...
    A: ...
    B: ...
    C: ...
    subtype: ...

  catalyst:
    required: false
    state: NONE

  reaction:
    state: PENDING | CONFIRMED | INVALIDATED

  execution:
    state: WAIT | QUALIFIED | INVALIDATED | NOT_AVAILABLE
```

This is an extension of the existing Evidence Packet technical context, not a new packet format.

## 15. Evidence Classification

All new outputs must preserve:

```text
FACT
DERIVED
INTERPRETATION
```

Examples:

- session timestamp = FACT/normalized calendar fact;
- previous quarter high = DERIVED from validated OHLC;
- ABC state = DERIVED;
- historical reactivity score = DERIVED;
- "London may be worth monitoring" = INTERPRETATION;
- "sell because London opened" = invalid reasoning.

## 16. Empirical Research Requirement

Do not promote timing assumptions into hard trading rules without testing.

The historical research dataset should measure, by time window:

- activity;
- liquidity interaction;
- volatility;
- sweep frequency;
- displacement frequency;
- reaction frequency;
- QMR confirmation frequency;
- ABC validation frequency;
- execution eligibility;
- target delivery;
- failure/invalidation rate.

The objective is to measure **reactivity relevant to this methodology**, not merely volatility.

Historical classification must first verify whether the engine correctly identified the market state and sequence before optimizing P&L.

## 17. Implementation Order — Immediate

### P0 — Reconcile current application

Before coding:

1. Confirm actual application repository/branch/commit.
2. Inspect existing technical/evidence engines.
3. Produce CURRENT REALITY vs REQUIRED matrix.
4. Reconcile docs against runtime/code.
5. Do not mark documentation-only requirements as implemented.

### P1 — Existing v7 foundation verification

Verify/complete in existing architecture:

- market data;
- Daily / 4H / 1H OHLC;
- HTF structure;
- trend alignment;
- protected swings;
- structural break;
- 20D location;
- liquidity/flow;
- QMR;
- QM/QML;
- Evidence Packet;
- parser firewall.

### P2 — Profile + DR extension

Implement:

- previous completed quarter H/L/mid;
- previous completed month H/L/mid;
- previous completed week H/L/mid;
- deterministic DR validator;
- premium/discount location;
- provenance/status for all outputs.

### P3 — ABC extension

Implement:

- A reference resolver;
- X fallback reference;
- C-zone resolver;
- B reaction resolver;
- ABC state machine;
- A/FVG midpoint alignment subtype;
- continuation and reversal integration with QMR.

### P4 — Time Reactivity

Implement:

- timezone/DST-safe session resolver;
- session windows;
- day-of-week context;
- historical reactivity storage/calculation;
- pre/post macro windows;
- `TIME_CONTEXT` output.

### P5 — Catalyst integration

Reuse existing macro/calendar architecture.

Implement event state transitions:

```text
UPCOMING
→ PRE_EVENT
→ RELEASE
→ POST_EVENT
→ CLOSED
```

with explicit missing/invalid states.

### P6 — Unified state + Evidence Packet

Extend the existing packet technical context with:

- profiles;
- DR;
- ABC;
- time reactivity;
- catalyst state;
- reaction state;
- execution state.

Do not create a second packet.

### P7 — Research/backtest classification

Run historical classification tests before enabling any new timing rule as a hard trade gate.

### P8 — Parser/adversarial tests

Add tests for:

- fabricated price;
- missing event actual;
- stale event;
- invalid session conversion;
- incorrect DST conversion;
- ABC without valid evidence;
- ABC without valid DR where DR is required;
- sweep without reaction;
- countertrend ABC without HTF structural break;
- QMR continuation without CISD;
- Premium continuation with intact HTF flow;
- no-catalyst reactive-window setup;
- catalyst present but no valid price reaction;
- unsupported AI factual claims.

## 18. Trader-Facing Output

Internal complexity must remain hidden behind progressive disclosure.

Primary view should answer:

```text
WHERE?
DIRECTION?
LIQUIDITY?
SETUP?
IS IT NOW?
```

Example:

```text
XAUUSD
BEARISH

Location: Premium
Liquidity: PMH
Profile: Previous Month / Previous Week context
Formation: ABC + QMR
Time: Highly Reactive
Catalyst: None required
Reaction: Confirmed
Status: QUALIFIED

Entry: [deterministic]
Invalidation: [deterministic]
Target: [deterministic liquidity target]
```

If confirmation is incomplete:

```text
STATUS: WAIT
Waiting for: valid liquidity interaction + reaction
Entry: NOT_CONFIRMED
```

## 19. Non-Goals / Guardrails

Do not:

- create a prediction engine;
- force a trade because time is active;
- force a trade because a catalyst exists;
- force ABC symmetry;
- require FVG midpoint alignment for every ABC;
- require CISD for every QMR;
- make quarterly/monthly/weekly profiles directional;
- use rolling three-month profiles as canonical before separate validation;
- lower execution below 1H;
- allow AI to infer unsupported factual state;
- replace existing QMR/HTF/evidence architecture.

## 20. Definition of Done

The extension is complete only when real validated market data can produce:

```text
PERIOD PROFILE
→ DEALING RANGE
→ HTF STRUCTURE
→ LIQUIDITY
→ QMR
→ ABC STATE
→ TIME REACTIVITY
→ OPTIONAL CATALYST STATE
→ PRICE REACTION
→ EXECUTION STATE
→ EVIDENCE PACKET
→ PARSER VALIDATION
```

with deterministic provenance and explicit unavailable/stale/invalid behavior.

The final system must answer both:

> **Where is price in the liquidity hierarchy?**

and:

> **When is this environment historically/reactively worth attention?**

without converting timing, profile, liquidity, or pattern labels into unsupported directional predictions.

## 21. Team Instruction

**Start implementation from P0. Do not redesign the methodology. Do not create parallel engines. Compare this directive against the actual application and the canonical v7 documents, identify the smallest verified implementation gap, implement in order, test, and update the handover/state record after each completed phase.**
