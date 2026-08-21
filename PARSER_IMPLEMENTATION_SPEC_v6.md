# HipMarvinFX Parser Implementation Specification v6.1

**Status:** Canonical implementation specification  
**Version:** 6.1  
**Date:** 2026-08-21

## 1. Purpose

Define the implementation boundary for the v6.1 research parser. The parser converts canonical weekly/daily research Markdown into validated normalized data consumed by the publication and website layers.

It is a **normalizer and validator**, not an analyst.

Canonical upstream contracts:

- `STANDING_PROTOCOL_v6.md`
- `PAIR_DISCOVERY_SELECTION_V6_1.md`
- `WEEKLY_RESEARCH_TEMPLATE_v4.md`
- `DAILY_UPDATE_TEMPLATE_v3.md`
- `POSITION_LEDGER_TEMPLATE_v2.md`
- `WEBSITE_PARSER_CONTRACT_v6.md`
- `WEBSITE_PAGE_SCHEMA_v6.md`
- `PUBLISHING_PIPELINE.md` v1.9+

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
v6.1 schema validation
    ↓
Pair-discovery validation
    ↓
Rule 23 business-rule validation
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

If the document is v5 or lacks v6/v6.1 fields, it remains parseable. Missing fields become `NOT_RECORDED`; they are never inferred from 20D location.

## 4. Canonical normalized envelope

```json
{
  "schema_version": "6.1",
  "source": {
    "file": "",
    "template": "",
    "template_version": "",
    "protocol": "",
    "parsed_at": ""
  },
  "weekly": {},
  "pair_discovery": [],
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

## 5. Pair discovery normalization

Each Pair Discovery Matrix row should normalize to:

- currency_thesis
- candidate_pair
- candidate_universe (`MAJOR` / `LIQUID_CROSS`)
- relative_strength
- daily_zone
- liquidity
- flow_regime
- catalyst
- expression_rank
- decision (`SELECT` / `WATCH` / `REJECT`)

The parser must not reject `LIQUID_CROSS` merely because it is a cross.

If `REJECT` is supplied, preserve the analyst's stated rejection reason where available. A rejection reason of only `"cross"` or equivalent is a validation error for v6.1.

## 6. Best-expression normalization

For each material currency thesis, normalize when supplied:

- preferred_expression
- alternative_expression
- why_preferred
- why_alternative_weaker
- concentration_note

The parser must preserve the selected expression exactly as written and must not substitute a major pair merely because it contains USD.

## 7. Trade normalization

Each Trade Priority List entry must normalize to:

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
- candidate_universe
- pair_discovery_thesis
- best_expression
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

## 8. Controlled normalization

Normalize:

- whitespace
- Unicode dash variants to `-` internally where safe
- percentage strings to numeric values where unambiguous
- LONG/SHORT/CONDITIONAL casing
- canonical flow labels
- MAJOR / LIQUID_CROSS casing
- SELECT / WATCH / REJECT casing

Do not normalize away meaning. Preserve original source text in an audit field when a transformation could affect interpretation.

## 9. Pair-discovery validation

The discovery layer is a **selection layer**, not an execution layer.

Required principles:

1. Start from currency-relative strength/weakness.
2. Compare majors and liquid crosses.
3. Do not treat USD inclusion as an automatic advantage.
4. Do not treat cross status as an automatic disadvantage.
5. Require an actual analytical/execution reason for rejection.
6. Correlated pairs must still be recognized as the same underlying currency/liquidity bet where appropriate.

A selected pair must be handed to Rule 23 for flow/execution validation; discovery alone cannot create a trade.

## 10. Flow regime validation

Accepted values:

```text
RANGE
TRANSITION
DIRECTIONAL
```

A `DIRECTIONAL` classification must have supporting acceptance/displacement evidence in the source. If the evidence is absent, emit a validation error for an executable trade or a warning for a narrative observation, depending on source type.

A `TRANSITION` state must not be presented as a confirmed directional setup.

A `RANGE` state must not be automatically converted into a fade instruction.

## 11. Location/flow rule

`daily_zone` answers **where price is within the 20D range**.

It does not answer **what direction price must trade**.

Therefore:

- Premium + bullish directional flow is valid.
- Discount + bearish directional flow is valid.
- Premium + bearish flow is not rejected merely because of location.
- Discount + bullish flow is not rejected merely because of location.
- Missing flow cannot be inferred from Premium/Discount.

## 12. Acceptance/rejection validation

When a source explicitly provides acceptance/rejection evidence, preserve it verbatim in the audit/source field and normalize the canonical state.

Do not manufacture evidence from price location alone.

Minimum directional evidence should be one or more explicit source observations such as:

- liquidity sweep + acceptance;
- displacement followed by acceptance;
- sequential liquidity consumption;
- breakout hold + failed retracement;
- equivalent explicit analyst evidence.

## 13. Conditional ideas

Conditional setups may contain separate long and short triggers. The parser must preserve both branches rather than flattening them into a single direction.

No branch may be promoted to active direction unless the source explicitly says the trigger has occurred.

## 14. Validation result

Errors prevent publication. Warnings permit publication only when the affected field is non-critical and the publication layer can safely represent uncertainty.

### Error examples

- missing stop on an executable trade;
- invalid flow regime value;
- invalid candidate universe value;
- selected cross rejected solely because it is a cross;
- contradictory canonical fields;
- malformed required numeric field where the source claims a numeric value;
- DIRECTIONAL executable setup without supporting flow evidence.

### Warning examples

- optional TP2 missing;
- historical v5 document missing v6/v6.1 fields;
- non-canonical but recoverable whitespace/casing;
- optional evidence field absent for a narrative-only observation.

## 15. Publication boundary

The parser outputs research truth. The publication layer determines presentation labels.

The parser must not generate marketing copy, headlines, CTAs, or sales claims.

The website must consume normalized fields rather than reparsing Markdown independently.

## 16. Auditability

Every normalized trade and discovery row should retain enough source metadata to locate the originating section/field. Recommended fields:

```json
{
  "source_ref": {
    "file": "",
    "section": "",
    "field": ""
  }
}
```

## 17. Acceptance fixtures

Implementation must pass every fixture in `PARSER_FIXTURE_V6.md` plus v6.1 pair-discovery cases.

Minimum v6.1 acceptance cases:

1. Strong-vs-weak currency relationship selects a liquid cross over a compressed major.
2. Major remains the Best Expression when the cross has inferior liquidity/execution.
3. A cross is not rejected solely because it is a cross.
4. Premium + DIRECTIONAL + accepted liquidity → continuation presentation.
5. Premium + RANGE + rejected liquidity → reversal-watch presentation.
6. TRANSITION + confirmation pending → no directional presentation.
7. v5 input → v6/v6.1 fields `NOT_RECORDED`, no inference.
8. Daily RANGE → DIRECTIONAL update → context changes without automatic Ledger status change.

## 18. Non-goals

The parser does not:

- make trades;
- determine whether a trade is profitable;
- invent missing liquidity levels;
- reinterpret historical research;
- override the Standing Protocol;
- replace the analyst's reasoning;
- infer direction from Premium/Discount alone;
- choose a pair from incomplete evidence on behalf of the analyst.

## 19. Implementation order

1. Markdown section parser.
2. Pair Discovery Matrix extraction.
3. Best Expression extraction.
4. Field normalizers.
5. v6.1 schema validator.
6. Pair-discovery validator.
7. Flow/business-rule validator.
8. Fixture test runner.
9. Stable JSON output.
10. Publication derivation adapter.
11. Website integration.
12. CI enforcement.

**Definition of done:** v6.1 discovery and v6 Rule 23 fixtures pass, v5 compatibility is preserved, invalid executable research blocks publication, and the website consumes the normalized contract without duplicating analytical parsing logic.
