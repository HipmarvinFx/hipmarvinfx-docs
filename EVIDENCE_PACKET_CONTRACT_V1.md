# HipMarvinFX Evidence Packet Contract v1

**Status:** Canonical v7 foundation contract  
**Implementation status:** Contract only  
**Date:** 28 August 2026

## 1. Purpose

The Evidence Packet is the controlled context boundary between deterministic HipMarvinFX engines and AI interpretation.

Its purpose is to minimize AI context while ensuring that every factual and derived claim available to AI is traceable to validated evidence.

> **AI receives evidence packets, not raw research inputs.**

## 2. Architectural position

```text
Source Data
   ↓
Validation
   ↓
Evidence Store
   ↓
Deterministic Engines
   ↓
Evidence Packet
   ↓
AI
   ↓
Parser Firewall
```

## 3. Packet modes

The packet MUST support:

```text
FULL
DELTA
```

### FULL
Contains the complete current evidence context required for a new research cycle or recovery from missing prior context.

### DELTA
Contains only evidence that changed, became stale, became unavailable, or otherwise materially changed since the relevant prior packet.

The system MUST retain enough metadata to establish which prior packet a DELTA applies to.

## 4. Required packet envelope

Conceptually:

```text
EvidencePacket
├── packet_id
├── packet_version
├── mode
├── generated_at
├── base_packet_id (DELTA only)
├── data_health
├── facts
├── derived
├── technical_context
├── macro_context
├── candidates
└── change_summary
```

Exact serialization may be JSON or another internal representation, but the semantic contract MUST remain stable.

## 5. Data health

The packet MUST expose the health of required evidence classes.

```text
VERIFIED
PENDING
STALE
INVALID
NOT_AVAILABLE
```

Critical unavailable evidence MUST be explicit.

The packet MUST NOT silently omit a required evidence class and allow AI to assume that it was checked.

## 6. FACT section

Facts are source-reported values that passed validation.

Examples:

```text
price
COT published positions
calendar actual
calendar forecast
calendar previous
central-bank decision
official macro release
```

Every fact MUST retain provenance or a resolvable evidence identifier.

## 7. DERIVED section

Derived values are deterministic outputs from verified facts.

Examples:

```text
20D location
currency strength
macro surprise
macro regime
policy direction
liquidity state
flow regime
HTF structure state
pair candidate ranking
```

Every derived value MUST identify its input evidence or derivation version sufficiently for reproducibility.

## 8. Technical context

Where required by the v7 methodology, the packet may include deterministic outputs for:

```text
HTF Trend
Trend Alignment
Structural Break
20D Location
Liquidity
Flow Regime
QMR Phase
QM/QML refinement
Liquidity Target
Execution timeframe eligibility
```

AI must consume these outputs rather than independently calculating them from raw data.

## 9. Macro context

The packet may include deterministic macro outputs including:

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

The detailed calculation rules are defined in `MACRO_ENGINE_SPEC_V1.md`.

## 10. Candidate section

Pair candidates MUST be supplied as deterministic engine outputs with the evidence supporting eligibility.

The AI may explain or articulate a candidate but MUST NOT create a candidate solely because it appears plausible.

The v7 principle remains:

> Never exclude a cross because it is a cross, and never fade a trend merely because it is in Premium.

## 11. Change summary

A DELTA packet SHOULD state:

```text
NEW
CHANGED
UNCHANGED
STALE
REMOVED
NOT_AVAILABLE
```

For unchanged evidence, a compact marker is preferred to resending the complete evidence payload.

## 12. Token minimization

The packet builder SHOULD minimize context by:

1. deduplicating repeated facts;
2. avoiding raw provider payloads;
3. omitting unchanged evidence in DELTA mode;
4. referencing stable evidence IDs where appropriate;
5. summarizing deterministic engine outputs;
6. retaining provenance without reproducing source documents.

Token minimization MUST NOT remove information necessary to verify a claim.

## 13. AI restrictions

AI MUST NOT treat packet absence as permission to invent.

If an item is:

```text
PENDING
STALE
INVALID
NOT_AVAILABLE
```

AI MUST state or respect that limitation according to the research contract.

AI MUST NOT convert an INTERPRETATION into a FACT.

## 14. Determinism

For identical validated evidence, engine version, and packet configuration, the resulting derived packet SHOULD be deterministic.

A packet SHOULD expose a stable hash or equivalent integrity identifier.

## 15. Provenance

Every claim-bearing fact or derived output MUST be traceable through:

```text
Evidence ID
Source
Provider
Source Reference
Retrieved At
Effective At
```

Derived outputs additionally SHOULD identify the engine/rule version used.

## 16. Parser compatibility

The parser receives:

```text
AI output
+
Evidence Packet
```

It validates factual claims against the packet and deterministic methodology rules.

A valid packet does not make AI output automatically valid.

## 17. Manual fallback

Manual evidence may appear in a packet only with explicit MANUAL provenance and applicable validation status.

## 18. Security

The packet MUST NOT contain:

- API keys;
- access tokens;
- service-role keys;
- cron secrets;
- authorization headers;
- private credentials.

## 19. Versioning

Breaking changes to the packet schema require a new contract version.

Backward-compatible additions may use the existing version only when older consumers can safely ignore them.

## 20. Non-goals

The Evidence Packet is not:

- a raw database dump;
- a provider response archive;
- an AI memory store;
- a substitute for the Evidence Store;
- a place for AI-generated facts.
