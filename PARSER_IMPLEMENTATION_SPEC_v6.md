# HipMarvinFX Parser Implementation Specification v6

**Status:** Canonical implementation specification  
**Version:** 6.0  
**Date:** 2026-08-19

## 1. Purpose

Define the implementation boundary for the v6 research parser. The parser converts canonical weekly/daily research Markdown into validated normalized data consumed by the publication and website layers.

It is a **normalizer and validator**, not an analyst.

Canonical upstream contracts:

- `STANDING_PROTOCOL_v6.md`
- `WEEKLY_RESEARCH_TEMPLATE_v4.md`
- `DAILY_UPDATE_TEMPLATE_v3.md`
- `POSITION_LEDGER_TEMPLATE_v2.md`
- `WEBSITE_PARSER_CONTRACT_v6.md`
- `WEBSITE_PAGE_SCHEMA_v6.md`
- `PUBLISHING_PIPELINE.md` v1.9

## 2. Processing pipeline

```text
Markdown source
    ↓
Document identification
    ↓
Section extraction
    ↓
Field normalization
    ↓
v6 schema validation
    ↓
Business-rule validation
    ↓
Normalized JSON
    ↓
Publication derivation
```

Every stage must be deterministic. The parser must not call an LLM to infer missing analytical fields.

## 3. Input handling

Supported source classes:

- Weekly Research
- Daily Update
- Position Ledger

The parser must identify the source template/version from explicit metadata where available.

If the document is v5 or lacks v6 fields, it remains parseable. Missing v6 flow fields become `NOT_RECORDED`; they are never inferred from 20D location.

## 4. Canonical normalized envelope

```json
{
  "schema_version": "6.0",
  "source": {
    "file": "",
    "template": "",
    "template_version": "",
    "protocol": "",
    "parsed_at": ""
  },
  "weekly": {},
  "trades": [],
  "daily_updates": [],
  "ledger": [],
  "validation": {
    "valid": true,
    "warnings": [],
    "errors": []
  }
}
```

`parsed_at` is metadata only and must never influence trading fields.

## 5. Trade normalization

Each Trade Priority List entry must normalize to the fields defined in `WEBSITE_PARSER_CONTRACT_v6.md`.

### Required

- pair
- direction
- priority, where supplied
- entry
- stop for executable ideas
- tp1
- rr
- timeframe
- correlation_class
- daily_zone.percent when supplied numerically
- daily_zone.label
- tier
- macro_regime
- macro_alignment
- liquidity_flow_regime
- liquidity_state
- zone_flow_relationship
- reasoning

### Optional

- tp2
- liquidity_target
- acceptance_rejection_evidence
- suggested_action
- setup_quality
- entry_quality

Unknown fields may be preserved under a non-public `extensions` object but must not alter canonical semantics.

## 6. Controlled normalization

Normalize:

- whitespace
- Unicode dash variants to `-` internally where safe
- percentage strings to numeric values where unambiguous
- LONG/SHORT/CONDITIONAL casing
- canonical flow labels

Do not normalize away meaning. Preserve original source text in an audit field when a transformation could affect interpretation.

## 7. Flow regime validation

Accepted values:

```text
RANGE
TRANSITION
DIRECTIONAL
```

A `DIRECTIONAL` classification must have supporting acceptance/displacement evidence in the source. If the evidence is absent, emit a validation error for an executable trade or a warning for a narrative observation, depending on source type.

A `TRANSITION` state must not be presented as a confirmed directional setup.

A `RANGE` state must not be automatically converted into a fade instruction.

## 8. Location/flow rule

`daily_zone` answers **where price is within the 20D range**.

It does not answer **what direction price must trade**.

Therefore:

- Premium + bullish directional flow is valid.
- Discount + bearish directional flow is valid.
- Premium + bearish flow is not rejected merely because of location.
- Discount + bullish flow is not rejected merely because of location.
- Missing flow cannot be inferred from Premium/Discount.

## 9. Acceptance/rejection validation

When a source explicitly provides acceptance/rejection evidence, preserve it verbatim in the audit/source field and normalize the canonical state.

Do not manufacture evidence from price location alone.

Minimum directional evidence should be one or more explicit source observations such as:

- liquidity sweep + acceptance;
- displacement followed by acceptance;
- sequential liquidity consumption;
- breakout hold + failed retracement;
- equivalent explicit analyst evidence.

## 10. Conditional ideas

Conditional setups may contain separate long and short triggers. The parser must preserve both branches rather than flattening them into a single direction.

Example:

```json
{
  "direction": "CONDITIONAL",
  "conditions": {
    "long": {},
    "short": {}
  }
}
```

No branch may be promoted to active direction unless the source explicitly says the trigger has occurred.

## 11. Validation result

Errors prevent publication. Warnings permit publication only when the affected field is non-critical and the publication layer can safely represent uncertainty.

### Error examples

- missing stop on an executable trade;
- invalid flow regime value;
- contradictory canonical fields;
- malformed required numeric field where the source claims a numeric value;
- DIRECTIONAL executable setup without supporting flow evidence.

### Warning examples

- optional TP2 missing;
- historical v5 document missing v6 fields;
- non-canonical but recoverable whitespace/casing;
- optional evidence field absent for a narrative-only observation.

## 12. Publication boundary

The parser outputs research truth. The publication layer determines presentation labels.

The parser must not generate marketing copy, headlines, CTAs, or sales claims.

The website must consume normalized fields rather than reparsing Markdown independently.

## 13. Auditability

Every normalized trade should retain enough source metadata to locate the originating section/field. Recommended fields:

```json
{
  "source_ref": {
    "file": "",
    "section": "",
    "field": ""
  }
}
```

## 14. Acceptance fixtures

Implementation must pass every fixture in `PARSER_FIXTURE_V6.md` before production use.

Minimum acceptance cases:

1. Premium + DIRECTIONAL + accepted liquidity → continuation presentation.
2. Premium + RANGE + rejected liquidity → reversal-watch presentation.
3. TRANSITION + confirmation pending → no directional presentation.
4. v5 input → flow fields `NOT_RECORDED`, no inference.
5. Daily RANGE → DIRECTIONAL update → context changes without automatic Ledger status change.

## 15. Non-goals

The parser does not:

- make trades;
- determine whether a trade is profitable;
- invent missing liquidity levels;
- reinterpret historical research;
- override the Standing Protocol;
- replace the analyst's reasoning;
- infer direction from Premium/Discount alone.

## 16. Implementation order

1. Markdown section parser.
2. Field normalizers.
3. v6 schema validator.
4. Flow/business-rule validator.
5. Fixture test runner.
6. Stable JSON output.
7. Publication derivation adapter.
8. Website integration.
9. CI enforcement.

**Definition of done:** all v6 fixtures pass, v5 compatibility is preserved, invalid executable research blocks publication, and the website consumes the normalized contract without duplicating analytical parsing logic.
