# HipMarvinFX v7 — Evidence Architecture & Implementation Directive

**Status:** FINAL
**Date:** 28 August 2026
**Repository:** `HipmarvinFx/hipmarvinfx-docs`
**Target:** HipMarvinFX v7 / v7.1 Evidence-First Architecture

---

## 1. Executive Decision

The current documentation repository is a methodology, contracts, templates, parser specifications, website contracts, migration, and handover repository. It is not the application/engine source repository.

**Do not restart HipMarvinFX.**

The v7 trading methodology is sufficiently mature. The next development phase is to build the **Deterministic Evidence Layer** underneath the existing methodology and progressively connect it to the actual application/engine.

The primary objective is:

> The system must gather, validate, normalize, timestamp, and classify factual market evidence before AI is allowed to interpret it.

AI must no longer be responsible for discovering, estimating, or inventing authoritative market facts.

---

## 2. Locked Target Architecture

```text
EXTERNAL SOURCES
      │
      ▼
SOURCE ADAPTERS
      │
      ▼
SOURCE VALIDATION
      │
      ▼
EVIDENCE STORE
      │
      ├───────────────┐
      ▼               ▼
MACRO ENGINE     TECHNICAL ENGINE
      │               │
      └───────┬───────┘
              ▼
       EVIDENCE PACKET
              │
              ▼
        AI / ANALYST
              │
              ▼
        RESEARCH FILE
              │
              ▼
           PARSER
              │
              ▼
        VALIDATION GATE
              │
              ▼
         PUBLICATION
```

This replaces the older implicit workflow in which manual research was supplied directly to AI and the parser was expected to protect the final result.

---

## 3. Methodology Is Frozen

The following are established v7 methodology and must not be unnecessarily rewritten:

- Macro → technical hierarchy
- HTF-first analysis
- Daily / 4H / 1H minimum framework
- Trend-first doctrine
- Structural-break countertrend gate
- QMR sequencing
- QM/QML refinement
- 20D location
- Liquidity/Flow Regime
- Premium/Discount interpretation
- Liquidity-target discipline
- Trade-risk rules
- Historical v5/v6 compatibility
- Research → parser → publication separation
- Anti-fabrication principles

In particular:

> Premium is not automatically a sell signal.

> Discount is not automatically a buy signal.

Directional flow, acceptance, liquidity, structure, and trend remain the deciding context.

The GBP/USD-style trend-extension discovery remains valid.

Cross-pair discovery remains valid. The system must never exclude a pair merely because it is a cross.

---

## 4. Evidence Responsibility Boundary

All important information must be separated into three classes:

### FACT

A value directly obtained from an authoritative or approved source.

Examples:

- US CPI Actual
- US CPI Forecast
- US CPI Previous
- CFTC Net Position
- ECB Rate Decision
- GBP/USD verified price

### DERIVED

A deterministic calculation performed by the system from verified facts.

Examples:

- CPI Surprise
- Currency Strength Rank
- 20D Location
- Trend Classification
- Flow Regime
- COT Change
- Macro Regime Score
- Pair Ranking

### INTERPRETATION

Human/AI reasoning based only on FACT and DERIVED information.

Examples:

- USD remains structurally supported.
- GBP/USD offers the strongest continuation expression.
- Premium location does not invalidate the bullish trend.

AI is allowed to perform interpretation.

AI is **not** allowed to manufacture FACT.

---

## 5. Evidence Status Is Mandatory

Every important evidence item must carry an explicit state:

```text
VERIFIED
PENDING
STALE
NOT_AVAILABLE
INVALID
```

The system must never silently convert `PENDING`, `STALE`, or `NOT_AVAILABLE` into `VERIFIED`.

If authoritative evidence cannot be obtained, the system reports that the evidence is unavailable. It does not guess.

---

## 6. Evidence Provenance Is Mandatory

Externally sourced factual values must be traceable wherever applicable.

Minimum provenance fields:

```text
source
provider
source_type
retrieved_at
effective_at
currency
event
value
unit
status
source_reference
```

For economic events, also support:

```text
forecast
previous
actual
revision
```

The system must be able to answer where a factual value came from and when it was valid without asking AI to reconstruct that information.

---

## 7. Evidence Layer Comes First

Create the following canonical specification first:

`EVIDENCE_INFRASTRUCTURE_V1.md`

It must define:

- evidence object
- provenance
- validation
- freshness
- status
- source authority
- duplicate handling
- conflict handling
- failure handling
- normalization
- timestamp rules

This becomes the foundational contract for the remaining v7 infrastructure.

---

## 8. Source Registry

Create:

`EVIDENCE_SOURCE_REGISTRY_V1.md`

The registry must define approved sources for:

- market prices
- CFTC/COT
- central banks
- official economic/statistical data
- economic calendar
- approved news/RSS

Prefer authoritative official sources wherever practical.

News must not become an unrestricted AI browsing exercise.

---

## 9. Evidence Packet

Create:

`EVIDENCE_PACKET_CONTRACT_V1.md`

The Evidence Packet becomes the primary factual context supplied to AI.

It should contain compact validated information such as:

```text
HIPMARVINFX EVIDENCE PACK

DATA HEALTH
Price: VERIFIED
COT: VERIFIED
Calendar: VERIFIED
Central Banks: VERIFIED
Macro Data: VERIFIED
News: VERIFIED

MACRO REGIME
USD: ...
EUR: ...
GBP: ...
JPY: ...

CURRENCY STRENGTH
1. USD
2. ...
3. ...

KEY MACRO DRIVERS
...

COT
...

KEY EVENTS
...

TECHNICAL ENGINE OUTPUT
...

PAIR CANDIDATES
...

RISK FLAGS
...

CHANGES SINCE LAST UPDATE
...
```

AI does not need the entire raw source database. It needs the validated evidence required for the decision.

---

## 10. Delta Updates Are Required

Token minimization is an architectural requirement.

The system must distinguish between a full Evidence Pack and a delta Evidence Pack.

If nothing changed:

```text
COT: UNCHANGED
ECB: NO NEW RELEASE
USD TECHNICAL STATE: UNCHANGED
```

Do not resend unchanged datasets.

If new evidence arrives, send only the new evidence and affected derived outputs.

---

## 11. Macro Engine

Create:

`MACRO_ENGINE_SPEC_V1.md`

The macro engine should deterministically normalize/calculate, where supported:

- Macro Regime
- Currency Relative Strength
- Macro Driver Ranking
- Event Impact
- COT Positioning
- Policy Direction
- Macro Surprise
- Pair Candidate Ranking

Defined scoring rules may be used. An LLM must not invent the score.

AI may explain the resulting score.

---

## 12. Technical Engine

The technical engine should deterministically provide the values that AI previously had to infer.

Minimum outputs:

```text
HTF Trend
Trend Alignment
Structural Break
20D Location
Liquidity State
Flow Regime
QMR Phase
QM/QML State
Acceptance/Rejection
Liquidity Target
Execution Eligibility
```

Minimum execution timeframe remains **1H**. No sub-1H execution should be introduced into v7 without a separate approved methodology change.

---

## 13. AI Responsibility Is Narrow

AI is responsible for:

- interpretation
- reasoning
- narrative
- comparison
- explanation
- scenario construction
- research writing
- reader-facing communication

AI is not responsible for:

- authoritative price discovery
- COT retrieval
- economic-calendar verification
- official-data retrieval
- central-bank fact verification
- inventing missing numbers
- inventing forecasts
- inventing actuals
- inventing technical levels
- silently correcting missing evidence

If evidence is unavailable, the state remains `NOT_AVAILABLE`.

---

## 14. Parser Is the Final Firewall

The parser remains deterministic.

Its job is:

```text
Normalize
Validate
Reject
Publish
```

It must never become an AI correction mechanism.

Examples:

```text
Unsupported price       → REJECT
Unsupported COT         → REJECT
Unsupported macro fact → REJECT
Missing provenance     → REJECT
Stale evidence         → STALE
Required evidence missing → PENDING
```

The parser must never silently fill missing data.

---

## 15. v7 Parser Contracts

Create:

```text
PARSER_IMPLEMENTATION_SPEC_v7.md
PARSER_TEST_PLAN_v7.md
PARSER_FIXTURE_V7.md
WEBSITE_PARSER_CONTRACT_v7.md
WEBSITE_PAGE_SCHEMA_v7.md
```

The v6 documents remain historical compatibility references and must not be deleted merely because v7 replacements exist.

---

## 16. Required v7 Safety Tests

The v7 test suite must include, at minimum:

```text
AI-generated unsupported price → REJECT
AI-generated unsupported COT → REJECT
AI-generated unsupported macro actual → REJECT
Missing provenance → REJECT
Stale evidence → STALE
Unavailable source → NOT_AVAILABLE
Missing required evidence → PENDING
Countertrend without structural break → REJECT
Sub-1H execution → REJECT
Premium without bearish structure → DO NOT AUTO-SELL
Discount without bullish structure → DO NOT AUTO-BUY
Directional accepted flow in Premium → continuation remains eligible
Historical v5/v6 data → preserve compatibility
```

---

## 17. Website Is Presentation Only

The website must consume normalized, validated output.

It must not independently calculate:

- macro regime
- COT
- 20D location
- trend
- liquidity
- flow
- QMR

The target chain is:

```text
Evidence
→ Engine
→ Evidence Packet
→ Research
→ Parser
→ Website
```

The website must not become another interpretation layer.

---

## 18. Manual Workflow Remains as Fallback

The existing manual research workflow is retained deliberately.

We are not replacing human review with blind automation.

The target workflow is:

```text
AUTOMATED EVIDENCE
        +
HUMAN REVIEW
        +
AI INTERPRETATION
```

Manual input remains available when an automated source is unavailable or fails.

Manual evidence must be explicitly marked and must not masquerade as automatically verified evidence.

---

## 19. First Vertical Slice

Do not attempt to build the entire system at once.

The first working vertical slice is:

```text
VERIFIED PRICE
      ↓
COT
      ↓
ECONOMIC CALENDAR
      ↓
EVIDENCE VALIDATION
      ↓
EVIDENCE PACKET
      ↓
AI INTERPRETATION
      ↓
PARSER
      ↓
VALIDATED OUTPUT
```

This slice must prove that:

1. Evidence can be retrieved.
2. Evidence can be validated.
3. Evidence can be timestamped.
4. Evidence can be normalized.
5. AI can consume evidence without inventing facts.
6. The parser can reject unsupported facts.
7. Validated output can reach the publication layer.

Only after this works should additional source families be expanded systematically.

---

## 20. Implementation Order

The development sequence is:

```text
PHASE 1  Evidence Contract
        ↓
PHASE 2  Source Registry
        ↓
PHASE 3  Evidence Store
        ↓
PHASE 4  Price + COT + Calendar adapters
        ↓
PHASE 5  Evidence Validation
        ↓
PHASE 6  Evidence Packet
        ↓
PHASE 7  Macro Engine
        ↓
PHASE 8  Technical Engine
        ↓
PHASE 9  v7 Parser
        ↓
PHASE 10 AI Prompt Boundary
        ↓
PHASE 11 Website Contract
        ↓
PHASE 12 Delta Updates / Token Optimization
        ↓
PHASE 13 Additional sources
        ↓
PHASE 14 End-to-end production testing
```

Do not skip ahead merely because an individual source integration appears easy. The contracts come first.

---

## 21. Definition of Done

HipMarvinFX v7 is not complete merely because AI produces convincing weekly research.

It is complete when:

```text
FACTS ARE VERIFIED
        +
DERIVED VALUES ARE DETERMINISTIC
        +
AI INTERPRETS INSTEAD OF INVENTING
        +
PARSER REJECTS UNSUPPORTED CLAIMS
        +
PROVENANCE IS TRACEABLE
        +
STALE/MISSING DATA IS EXPLICIT
        +
MANUAL FALLBACK EXISTS
        +
WEBSITE CONSUMES VALIDATED OUTPUT
        +
AI CONTEXT IS DELTA-OPTIMIZED
```

---

## 22. Repository Change Policy

Do not perform another wholesale documentation rewrite.

Use the current repository as the baseline.

The immediate next documents are:

```text
EVIDENCE_INFRASTRUCTURE_V1.md
EVIDENCE_SOURCE_REGISTRY_V1.md
EVIDENCE_PACKET_CONTRACT_V1.md
MACRO_ENGINE_SPEC_V1.md
PARSER_IMPLEMENTATION_SPEC_v7.md
PARSER_TEST_PLAN_v7.md
PARSER_FIXTURE_V7.md
WEBSITE_PARSER_CONTRACT_v7.md
WEBSITE_PAGE_SCHEMA_v7.md
```

Existing v5/v6 methodology, migration, parser, website, and handover documents must be preserved for compatibility and architectural history.

---

## 23. Security Requirement

No API key, private credential, secret token, password, `.env` value, or private service credential may be committed to this public documentation repository or exposed through AI prompts.

Use:

```text
environment variables
secret stores
deployment-platform secrets
```

Never use hard-coded credentials or public configuration files containing secrets.

AI and developers should receive only the minimum credentials necessary for the operation being performed.

---

## 24. Final Strategic Position

The HipMarvinFX problem is no longer primarily a trading-methodology problem.

The methodology is sufficiently mature.

The problem is now **trusted evidence infrastructure**.

The system must move from:

```text
AI tries to discover reality
```

to:

```text
SYSTEM ESTABLISHES REALITY
        ↓
AI EXPLAINS REALITY
```

The objective is not to make AI do more.

The objective is to make AI **do less, but do it reliably**.

The system should automatically establish what is known, explicitly identify what is missing, deterministically calculate what can be calculated, and allow AI to focus on the part where it adds the most value:

> **Reasoning, interpretation, and communication.**

This directive is the architectural starting point for the HipMarvinFX v7 Evidence-First implementation phase.
