# HipMarvinFX Evidence Source Registry v1

**Status:** Canonical v7 foundation contract  
**Implementation status:** Contract only  
**Date:** 28 August 2026

## 1. Purpose

The Source Registry defines how HipMarvinFX identifies, evaluates, and uses external evidence providers without coupling the rest of the system to a specific vendor.

> **Providers are replaceable. Evidence contracts are not.**

No provider named in an implementation plan is automatically an architectural requirement.


## Current Approved / Implemented Source Registry

The following sources are confirmed against the current v7 implementation.
This table describes what is actually wired today; it is not a declaration
that every source category in the architecture is already implemented.

| Evidence class | Source / adapter | Provider | Authority | Priority | Implementation status |
|---|---|---|---|---:|---|
| MARKET_PRICE | `yahoo-fx` | Yahoo Finance | MARKET_DATA_VENDOR | 0 | IMPLEMENTED |
| MARKET_PRICE | `twelve-data` | Twelve Data API | MARKET_DATA_VENDOR | 1 | IMPLEMENTED / FALLBACK |
| COT | `cftc-cot` | CFTC Public Reporting Environment (Socrata) | PRIMARY_OFFICIAL | 2 | IMPLEMENTED |
| COT | `cftc-cot-html-fallback` | CFTC TFF short-form HTML report | PRIMARY_OFFICIAL | 3 | IMPLEMENTED / FALLBACK |
| ECONOMIC_CALENDAR | `finance-calendar` | FinanceCalendar | SECONDARY_AGGREGATOR | 10 | IMPLEMENTED |
| CENTRAL_BANK | `fed-rss` | Federal Reserve official RSS | PRIMARY_OFFICIAL | 0 | IMPLEMENTED |
| CENTRAL_BANK | `ecb-rss` | European Central Bank official RSS | PRIMARY_OFFICIAL | 1 | IMPLEMENTED |
| CENTRAL_BANK | `boe-rss` | Bank of England official RSS | PRIMARY_OFFICIAL | 2 | IMPLEMENTED |
| CENTRAL_BANK | `boj-mpm` | Bank of Japan official MPM statement index | PRIMARY_OFFICIAL | 3 | IMPLEMENTED |
| CENTRAL_BANK | `snb-rss` | Swiss National Bank official RSS | PRIMARY_OFFICIAL | 4 | IMPLEMENTED |
| TEST_ONLY | `mock` | Internal mock adapter | MANUAL | — | TEST ONLY — NOT A PRODUCTION SOURCE |

### Confirmed source references

- Twelve Data: `https://api.twelvedata.com/time_series`
- CFTC Socrata TFF: `https://publicreporting.cftc.gov/resource/gpe5-46if.json`
- CFTC TFF HTML fallback: `https://www.cftc.gov/dea/futures/financial_lf.htm`
- ECB official press RSS: `https://www.ecb.europa.eu/rss/press.xml`
- Bank of England official news RSS: `https://www.bankofengland.co.uk/rss/news`
- Bank of Japan official MPM index: `https://www.boj.or.jp/en/mopo/mpmdeci/state_<year>/index.htm`
- Swiss National Bank official monetary-policy RSS: `https://www.snb.ch/public/rss/en/mopo`
- FinanceCalendar source reference is the adapter's `FINANCE_CALENDAR_BASE`; the adapter also exposes `https://www.financecalendar.com` as its attribution source.
- Yahoo Finance source reference is emitted by the Yahoo adapter and remains provider-specific; this registry does not invent a URL not explicitly exposed by the adapter metadata.

### Source-category implementation boundary

The architecture defines additional source classes that are **not yet general production adapters**:

- `GOVERNMENT_MACRO` / official economic-statistical data: **NOT YET IMPLEMENTED as a general adapter class**.
- `NEWS`: **NOT YET IMPLEMENTED as a general news adapter**.
- `GEOPOLITICAL`: **NOT YET IMPLEMENTED as a general adapter**.
- `MANUAL`: supported as an evidence class by contract, but is not an unrestricted external-provider substitute.

Central-bank RSS feeds are approved only for the `CENTRAL_BANK` evidence class represented by their adapters. Their existence does **not** authorize unrestricted news/RSS ingestion.

### News and RSS restriction

HipMarvinFX must not give the AI unrestricted web/news browsing authority as an evidence mechanism.

Any future news or RSS source must first be:

1. explicitly registered;
2. assigned a source ID and authority class;
3. assigned a defined source reference;
4. normalized through an adapter;
5. validated and provenance-stamped;
6. subject to the same freshness, failure, and conflict rules as other evidence.

Unregistered websites, search results, arbitrary URLs, and ad-hoc AI browsing are **not evidence sources**.

### Priority and fallback interpretation

Adapter priority is deterministic and ascending. A lower numeric priority is attempted before a higher numeric priority for the same source type.

Current production fallback relationships include:

- Yahoo Finance → Twelve Data for market-price retrieval.
- CFTC Socrata → CFTC TFF HTML for COT retrieval.

The internal mock adapter is test-only. It was removed from the production registry on 2 October 2026 (app-repo commit `226b80f`) and must never be treated as a production evidence source.

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

## 13. Implementation Status and Scope

The original registry contract was written before the current central-bank
adapter layer was implemented. That historical wording is superseded.

The current v7 implementation has production adapters for:

- market prices;
- CFTC/COT;
- economic calendar;
- five central banks: Fed, ECB, BoE, BoJ, and SNB.

The registry therefore treats those sources as implemented evidence paths.

Official government economic/statistical data outside the central-bank layer,
general news/RSS, and geopolitical sources remain future source categories
until explicit adapters are implemented and validated. They must not be
represented as currently available evidence merely because the architecture
allows for them.

The registry remains provider-agnostic at the EvidenceItem boundary. Adding
a provider requires an explicit registry/adapter change rather than an AI
decision to browse or select a source dynamically.
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
