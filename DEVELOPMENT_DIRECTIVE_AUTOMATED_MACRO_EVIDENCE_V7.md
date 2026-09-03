# HIPMARVINFX DEVELOPMENT DIRECTIVE
## Restore Automated Macro Evidence Ingestion — v7 Evidence Architecture

**Date:** 2026-09-03  
**Priority:** HIGH  
**Applies to:** v7 Evidence Infrastructure / Macro Engine / Research Pipeline

---

## 1. PURPOSE

The development team must correct the current assumption that HipMarvinFX requires the user to manually provide COT data, central-bank information, RSS/news information, and other publicly available macro evidence.

The original `HipMarvinFX_Roadmap_and_Handover_v6.2.md` explicitly specified automated ingestion for:

- COT → CFTC API
- Central-bank policy/speeches → RSS
- Economic calendar → machine-readable calendar source
- Inflation → TradingEconomics / OECD / FRED
- News → NewsAPI / EventRegistry

The handover's Phase 2 acceptance criteria explicitly required the evidence ledger to contain real COT, central-bank, news, calendar and inflation data, with `N/A` used only where data is genuinely unavailable.

Therefore:

> **The problem is implementation status, not data availability.**

Do NOT redesign HipMarvinFX around permanent manual screenshots or AI-supplied macro facts.

---

## 2. CORE ARCHITECTURAL RULE

The AI must NEVER be the origin of a factual macro value when that value can be obtained from a trusted machine-readable source.

The required architecture is:

```text
SOURCE
→ EVIDENCE ADAPTER
→ RAW EVIDENCE
→ VALIDATION
→ EVIDENCE STORE
→ DETERMINISTIC DERIVATION
→ EVIDENCE PACKET
→ AI INTERPRETATION
→ PARSER / VALIDATION
→ PUBLICATION
```

The AI is an interpreter of evidence, not the source of evidence.

---

## 3. COT INGESTION — BUILD THIS

### Source

Use the official CFTC Commitment of Traders data/API or official machine-readable CFTC publication endpoint.

Do NOT ask the AI to reconstruct COT numbers.

Do NOT use manually pasted COT figures as the primary architecture.

### Required adapter

Create/complete:

`lib/data/cot.ts`

The adapter must:

1. Fetch the relevant CFTC dataset.
2. Identify the reporting date.
3. Identify the relevant currency/futures contract.
4. Extract, where published:
   - Leveraged Funds
   - Asset Managers
   - Long
   - Short
   - Net
   - Week-over-week change
5. Preserve the original source reference.
6. Preserve retrieval timestamp.
7. Validate numerical fields.
8. Return structured evidence.
9. Mark unavailable/failed records explicitly.

The AI may subsequently interpret the positioning. It must NOT invent or alter the underlying figures.

---

## 4. CENTRAL-BANK INGESTION — BUILD THIS

Use official central-bank RSS feeds and/or official machine-readable publication endpoints wherever available.

Create/complete:

`lib/data/central-banks.ts`

Initial institutions should include the G10 central banks relevant to HipMarvinFX:

- Federal Reserve
- ECB
- Bank of England
- Bank of Japan
- Swiss National Bank
- Bank of Canada
- Reserve Bank of Australia
- Reserve Bank of New Zealand
- plus CBN where relevant to the project's African/Nigerian macro context

Capture, where available:

- institution
- publication type
- title
- publication date
- source URL/reference
- raw content or permitted excerpt
- retrieval timestamp
- status

Relevant publication types include rate decisions, monetary-policy statements, speeches, minutes, press conferences and official guidance.

The publication itself is evidence. The AI's explanation of what the publication means for FX is interpretation.

---

## 5. CBN INGESTION — DO NOT TREAT IT AS MANUAL ONLY

CBN information must be incorporated into the same evidence architecture.

Use official CBN machine-readable/publication sources wherever available, including official RSS/publication feeds.

Do not assume that RSS means every numerical series is directly available through RSS.

The CBN adapter should support, where technically available:

1. RSS/publication discovery
2. Official downloadable datasets
3. Official statistical publications
4. Machine-readable endpoints

Every item must record the exact source.

If a specific CBN numerical series cannot be obtained automatically, return:

`NOT_AVAILABLE`

Do NOT fabricate it.

---

## 6. ECONOMIC CALENDAR — REDUCE SCREENSHOT DEPENDENCY

Implement the original planned calendar layer:

`lib/data/economic-calendar.ts`

Investigate the best reliable machine-readable source available for:

- event
- country/currency
- timestamp
- importance
- forecast
- previous
- actual
- revision, where available

The system must preserve provenance.

Manual screenshot input may remain as an emergency/manual-validation path, but it must NOT remain the only architecture.

If a field is unavailable, return `NOT_AVAILABLE`; never manufacture a value because the template expects one.

---

## 7. NEWS / RSS INGESTION

Implement the original planned news layer:

`lib/data/news.ts`

Candidate architecture:

```text
Official RSS
+
NewsAPI/EventRegistry where appropriate
+
Central-bank RSS
+
CBN RSS
```

Capture, where available:

- headline
- source
- publication time
- URL
- country/currency relevance
- event/category
- retrieval timestamp
- status

The AI can classify the evidence (for example hawkish/dovish or currency-positive/negative), but the underlying headline and timestamp must originate from the evidence layer.

---

## 8. EVIDENCE STATUS MODEL

All macro evidence must use explicit states. At minimum:

```text
VERIFIED
PENDING
STALE
NOT_AVAILABLE
INVALID
```

Never silently convert `PENDING` into `VERIFIED`.

Never allow an AI-generated value to enter the evidence store as factual source data.

---

## 9. SOURCE PROVENANCE IS MANDATORY

Every evidence record must contain enough provenance to answer:

> "Where did this number/fact come from?"

Minimum fields should include, as applicable:

```text
source
source_type
source_url/reference
retrieved_at
published_at
effective_date
status
raw_value
normalized_value
unit
```

Where appropriate also include:

```text
dataset
country
currency
instrument
report_date
```

---

## 10. DETERMINISTIC DERIVATION

Once raw evidence is stored, calculations must happen outside the AI.

Examples:

### COT

```text
Long - Short = Net Position
```

### Week-over-week

```text
Current Net - Previous Net = Weekly Change
```

### Macro regime

```text
Raw evidence
→ deterministic scoring rules
→ regime score
```

### Calendar

```text
Event data
→ event importance
→ scenario trigger
→ affected currency
```

The AI receives the resulting evidence packet and explains it.

---

## 11. EVIDENCE PACKET

Build the v7 Evidence Packet as the controlled interface between data infrastructure and AI.

Conceptually:

```text
EVIDENCE PACKET
├── Market Data
├── COT
├── Central Banks
├── Economic Calendar
├── Inflation
├── News
├── CBN
├── Currency Strength
├── 20D Location
├── Liquidity
├── Flow Regime
└── Derived Macro Regime
```

Every item must retain:

```text
FACT / DERIVED / INTERPRETATION
+
VERIFIED / PENDING / STALE / NOT_AVAILABLE / INVALID
```

The AI should receive the packet, not independently invent the underlying facts.

---

## 12. MANUAL INPUT POLICY

Manual screenshots/pastes are NOT to be deleted immediately.

They become:

### FALLBACK / OVERRIDE / VALIDATION INPUT

rather than the primary data architecture.

Required behaviour:

```text
AUTOMATED SOURCE
       ↓
successful
       ↓
use automatically

AUTOMATED SOURCE
       ↓
failed/unavailable
       ↓
PENDING / NOT_AVAILABLE
       ↓
optional manual evidence
       ↓
validate
       ↓
promote if valid
```

This preserves operational resilience without making manual work mandatory.

---

## 13. ANTI-FABRICATION RULE

This is non-negotiable.

If the system cannot obtain a factual value from an approved source:

`NOT_AVAILABLE`

If the source is too old:

`STALE`

If the source fails validation:

`INVALID`

If ingestion has not completed:

`PENDING`

The system must NEVER instruct the AI to fill in a missing value.

---

## 14. IMPLEMENTATION ORDER

### Phase A — Evidence foundation

1. Evidence Source Registry
2. Evidence Packet Contract
3. Evidence status enums
4. Evidence storage schema
5. Provenance fields
6. Validation layer

### Phase B — Data adapters

1. CFTC COT
2. Central-bank RSS
3. CBN sources
4. Economic calendar
5. Inflation
6. News/RSS

### Phase C — Deterministic engines

1. COT calculations
2. Currency-relative implications
3. Macro regime scoring
4. Macro driver extraction
5. Scenario Matrix inputs

### Phase D — AI integration

Feed the validated Evidence Packet into the AI.

The AI produces:

- Weekly Thesis
- Macro Drivers
- Scenario Matrix
- COT Read
- Pair-level translation
- Trade reasoning

It does NOT originate the factual inputs.

### Phase E — Parser / publication

```text
AI output
→ v7 parser
→ validation
→ structured DB
→ publication
```

---

## 15. ACCEPTANCE CRITERIA

### COT

- CFTC data fetched automatically.
- COT figures stored with provenance.
- Leveraged Funds and Asset Managers available where published.
- Net positioning calculated deterministically.
- No AI-generated COT numbers.

### Central Banks

- Official RSS/publication feeds automatically monitored.
- New publications ingested.
- Publication timestamp and source retained.
- AI interpretation references stored evidence.

### CBN

- Official CBN publication/RSS sources integrated where technically available.
- Numerical datasets use their actual source.
- Unsupported/unavailable series return `NOT_AVAILABLE`.

### News

- RSS/API news automatically ingested.
- Source and timestamp retained.
- AI classification operates on stored evidence.

### Calendar

- Events automatically ingested.
- Forecast/previous/actual retained when available.
- Missing fields remain explicitly unavailable.

### Evidence Layer

- Every factual value has provenance.
- Every value has a status.
- No fabricated values can enter the verified evidence store.
- AI cannot promote its own guesses into FACT evidence.

---

## 16. IMPORTANT CORRECTION TO CURRENT DEVELOPMENT UNDERSTANDING

The team should NOT describe HipMarvinFX as:

> "A system where the user must provide COT, central-bank and macro data."

That describes the current unfinished implementation, not the intended architecture.

The correct description is:

> **HipMarvinFX is being built as a deterministic evidence-driven research system in which trusted external data sources are automatically ingested, validated and stored before AI interpretation. Manual inputs are fallback/override mechanisms, not the primary evidence architecture.**

The original v6.2 handover already established this direction. Phase 2 was never completed.

---

## 17. FINAL DEVELOPMENT DIRECTIVE

Do not solve the macro-data problem by making the AI "better at researching."

Solve it by building the **Evidence Infrastructure**.

The fundamental rule is:

> **SOURCE THE FACT → VALIDATE THE FACT → STORE THE FACT → DERIVE THE METRIC → LET AI INTERPRET THE EVIDENCE.**

This is the required path for COT, CBN, central banks, RSS, news, calendar and other macro inputs.

No fabrication.
No AI-generated factual placeholders.
No permanent screenshot dependency.
No silent fallback to invented values.

Manual evidence remains available only as a controlled fallback when an approved automated source is unavailable.
