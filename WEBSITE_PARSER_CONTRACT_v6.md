# HipMarvin FX — Website / Parser Contract v6

**Version:** v6.0  
**Date:** 2026-08-19  
**Status:** Canonical implementation contract for v6 website generation

This document defines the stable hand-off between the v6 research files, the parser/publication layer, and the three website experience layers. It exists so the landing page, trade pages, research pages, and parser can be built against one explicit contract rather than inferred from prose.

## 1. Source-of-truth hierarchy

The implementation must preserve this order:

`Research File → Parser Normalization → Publication Derivation → Website Pages`

The parser may normalize syntax, but it must not invent analytical conclusions. Publication derivation may shorten or label content, but it must not change the research conclusion.

v6 introduces three analytical fields that are first-class parser fields:

- `liquidity_flow_regime`
- `liquidity_state`
- `zone_flow_relationship`

The 20D range remains a location field. It must never be converted by the parser into a directional buy/sell signal.

## 2. Canonical normalized trade object

Each weekly Trade Priority List idea should normalize to the following object shape:

```json
{
  "schema_version": "6.0",
  "pair": "GBP/USD",
  "direction": "LONG",
  "priority": 1,
  "rating": 5,
  "entry": "1.3520-1.3540",
  "stop": "1.3450",
  "tp1": "1.3600",
  "tp2": "1.3660",
  "rr": "2.1",
  "timeframe": "4H",
  "correlation_class": "USD-quote group",
  "daily_zone": {
    "percent": 82,
    "label": "Deep Premium"
  },
  "tier": 1,
  "macro_regime": "GBP constructive / USD weakening",
  "macro_alignment": "Aligned",
  "liquidity_flow_regime": "DIRECTIONAL",
  "liquidity_state": "Liquidity swept + accepted",
  "zone_flow_relationship": "Location vs flow conflict — continuation justified",
  "liquidity_target": "prior buy-side high / next external liquidity",
  "acceptance_rejection_evidence": "breakout accepted; shallow retracement; higher low",
  "reasoning": "...",
  "suggested_action": "Wait for Pullback",
  "setup_quality": "Strong",
  "entry_quality": "Buying High"
}
```

The example is illustrative only. The parser must use actual source values and must never copy the example as data.

## 3. Required parser fields

### Required for every trade idea

| Canonical key | Source field | Required | Notes |
|---|---|---:|---|
| `schema_version` | template version | yes | `6.0` for v6 files |
| `pair` | Priority heading | yes | normalized pair symbol |
| `direction` | Priority heading | yes | `LONG` / `SHORT` / `CONDITIONAL` |
| `priority` | Priority heading | yes | integer where present |
| `entry` | Entry | yes | preserve source text if zone/range |
| `stop` | Stop | yes | no trade without stop |
| `tp1` | TP1 | yes | null only if source explicitly lacks it |
| `tp2` | TP2 | conditional | null allowed when source omits it |
| `rr` | R:R | yes | preserve source value/text |
| `timeframe` | Timeframe | yes | preserve source value |
| `correlation_class` | Correlation Class | yes | controlled vocabulary from template |
| `daily_zone.percent` | Daily Zone | yes | numeric when parseable |
| `daily_zone.label` | Daily Zone | yes | preserve canonical label |
| `tier` | Tier | yes | `1` / `2` |
| `macro_regime` | Macro Regime | yes | source text |
| `macro_alignment` | Macro Alignment | yes | `Aligned` / `Mixed` / `Contrarian` |
| `liquidity_flow_regime` | Liquidity/Flow Regime | yes in v6 | `RANGE` / `TRANSITION` / `DIRECTIONAL` |
| `liquidity_state` | Liquidity State | yes in v6 | controlled vocabulary |
| `zone_flow_relationship` | Zone/Flow Relationship | yes in v6 | controlled vocabulary |
| `reasoning` | Reasoning | yes | preserve uncertainty |

### Recommended derived/publication fields

These are derived downstream only when the source fields exist:

- `liquidity_target`
- `acceptance_rejection_evidence`
- `setup_quality`
- `entry_quality`
- `suggested_action`
- `public_flow_label`
- `public_zone_label`

A derived field must retain a traceable pointer to its source fields.

## 4. Controlled vocabularies

### Liquidity/Flow Regime

```text
RANGE
TRANSITION
DIRECTIONAL
```

### Liquidity State

```text
Untaken liquidity
Liquidity swept + rejected
Liquidity swept + accepted
Sequential liquidity consumption
Transition / confirmation pending
```

### Zone/Flow Relationship

```text
Mean-reversion aligned
Continuation aligned
Location vs flow conflict — continuation justified
Location vs flow conflict — reversal not yet confirmed
Neutral / insufficient evidence
```

The parser should normalize whitespace/case but should not silently map an unknown value to another valid state. Unknown values must produce a validation warning.

## 5. Backward compatibility

v5 files remain parseable.

If `schema_version < 6` or the source file uses the v5 template:

- parse all existing fields normally;
- preserve the existing `daily_zone` calculation/label as historical data;
- set v6 fields to `null` / `NOT_RECORDED` rather than inferring them;
- do not derive `RANGE`, `TRANSITION`, or `DIRECTIONAL` from price position alone;
- do not convert historical Premium/Discount flags into v6 directional conclusions.

This is mandatory. Historical data must not be silently rewritten by the parser.

## 6. Validation rules

A v6 trade object is valid only when:

1. `liquidity_flow_regime` is one of the three canonical values.
2. `liquidity_state` is a canonical value.
3. `zone_flow_relationship` is a canonical value.
4. `daily_zone.percent` is numeric when the source provides a numeric percentage.
5. `direction` is explicit or the source is explicitly conditional.
6. `stop` is present for executable trade ideas.
7. A `DIRECTIONAL` classification has supporting acceptance/displacement evidence in the source reasoning or daily flow record.
8. A Premium Buy or Discount Sell is not rejected merely because of location; the parser should surface the flow relationship instead.
9. A `RANGE` classification does not automatically become a fade instruction.
10. A regime change does not alter Ledger status, stop, TP, or open/closed state.

## 7. Website Experience Layer 1 — Landing / Dashboard

**Purpose:** answer: "What are the best opportunities this week?"

The landing page should expose a compact setup card for each selected trade:

- Pair
- Direction
- Setup Quality
- Entry Quality
- Suggested Action
- 20D Location
- Flow Regime
- Liquidity State
- One-line thesis

### Public presentation rule

Do not present `Deep Premium` as a red/short signal by itself.

Instead show the relationship explicitly, for example:

`Deep Premium · Directional Bullish · Continuation`

or

`Deep Premium · Range · Reversal Watch`

The landing page may use short labels such as:

- `Directional`
- `Transition`
- `Range`
- `Continuation`
- `Reversal Watch`
- `Wait for Pullback`

These are presentation labels only; the canonical internal fields remain unchanged.

## 8. Website Experience Layer 2 — Trade Page

The Trade Page must expose enough context to answer: "Why this trade despite its 20D location?"

Recommended order:

1. Pair / Direction / Suggested Action
2. Entry / Stop / TP1 / TP2 / R:R
3. Setup Quality / Entry Quality
4. 20D Structural Location
5. Liquidity/Flow Regime
6. Liquidity State
7. Zone/Flow Relationship
8. Liquidity Target
9. Acceptance/Rejection Evidence
10. Macro Regime + Macro Alignment
11. Reasoning
12. Risk / invalidation

When `zone_flow_relationship` contains `Location vs flow conflict`, the UI must show that fact rather than hiding it.

## 9. Website Experience Layer 3 — Research Article

The research article remains narrative-first, but v6 flow data should appear wherever the article discusses trade location or execution.

For weekly articles:

- This week's view = Overall Bias + Macro Thesis
- What's driving it = Macro Drivers
- Trade ideas = normalized Priority List
- Zone + liquidity = Rule 21 + Rule 23 translation
- Institutional positioning = COT
- What we're watching = Daily Game Plan

For daily articles:

- Market takeaway = Verdict + Thesis Update
- What changed today = Events + Actual + Price Note
- Flow change = Daily Liquidity/Flow Regime Check when present
- Impact on outlook = Trade Impact + Thesis Update
- What we're watching next = existing forward-only derivation

## 10. Landing-page card derivation

Use this deterministic presentation logic:

| Internal state | Public interpretation |
|---|---|
| RANGE + swept/rejected | `Range / Reversal Watch` |
| TRANSITION | `Transition / Wait for Confirmation` |
| DIRECTIONAL + accepted | `Directional / Continuation` |
| DIRECTIONAL + sequential consumption | `Directional / Continuation` |
| Premium + DIRECTIONAL bullish | `Premium / Bullish Continuation` |
| Discount + DIRECTIONAL bearish | `Discount / Bearish Continuation` |
| Premium + RANGE | `Premium / Reversal Watch` |
| Discount + RANGE | `Discount / Reversal Watch` |
| Unknown / missing | `Flow Unconfirmed` |

This table is a presentation rule, not a trading rule.

## 11. Parser output envelope

The parser should return a stable top-level object:

```json
{
  "schema_version": "6.0",
  "source": {
    "file": "WEEK35_WEEKLY_RESEARCH.md",
    "template": "WEEKLY_RESEARCH_TEMPLATE_v4.md",
    "protocol": "STANDING_PROTOCOL_v6.md"
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

The `validation` block is mandatory. Parser failures must be visible to the build/publishing process and must never be silently swallowed.

## 12. Landing-page safety rules

The website must never:

- infer direction from Premium/Discount alone;
- display a Premium Buy as an error solely because it is Premium;
- display a Discount Sell as an error solely because it is Discount;
- turn `DIRECTIONAL` into an automatic buy/sell instruction;
- close or alter a Ledger position from a flow-regime change;
- remove uncertainty from `TRANSITION` or unconfirmed states;
- invent liquidity levels not present in the source research.

## 13. Build acceptance tests

Before the website/parser implementation is considered v6-ready, it must pass these cases:

### Case A — GBP/USD continuation in Premium

Input: `Deep Premium` + `DIRECTIONAL` + `Liquidity swept + accepted` + `Location vs flow conflict — continuation justified`.

Expected public card: **Premium / Bullish Continuation**.

Expected behavior: no zone-conflict rejection.

### Case B — GBP/USD failed sweep

Input: `Deep Premium` + `RANGE` + `Liquidity swept + rejected`.

Expected public card: **Premium / Reversal Watch**.

Expected behavior: no directional continuation label.

### Case C — transition

Input: `Mild Premium` + `TRANSITION` + `Transition / confirmation pending`.

Expected public card: **Transition / Wait for Confirmation**.

Expected behavior: no automatic trade instruction.

### Case D — historical v5

Input: v5 trade with `Daily Zone: 84% — Deep Premium` and no Rule 23 fields.

Expected parser output: `liquidity_flow_regime = NOT_RECORDED` and no inferred direction.

### Case E — open position flow change

Input: Ledger status `OPEN`, Daily Update changes `RANGE → DIRECTIONAL`.

Expected behavior: analytical context updated; Ledger status remains `OPEN` unless actual price/status rules independently change it.

## 14. Implementation boundary

This contract belongs in the docs repository. The actual landing page, Trade Page, parser, and build code may live elsewhere, but those implementations must conform to this contract.

If implementation requirements conflict with this document, resolve the discrepancy through a versioned schema change rather than silently changing the parser or UI.

**Canonical principle:**

> The website shows the research conclusion; it does not manufacture the conclusion.
