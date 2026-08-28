# HipMarvinFX Evidence Source Registry v1

**Status:** Canonical v7 foundation contract  
**Implementation status:** Contract only  
**Date:** 28 August 2026

## 1. Purpose

The Source Registry defines how HipMarvinFX identifies, evaluates, and uses external evidence providers without coupling the rest of the system to a specific vendor.

> **Providers are replaceable. Evidence contracts are not.**

No provider named in an implementation plan is automatically an architectural requirement.

## 2. Registry responsibilities

The registry MUST define, for every source adapter:

```text
source_id
provider_id
evidence_types
authority_class
update_frequency
freshness_policy
coverage
availability
authentication_requirement
validation_policy
fallback_policy
source_reference_format
```

The registry is configuration/contract metadata. It is not the place for AI reasoning.

## 3. Source classes

The initial registry MUST support, without requiring any specific vendor:

```text
MARKET_PRICE
COT
ECONOMIC_CALENDAR
CENTRAL_BANK
GOVERNMENT_MACRO
NEWS
GEOPOLITICAL
MANUAL
```

Additional classes may be introduced only through an explicit contract update.

## 4. Authority model

Each source MUST have an authority classification appropriate to the evidence type.

Suggested classes:

```text
PRIMARY_OFFICIAL
SECONDARY_AUTHORITATIVE
SECONDARY_AGGREGATOR
MARKET_DATA_VENDOR
NEWS_OR_MEDIA
MANUAL
```

Authority is evidence-type-specific. A source authoritative for one evidence type MUST NOT automatically be treated as authoritative for another.

## 5. Provider selection rules

A provider may be selected for implementation only after evaluation against:

- authority;
- accuracy;
- availability;
- freshness;
- rate limits;
- cost;
- licensing/usage rights;
- historical coverage;
- reproducibility;
- failure behavior;
- machine-readable access;
- ability to expose a stable source reference.

Provider selection MUST NOT change the normalized EvidenceItem schema.

## 6. Adapter contract

Every external adapter MUST conceptually implement:

```text
fetch(request)
    → raw provider response
    → normalize(response)
    → EvidenceItem[]
    → validate(EvidenceItem[])
```

Adapters MUST NOT return provider-specific objects to the AI layer.

Adapters MUST NOT call an LLM for extraction, validation, or interpretation.

## 7. Provenance requirements

Every normalized EvidenceItem MUST identify:

```text
source
provider
source_reference
retrieved_at
effective_at
```

The adapter SHOULD preserve publication/revision metadata where available.

No provenance field may contain secrets or authorization material.

## 8. Freshness policy

Freshness is evidence-type-specific.

Examples:

```text
LIVE / NEAR-LIVE PRICE
→ short freshness window

ECONOMIC EVENT ACTUAL
→ remains historically valid after publication, but event recency must be explicit

COT
→ weekly publication cadence; stale status determined relative to the latest applicable report

CENTRAL BANK POLICY
→ event/version based; previous policy remains historically valid but must not be represented as current without a currentness check
```

Exact thresholds belong to the implementation and evidence-specific validation rules, not to AI prompts.

## 9. Source conflict policy

When multiple providers report the same fact, the system MUST NOT silently choose whichever value is convenient.

Conflict handling MUST be deterministic and registry-defined.

Possible outcomes:

```text
PRIMARY_ACCEPTED
SECONDARY_CONFIRMS
SECONDARY_DISAGREES
CONFLICT_REQUIRES_REVIEW
```

A material unresolved conflict MUST prevent the affected evidence from being represented as unqualified VERIFIED data.

## 10. Fallback policy

Fallback sources may improve availability but MUST NOT silently change evidence authority.

Example:

```text
Primary source unavailable
        ↓
Approved fallback attempted
        ↓
Fallback provenance retained
        ↓
Evidence status reflects fallback source
```

The system MUST never present fallback evidence as if it came from the primary provider.

## 11. Manual source

Manual evidence is a valid fallback class when explicitly required.

It MUST include:

```text
source = MANUAL
operator/reference information where appropriate
retrieved_at
entered/effective time
```

Manual input remains subject to validation and parser rules.

## 12. Source health

The implementation SHOULD track:

```text
last_success
last_failure
failure_count
latency
rate_limit_state
last_verified_evidence
```

Source health MUST be visible to operational diagnostics but MUST NOT be fabricated when monitoring data is unavailable.

## 13. Initial registry scope

The first vertical slice requires only abstract capabilities for:

```text
PRICE
COT
ECONOMIC_CALENDAR
```

Price may use the existing v6.1 providers during migration. COT and calendar may use deterministic test fixtures before production adapters are selected.

Central-bank, government-macro, news, and geopolitical adapters are later phases and must not delay the first architecture proof.

## 14. Provider-agnostic AI boundary

The AI layer receives normalized evidence, not provider-specific API responses.

For example, AI may receive:

```text
source: PRIMARY_OFFICIAL
provider: <registered provider>
status: VERIFIED
value: ...
retrieved_at: ...
```

It must not be responsible for deciding whether a provider is trustworthy.

## 15. Security

Provider credentials MUST remain outside source control and outside EvidenceItem payloads.

The registry MUST never contain API keys, access tokens, service-role keys, cron secrets, or private credentials.

## 16. Change control

Adding, replacing, or retiring a production source requires:

1. registry entry;
2. adapter contract compliance;
3. validation rules;
4. provenance test;
5. failure/fallback test;
6. evidence fixture coverage;
7. approval before production use.

## 17. Non-goals

This contract does NOT:

- select a final commercial provider;
- define macro scoring;
- define AI prompts;
- define parser rules;
- define database implementation details.

Those belong to companion contracts.
