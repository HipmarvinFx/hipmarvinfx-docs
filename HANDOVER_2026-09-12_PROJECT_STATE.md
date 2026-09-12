# HIPMARVINFX — COMPREHENSIVE PROJECT HANDOVER

**Date:** 12 September 2026  
**Project:** HipMarvinFX  
**Production domain:** `hipmarvinfx.com`  
**Framework:** Next.js  
**Deployment:** Vercel  
**Primary branch:** `main`

> **Purpose:** This document is the project-state handover and working source of truth for the current development phase. It records what is established, what is open, and the recommended order of work. It contains no credentials or secrets.

---

## 1. EXECUTIVE STATE

HipMarvinFX has evolved beyond a marketing website into an evidence-driven forex research and membership platform with:

- premium research/membership functionality;
- evidence-first market-data architecture;
- QMR trading framework;
- ABC liquidity-mapping innovation;
- live-trade and canonical rating logic;
- future machine-readable market reasoning;
- Vercel production deployment.

The core project principle is:

> **The system must never manufacture market facts, prices, trade states, macro conditions, or confirmations that the underlying evidence does not actually support.**

The project is deployed, but it is **not yet considered fully production-validated** until access control, API exposure, evidence integrity, and deterministic ABC rules have been verified.

---

## 2. CURRENT HIGHEST PRIORITY — SECURITY

The immediate issue is whether `/live-trades` and the underlying data APIs are actually protected.

Removing a Live Trades link from the landing page is **not** security.

### Mandatory test

Using an incognito/logged-out browser, directly visit:

`https://hipmarvinfx.com/live-trades`

Then inspect the browser network requests and test the underlying API/data endpoints independently.

### Expected

```text
PUBLIC VISITOR
    ↓
No protected trade/research data

UNAUTHORIZED USER
    ↓
No protected trade/research data

AUTHORIZED MEMBER
    ↓
Protected data available
```

If an unauthenticated visitor can receive live trades, ratings, research, or premium data, **stop feature expansion and fix the server/API authorization boundary first.**

---

## 3. UI HIDING VS REAL SECURITY

These are not equivalent:

- hiding a navigation link;
- hiding a button;
- hiding a card;
- removing a route from the homepage.

Real protection must exist at the server/API/data-access level.

A protected page with a public JSON/API endpoint is still effectively public.

---

## 4. LIVE-TRADE RATING STATE

The latest known implementation direction moved live-trade ratings to a **canonical derivation** rather than allowing multiple independent rating calculations.

The authoritative principle is:

> **One authoritative derivation path should determine the trade rating; the UI should consume that result.**

### Closed direction

- canonical live-trade rating derivation implemented;
- change committed/pushed in the implementation workflow;
- production deployment exists.

### Still open

- verify canonical rating in production runtime;
- verify no older/parallel derivation can override it;
- verify page/API consistency;
- verify unauthorized users cannot access the result.

---

## 5. EVIDENCE-FIRST ARCHITECTURE

The intended chain is:

```text
REAL MARKET / EXTERNAL SOURCE
        ↓
DATA ADAPTER
        ↓
NORMALIZATION
        ↓
VALIDATION
        ↓
EVIDENCE PACKET
        ↓
RULE ENGINE
        ↓
AI INTERPRETATION
        ↓
FINAL REPORT
```

AI must reason from evidence. It must not become the source of market facts.

### Evidence states

- `VERIFIED`
- `PENDING`
- `STALE`
- `NOT_AVAILABLE`
- `INVALID`

### Semantic types

- `FACT`
- `DERIVED`
- `INTERPRETATION`

These distinctions must remain explicit in machine-readable output and user-facing research where relevant.

---

## 6. PRICE-DATA INTEGRITY

Hard rule:

> **If actual price evidence is unavailable, state that it is unavailable. Never guess a price.**

The system must not:

- invent current price;
- invent candle values;
- invent market structure;
- claim a feed is current when stale;
- substitute hypothetical prices without explicitly labeling them hypothetical.

---

## 7. MARKET-DATA ARCHITECTURE

Known direction:

- **Primary:** Yahoo Finance
- **Fallback:** TwelveData

Fallback does not mean permission to fabricate data.

The expected behavior is:

```text
Yahoo
  ↓
Validate
  ↓
Evidence

If unavailable
  ↓
TwelveData
  ↓
Validate
  ↓
Evidence

If both unavailable
  ↓
NOT_AVAILABLE
```

### Open verification

A previous audit identified a possible mapping issue in:

`lib/market-data/adapters/yahoo-finance.ts`

with:

```text
"4H": "1h"
```

The intended mapping discussed was:

```text
"4H": "4h"
```

Verify the current repository before changing anything; do not assume this remains present or already fixed.

---

## 8. COT / MACRO / CALENDAR

The project recognizes that price structure alone is insufficient.

Relevant evidence layers include:

- COT;
- macroeconomic regime;
- economic calendar;
- scheduled catalysts;
- session timing;
- price location;
- HTF structure;
- liquidity.

The conceptual architecture is established, but each source must be classified honestly as:

- automated;
- partially automated;
- manually ingested;
- deferred;
- unavailable.

Do not mark the macro layer complete merely because the doctrine exists in documentation.

---

## 9. APPROVED QMR ARCHITECTURE

QMR remains the approved trading framework. It is not being replaced.

The hierarchy is:

```text
DIRECTION
   ↓
DEALING RANGE
   ↓
LIQUIDITY MAP
   ↓
QMR SEQUENCE
   ↓
REACTION
   ↓
ENTRY
```

The broader doctrine remains:

```text
Macro Regime
      ↓
Weekly Thesis
      ↓
HTF Trend / Structure
      ↓
20D Location
      ↓
Liquidity
      ↓
QMR
      ↓
Reaction
      ↓
QM / QML
      ↓
Liquidity Target
      ↓
Risk
```

QMR/QML is an entry refinement, not a standalone reversal system. A pattern must not override HTF trend, structure, dealing range, or liquidity context.

---

## 10. ABC LIQUIDITY-MAPPING INNOVATION

ABC is a liquidity-mapping and sequence framework to be integrated with QMR, not a competing strategy.

Core concepts:

- DRL is the initial liquidity reference that can be attacked/spiked and rejected;
- B is the first meaningful reaction/wave and setup validator;
- A is the relevant swing/liquidity reference subsequently interacted with by C;
- A is not automatically the 50% level;
- C must be evaluated in the relevant premium/reactive zone;
- for bearish structures, the working premium region is 50–75% of the relevant dealing range;
- liquidity references may include swing points, FVGs, order blocks, or confluence;
- the reference must actually sit in the C target/reactive zone to matter;
- internal/pre-swing liquidity inside DRH–DRL can act as relevant X/C liquidity;
- ABC can be used with the prevailing trend and does not require CISD in every setup.

---

## 11. CISD / DEALING RANGE

A bullish structural example:

```text
HL
 ↓
BOS
 ↓
HH
 ↓
pullback
 ↓
previous HL liquidity taken
 ↓
close below HL
 ↓
CISD
```

The swing structure associated with the CISD context helps define DRH/DRL.

ABC is then mapped within that dealing range.

Bearish logic is the inverse.

Important: ABC is **not restricted to CISD setups**. It can also operate with a prevailing HTF trend where QMR is being used with the trend.

---

## 12. ABC RULES STILL OPEN FOR DETERMINISTIC FREEZE

Seven questions must become explicit, machine-testable rules:

1. What exactly constitutes the DRL attack/rejection?
2. What makes X3 a valid reclaim?
3. What makes an intermediate X4/X5 swing qualify?
4. What is the exact deterministic rule for selecting A?
5. What makes C a valid premium/reactive zone?
6. What constitutes the qualifying reaction?
7. When does the whole ABC become invalid or stale?

These are **not closed** until they are written as deterministic conditions that can be implemented and backtested without discretionary interpretation.

---

## 13. REJECTION DEFINITION

Rejection must not be reduced to “wick through level and reverse.”

Potential valid forms include:

- wick/spike through liquidity;
- close through liquidity followed by reclaim;
- displacement;
- imbalance/re-entry behavior;
- deterministic return above/below the relevant boundary.

The final machine rule must define which of these qualify and under what conditions.

---

## 14. TIME + PRICE + CATALYST

The intended research engine should eventually answer:

```text
WHAT PRICE?
WHEN?
WHY NOW?
```

or:

```text
PRICE LOCATION
+
TIME WINDOW
+
CATALYST
```

Relevant timing context includes:

- London session;
- New York session;
- ICT kill zones;
- session opens/transitions;
- scheduled macro releases;
- weekly/monthly/quarterly context.

These are context/probability filters, not automatic trade triggers.

---

## 15. QUARTERLY / MONTHLY / WEEKLY LIQUIDITY PROFILE

The proposed hierarchy is:

```text
QUARTER
   ↓
MONTH
   ↓
WEEK
   ↓
DAY
   ↓
SESSION
   ↓
EXECUTION
```

Quarterly methodology still requires a final decision between:

- rolling 3-month range excluding the current month;
- normal calendar quarter (Q1/Q2/Q3/Q4).

Monthly context includes:

- PMH — Previous Month High;
- PML — Previous Month Low;
- monthly 50% midpoint.

Weekly context includes:

- PWH — Previous Week High;
- PWL — Previous Week Low.

These are contextual liquidity references, not isolated strategies.

---

## 16. LIVE ABC EXAMPLE

A live bearish ABC example reviewed during development included:

- a pink FVG;
- the FVG midpoint sitting in the premium zone;
- the midpoint aligning with A;
- A also representing the relevant swing point.

Implementation lesson:

> An FVG is not automatically meaningful because it exists. Its relationship to the dealing range, premium zone, A, liquidity, and expected reaction determines its relevance.

---

## 17. WHAT IS CLOSED

### Product / platform

- HipMarvinFX is a premium forex research/membership product.
- Next.js is the application framework.
- Vercel is the deployment platform.
- Monthly subscription architecture is established.
- Cancellation is before the next billing cycle.
- Do not invent refund guarantees.

### Evidence

- AI must not invent market facts.
- Missing evidence remains missing.
- Evidence states are explicit.
- FACT / DERIVED / INTERPRETATION separation is established.

### QMR

- QMR remains approved.
- QMR operates within HTF context.
- QM/QML is entry refinement.
- Pattern cannot override structure/trend.

### ABC

- ABC is a liquidity-mapping innovation.
- ABC can work with trend.
- ABC can work with CISD.
- ABC integrates with QMR.
- Premium/discount and dealing-range location matter.
- Internal liquidity matters.
- FVG/OB/swing references can participate as liquidity references.

### Live ratings

- Canonical rating derivation direction has been implemented.

### Deployment

- Production deployment exists.
- `main` is the production branch.

---

## 18. WHAT REMAINS OPEN

### CRITICAL

- Direct unauthenticated `/live-trades` access.
- Underlying live-trade API exposure.
- Membership/authorization boundary.
- Production runtime verification.

### HIGH PRIORITY

- Deterministic ABC specification.
- ABC state machine.
- ABC invalidation/staleness.
- Market-data freshness rules.
- Yahoo/TwelveData fallback verification.
- COT implementation status.
- Macro/calendar implementation status.
- Catalyst data implementation status.
- Canonical rating runtime verification.

### RESEARCH

- Quarterly profile methodology.
- Time/price/catalyst weighting.
- Historical backtest validation.

---

## 19. RECOMMENDED ORDER OF ACTION

### PHASE 1 — SECURITY

1. Test `/live-trades` while logged out/incognito.
2. Inspect network/API requests.
3. Identify every endpoint serving protected data.
4. Test those endpoints directly while unauthenticated.
5. Fix any exposure before adding features.

### PHASE 2 — ACCESS-CONTROL CONTRACT

Explicitly define:

```text
PUBLIC
AUTHENTICATED
ACTIVE MEMBER
ADMIN
```

Map every protected page/API to a role.

### PHASE 3 — ABC DETERMINISM

Convert the seven open ABC questions into:

```text
INPUT
CONDITION
VALIDATION
STATE
FAILURE CONDITION
EXPIRY CONDITION
OUTPUT
```

### PHASE 4 — QMR + ABC

Use:

```text
HTF DIRECTION
      ↓
DEALING RANGE
      ↓
LIQUIDITY MAP
      ↓
QMR CONTEXT
      ↓
ABC QUALIFICATION
      ↓
REACTION
      ↓
ENTRY
```

Do not create a second competing QMR system.

### PHASE 5 — TIME + PRICE + CATALYST

Integrate session timing, macro timing, liquidity, price location, and catalysts.

### PHASE 6 — BACKTEST

Freeze rules first, then measure:

- setup frequency;
- valid setup rate;
- reaction rate;
- entry conversion;
- stop/target behavior;
- R multiple;
- MAE/MFE;
- session distribution;
- catalyst distribution;
- HTF trend distribution;
- failure modes.

### PHASE 7 — AI INTERPRETATION

Only after deterministic evidence and state are trustworthy should AI provide explanations.

### PHASE 8 — UI POLISH

Cosmetic expansion comes last.

---

## 20. TARGET ARCHITECTURE

```text
MARKET DATA
     ↓
EVIDENCE LAYER
     ↓
VALIDATED EVIDENCE PACKET
     ↓
MARKET CONTEXT
     ↓
HTF STRUCTURE + TIME/CATALYST
     ↓
DEALING RANGE
     ↓
LIQUIDITY MAP
     ↓
QMR
     ↓
ABC
     ↓
REACTION
     ↓
ENTRY
     ↓
RISK / TARGET
     ↓
TRADE STATE
     ↓
CANONICAL RATING
     ↓
AI EXPLANATION
     ↓
MEMBER INTERFACE
```

Separate security architecture:

```text
PUBLIC USER
    ↓
AUTHENTICATION
    ↓
AUTHORIZATION
    ↓
MEMBERSHIP CHECK
    ↓
PROTECTED SERVER ROUTE
    ↓
PROTECTED DATA/API
    ↓
LIVE TRADE / RESEARCH
```

---

## 21. DEFINITION OF PRODUCTION READY

Production readiness requires:

### Security

- protected routes protected;
- protected APIs protected;
- membership authorization enforced server-side.

### Data

- no fabricated prices;
- evidence freshness enforced;
- fallback deterministic;
- unavailable data explicitly labeled.

### Trading logic

- QMR deterministic;
- ABC deterministic;
- invalidation deterministic;
- stale-state handling deterministic.

### Integration

- QMR + ABC harmonized;
- macro/time/catalyst integrated;
- canonical trade rating derived from authoritative state.

### User experience

- public/private boundary clear;
- members receive intended research;
- unauthorized users receive no protected information.

---

## 22. NON-NEGOTIABLE PRINCIPLES

1. **No evidence = no invented fact.**
2. **A pattern cannot override structure.**
3. **ABC does not replace QMR.**
4. **CISD is not mandatory for every ABC setup.**
5. **Liquidity must be contextual, not arbitrary.**
6. **Time + price + catalyst should eventually work together.**
7. **AI interprets evidence; it does not manufacture evidence.**
8. **Hiding a route is not securing a route.**
9. **Successful Vercel deployment does not prove runtime correctness.**
10. **Backtesting follows deterministic rule definition.**

---

## 23. NEXT SESSION STARTING POINT

Start here:

1. Open `https://hipmarvinfx.com/live-trades` in an incognito/logged-out browser.
2. Inspect its network requests.
3. Identify all live-trade/research APIs.
4. Test those APIs independently without authentication.
5. Fix any public exposure.
6. Then freeze the seven ABC deterministic rules.
7. Then integrate ABC into QMR.
8. Then add time + price + catalyst.
9. Then backtest.
10. Then expand AI interpretation.

**Do not restart the architecture. Continue from this state.**
