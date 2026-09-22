# HipMarvinFX — Current State & Priority Open Items

**Date:** 22 September 2026  
**Type:** Current engineering state / next-session priority handover  
**Application repository:** `HipmarvinFx/hipmarvinfx`  
**Documentation repository:** `HipmarvinFx/hipmarvinfx-docs`

> This document records the current verified working state as of 22 September 2026. It supersedes older priority assumptions where they conflict with current application inspection. It is a state/handover record, not a new methodology specification.

---

## 1. Current position

The evidence/calendar/central-bank foundation is substantially closed. The canonical evidence-packet documentation has been reconciled with the implementation. HTF/QMR and ABC V2 now have substantial deterministic implementation and tests.

The remaining priority work is **verification and targeted integration**, not a redesign of the trading methodology and not a rewrite of ABC V2.

---

## 2. CLOSED

### 2.1 Evidence foundation — CLOSED

Verified/implemented:

- Evidence statuses: VERIFIED, PENDING, STALE, INVALID, NOT_AVAILABLE.
- Evidence classifications: FACT, DERIVED, INTERPRETATION.
- Provenance and validation infrastructure.
- Yahoo Finance price evidence.
- Twelve Data integration.
- CFTC/COT evidence and HTML fallback.
- Finance Calendar adapter and canonical event normalization.
- Daily/Weekly calendar subject scoping.
- Phase 14 calendar/evidence normalization work.

### 2.2 Central-bank evidence — CLOSED

Five registered adapters are wired:

- Federal Reserve
- ECB
- Bank of England
- Bank of Japan
- Swiss National Bank

Daily/Weekly research passes the publication data through to AI context.

The unsafe legacy bank-level `policyDirection` / `currencyImplication` derivation was rejected.

### 2.3 Source registry — CLOSED for currently implemented sources

Registered/implemented sources include:

- Yahoo Finance
- Twelve Data
- CFTC
- Finance Calendar
- five central banks
- mock/test sources

General government statistical data, general news/RSS, and geopolitical feeds remain explicitly unimplemented rather than being silently substituted with unrestricted AI browsing.

### 2.4 Evidence packet reconciliation — CLOSED

The documentation now distinguishes:

- legacy `EvidencePacket` — FULL/DELTA and existing callers;
- canonical `CanonicalResearchEvidencePacket` — v7 research packet, FULL-only.

Canonical DELTA/base-packet linkage is not falsely represented as implemented.

### 2.5 HTF/QMR foundation — SUBSTANTIALLY CLOSED

Existing deterministic components include:

- HTF structure
- trend alignment
- structural break
- HTF composite
- QMR
- QM/QML refinement

QMR tests previously passed 10/10.

Locked methodology remains:

**Macro Regime → Weekly Thesis → HTF Trend/Structure → 20D Location → Liquidity → QMR → Reaction → QM/QML → Liquidity Target → Risk**

### 2.6 Dealing-range structural validity — SUBSTANTIALLY CLOSED

The dealing-range engine already supports structural validity/invalidation, including:

- valid range;
- structural-break prerequisites;
- INVALIDATED-BULLISH;
- INVALIDATED-BEARISH;
- refusal to interpret ABC from an invalid dealing range.

### 2.7 ABC V2 mechanical formation — SUBSTANTIALLY CLOSED

The current implementation contains:

- dealing-range prerequisite;
- boundary attack;
- B validation;
- B/X3 same-event semantics;
- post-B formation;
- retrospective A selection;
- C identification;
- C Premium/Discount zone;
- B/A/C structural relation;
- reaction confirmation;
- target-B structure;
- lifecycle states;
- structural invalid/not-applicable handling;
- dedicated tests.

The approved B/X3 decision is implemented with:

`const x3Idx = bIdx`

The obsolete independent `bIdx + 1` X3 search is removed.

---

## 3. PRIORITY OPEN ITEMS

These items must be **verified against the existing implementation before any patch is authorized**.

### Priority 1 — Deterministic Macro Regime construction

**Status: OPEN / not yet proven complete**

Macro evidence and `macroRegime` references exist in the evidence ledger and Daily/Weekly/AI layers. However, the latest targeted search did not identify a dedicated deterministic Macro Regime writer/builder.

Required next determination:

- What exact code constructs the Macro Regime?
- Which verified evidence feeds it?
- Is the result deterministic and provenance-backed?
- What happens when macro evidence is PENDING, STALE, INVALID, or NOT_AVAILABLE?

Do not label this CLOSED until the actual construction path is identified and verified.

### Priority 2 — Macro/HTF/QMR gating into ABC

**Status: OPEN / wiring verification**

ABC V2 is called from Daily/Weekly, but the inspected ABC function itself primarily evaluates structural ABC geometry. Its `htfStructure` input is not sufficient evidence that HTF/QMR gating is actually enforced.

Verify whether the caller already enforces:

- HTF direction;
- structural-break eligibility for countertrend;
- QMR phase;
- liquidity acceptance/rejection;
- aligned 4H structure;
- 1H execution minimum;
- timeframe alignment.

If these gates already exist upstream, close this item. If not, add only the missing deterministic gate.

### Priority 3 — **Zone-B Premium/Discount validation / invalidation**

**Status: OPEN / MUST REVIEW AGAINST BUILT CODE**

This is an established structural requirement and must not be forgotten.

B is not considered fully validated merely because price attacks the dealing-range boundary and closes back inside.

The intended rule is:

- **Bearish context:** B must satisfy the required Premium-side DR location for the intended bearish ABC scenario.
- **Bullish context:** B must satisfy the required Discount-side DR location for the intended bullish ABC scenario.
- A B formation on the wrong side of the DR can make the intended ABC scenario **unsatisfactory/invalid**, rather than merely being a descriptive location label.

First inspect the current ABC/dealing-range implementation and tests to determine whether this rule is already captured, partially captured, or absent.

**Do not invent a new threshold or overwrite existing logic without reconciling it with the established ABC decision/specification.**

### Priority 4 — **B-zone consequence for the protected DRH/DRL objective**

**Status: OPEN / MUST REVIEW AGAINST BUILT CODE**

The B-zone rule is not isolated location metadata. It is intended to affect whether the formation is structurally satisfactory for targeting the relevant protected point of interest:

- protected DRH / High in the applicable context;
- protected DRL / Low in the applicable context.

Verify whether the current implementation captures the complete chain:

**B location → satisfactory/unsatisfactory → validation/invalidation → protected DRH/DRL consequence**

If already implemented, close it with tests. If only partially implemented, patch only the missing deterministic piece.

### Priority 5 — ABC reaction confirmation strictness

**Status: OPEN / REVIEW ONLY**

Current ABC reaction confirmation uses a minimum body/range ratio plus directional close.

Review whether this is sufficient under the approved ABC specification and whether broader QMR/flow confirmation is enforced by the caller.

Do not strengthen this rule merely because it appears permissive. Any change requires an existing approved methodology/decision basis.

---

## 4. Explicitly NOT OPEN FOR REWORK

Do not reopen the following without a demonstrated defect or new explicit decision:

- central-bank adapter architecture;
- calendar normalization already completed in Phase 14;
- canonical evidence packet documentation reconciliation;
- B/X3 same-event decision;
- the basic ABC V2 formation geometry;
- dealing-range invalidation already implemented;
- v7 HTF-first/QMR methodology;
- 20D Premium/Discount as a standalone directional signal;
- unrestricted AI browsing as an evidence source.

---

## 5. Recommended next engineering action

Perform one targeted code review covering only:

1. `lib/engine/evidence-ledger.ts`
2. the ABC invocation/gating sections of `lib/pipeline/daily.ts`
3. the ABC invocation/gating sections of `lib/pipeline/weekly.ts`
4. the relevant ABC/dealing-range tests for B location and protected DRH/DRL behavior.

The review must answer four questions:

1. **How is Macro Regime actually constructed?**
2. **What upstream HTF/QMR conditions are actually enforced before ABC is accepted?**
3. **Does B Premium/Discount validation already exist?**
4. **Does B-zone validity determine the appropriate protected DRH/DRL consequence?**

Only after those four questions are answered should implementation changes be made.

---

## 6. Operating rule for the next session

> **Inspect first. Patch only what is demonstrably missing.**

The project is being hardened, not redesigned.

No invented market facts. No speculative thresholds. No duplicate engines. No reopening closed documentation work.
