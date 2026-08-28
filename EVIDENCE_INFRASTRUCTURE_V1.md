# HipMarvinFX Evidence Infrastructure v1

**Status:** Canonical v7 foundation contract  
**Implementation status:** Contract only — application implementation is blocked until this contract set is approved.  
**Date:** 28 August 2026

## 1. Purpose

The Evidence Infrastructure is the trusted fact layer beneath HipMarvinFX v7. Its purpose is to establish market reality deterministically before AI interpretation.

> **SYSTEM ESTABLISHES REALITY → AI EXPLAINS REALITY.**

The Evidence Layer MUST NOT depend on an LLM to establish facts, invent missing values, repair unsupported values, or select a provider-specific interpretation.

## 2. Architectural position

```text
External Sources
      ↓
Source Adapters
      ↓
Normalization
      ↓
Evidence Validation
      ↓
Evidence Store
      ↓
Deterministic Engines
      ↓
Evidence Packet
      ↓
AI Interpretation
      ↓
v7 Parser / Firewall
      ↓
Publication
```

The existing v6.1 pipeline remains operational during migration. Existing price verification remains active until the v7 firewall has demonstrated equivalent or stronger protection.

## 3. Evidence classes

Every item MUST have one primary semantic class:

### FACT
Directly reported by an approved source.

Examples: OHLC, COT published position, CPI actual, central-bank rate decision.

### DERIVED
Deterministically calculated from verified facts.

Examples: 20D location, currency-strength rank, macro surprise, flow regime.

### INTERPRETATION
Human/AI reasoning applied to verified facts and derived outputs.

Examples: weekly thesis, scenario, narrative explanation.

AI MUST NOT create FACT or DERIVED values that were not supplied by the Evidence Layer.

## 4. Evidence status

```text
VERIFIED
PENDING
STALE
INVALID
NOT_AVAILABLE
```

### VERIFIED
Evidence passed all applicable validation rules and has sufficient provenance.

### PENDING
Evidence is expected but has not yet been obtained or confirmed.

### STALE
Evidence exists but is outside its permitted freshness window for the requested use.

### INVALID
Evidence failed validation or contains contradictory/impossible values.

### NOT_AVAILABLE
The required source/evidence is unavailable and no verified substitute exists.

AI MUST receive the status explicitly. Missing evidence MUST NOT be silently converted into a guessed value.

## 5. Minimum provenance

Every persisted EvidenceItem MUST contain:

```text
source
provider
source_reference
retrieved_at
effective_at
```

Where applicable it SHOULD also contain:

```text
published_at
coverage_start
coverage_end
revision
content_hash
```

`source_reference` MUST be sufficient to identify the originating document, endpoint, report, event, or record without exposing credentials.

## 6. Source authority

Sources are classified by authority and capability in the Source Registry contract. The Evidence Layer MUST remain provider-agnostic.

No provider is architecturally mandatory merely because it is used during an initial implementation.

Provider substitution MUST NOT require changes to AI prompts, parser rules, or publication schemas.

## 7. Validation principles

Validation MUST be deterministic.

Minimum rules include:

- required fields present;
- correct type and units;
- timestamps valid;
- freshness within evidence-specific limits;
- internally consistent numerical relationships;
- source provenance present;
- duplicate/version handling deterministic;
- conflicting sources resolved by explicit registry policy;
- no unsupported inference during validation.

For prices, existing v6.1 sanity checks remain mandatory during migration, including null checks, high/low consistency, close-range checks, minimum candle history, staleness checks, and cross-provider deviation checks where applicable.

## 8. Persistent Evidence Store

The v7 Evidence Store is the durable source of validated evidence history.

It MUST support:

1. immutable or versioned evidence history;
2. provenance retention;
3. status retention;
4. deterministic identity/deduplication;
5. retrieval by evidence type, instrument/currency, effective time and source;
6. comparison with prior evidence;
7. construction of FULL and DELTA Evidence Packets;
8. auditability of what the AI was given.

The existing `lib/engine/evidence-ledger.ts` MUST be migrated rather than deleted first. It remains a compatibility component until its responsibilities are replaced and regression-tested.

## 9. Deterministic engine boundary

Technical and macro engines consume VERIFIED facts and produce DERIVED outputs.

They MUST NOT fetch arbitrary external information directly.

They MUST NOT use an LLM to calculate:

- price;
- 20D location;
- currency strength;
- liquidity state;
- flow regime;
- macro surprise;
- COT positioning calculations;
- pair rankings;
- structural gates.

Same verified input MUST produce the same derived output.

## 10. AI boundary

AI receives an Evidence Packet and performs interpretation only.

AI MAY:

- explain verified facts;
- synthesize derived outputs;
- rank or articulate scenarios when the contract permits;
- produce editorial research text.

AI MUST NOT:

- invent current prices;
- invent COT values;
- invent calendar actuals/forecasts/previous values;
- invent central-bank facts;
- invent unsupported macro statistics;
- invent technical levels presented as verified facts;
- silently upgrade PENDING/STALE/NOT_AVAILABLE evidence to VERIFIED;
- override deterministic structural gates.

## 11. Manual fallback

Manual evidence remains supported when automated evidence is unavailable.

Manual evidence MUST be explicitly tagged with provenance indicating `MANUAL` and MUST pass the same parser/evidence checks applicable to the claim.

Manual fallback is not permission to fabricate values.

## 12. Failure behavior

If required evidence cannot be established:

```text
Source failure
    ↓
PENDING / NOT_AVAILABLE
    ↓
Do not invent
    ↓
Continue only if the research contract permits incomplete evidence
    OR
Block publication
```

Critical evidence failures MUST result in a blocked or degraded state that is explicit to the caller and admin UI.

## 13. Security

Secrets MUST remain outside source control.

API keys, service-role keys, cron secrets and provider credentials MUST be supplied through local environment configuration and/or deployment secret stores.

Evidence provenance MUST never contain secret values, authorization headers, tokens, or private credentials.

## 14. Migration rule

v7 is a parallel architectural upgrade, not an emergency rewrite.

The migration sequence is:

```text
v6.1 stable
   ↓
Evidence Layer in isolation
   ↓
Vertical-slice validation
   ↓
Shadow mode
   ↓
Controlled v7 cutover
   ↓
Retire replaced v6.1 components only after regression proof
```

## 15. Non-goals

This contract does NOT:

- mandate a particular vendor;
- define the detailed source registry;
- define the Evidence Packet schema;
- define the macro scoring algorithm;
- replace the v7 trading doctrine;
- authorize application implementation before the contract set is approved.

Those responsibilities are defined by the companion contracts.
