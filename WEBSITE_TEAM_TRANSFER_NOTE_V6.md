# HipMarvinFX — Website Team Transfer Note v6

**Date:** 2026-08-19  
**Purpose:** Handoff between the methodology/documentation work and the team building the actual website/pages/parser.

---

## 1. Executive summary

The website should now be built around the **v6 Liquidity/Flow Regime model**.

The key methodological correction came from reviewing situations such as GBP/USD where price can be in a Deep Premium or Deep Discount part of the 20-day range while the prevailing liquidity flow is still directional.

### The old mental model must NOT be used

> Premium = automatically bearish / sell.
>
> Discount = automatically bullish / buy.

That interpretation is no longer valid for v6.

### The v6 model

```text
20D Range
   ↓
Structural Location
   ↓
Liquidity
   ↓
Acceptance / Rejection
   ↓
Liquidity/Flow Regime
   ↓
Execution Context
```

The 20D range answers **WHERE price is**.

Liquidity and acceptance/rejection help answer **WHAT price is doing**.

The flow regime describes the current execution context:

- `RANGE`
- `TRANSITION`
- `DIRECTIONAL`

The website must preserve this distinction.

---

## 2. What has already been completed

The canonical documentation repository is:

`HipmarvinFx/hipmarvinfx-docs`

The v6 methodology was developed on feature branch `agent/liquidity-flow-regime`, reviewed through PR #1, and merged into `main`.

The following contracts are now part of `main`:

### Methodology

- `STANDING_PROTOCOL_v6.md`
- `WEEKLY_RESEARCH_TEMPLATE_v4.md`
- `DAILY_UPDATE_TEMPLATE_v3.md`
- `POSITION_LEDGER_TEMPLATE_v2.md`
- `HipMarvinFX_Generation_Prompts_v6_0.md`
- `LIQUIDITY_FLOW_REGIME_V6_MIGRATION.md`

### Website/parser contracts

- `WEBSITE_PARSER_CONTRACT_v6.md`
- `WEBSITE_PAGE_SCHEMA_v6.md`
- `V6_WEBSITE_INTEGRATION.md`
- `PARSER_FIXTURE_V6.md`
- `PARSER_IMPLEMENTATION_SPEC_v6.md`
- `PARSER_TEST_PLAN_v6.md`
- `PUBLISHING_PIPELINE.md` v1.9

This transfer note is the bridge for the website team.

---

## 3. Source of truth hierarchy

The website team should treat the repositories/files in this order:

```text
STANDING_PROTOCOL_v6
        ↓
Research Templates
        ↓
Research Markdown
        ↓
WEBSITE_PARSER_CONTRACT_v6
        ↓
PARSER_IMPLEMENTATION_SPEC_v6
        ↓
Validation / Test Fixtures
        ↓
PUBLISHING_PIPELINE v1.9
        ↓
WEBSITE_PAGE_SCHEMA_v6
        ↓
Actual Website UI
```

### Critical rule

The UI must **not create its own trading interpretation**.

The website consumes normalized research data.

The parser normalizes and validates.

The publication layer determines presentation labels.

The analyst/research file remains the source of analytical conclusions.

---

## 4. What the parser is supposed to do

The parser is a **deterministic normalizer and validator**, not an analyst.

Its job:

1. Read weekly/daily research Markdown.
2. Identify the source/template/version.
3. Extract structured fields.
4. Normalize values into the v6 canonical schema.
5. Validate required fields and business rules.
6. Output stable JSON.
7. Pass normalized data to publication/page generation.

It must NOT:

- infer direction from Premium/Discount;
- invent liquidity levels;
- manufacture acceptance/rejection evidence;
- promote an untriggered conditional setup;
- rewrite historical v5 research as v6;
- change Ledger position status;
- generate marketing claims or CTAs as analytical content.

---

## 5. Important v6 fields

A trade object should expose, where available:

```text
pair
direction
priority
entry
stop
tp1
tp2
rr
timeframe
correlation_class
daily_zone
 tier
macro_regime
macro_alignment
liquidity_flow_regime
liquidity_state
zone_flow_relationship
liquidity_target
acceptance_rejection_evidence
reasoning
suggested_action
setup_quality
entry_quality
```

The exact canonical schema is defined in:

`WEBSITE_PARSER_CONTRACT_v6.md`

Do not create a competing schema in the website code.

---

## 6. How the website should interpret location + flow

These are presentation mappings, not new trading rules.

| 20D Location | Flow | Liquidity State | Example public presentation |
|---|---|---|---|
| Deep Premium | DIRECTIONAL | Swept + accepted | Premium / Bullish Continuation when source direction is bullish |
| Deep Premium | RANGE | Swept + rejected | Premium / Reversal Watch |
| Mild Premium | TRANSITION | Confirmation pending | Transition / Wait for Confirmation |
| Deep Discount | DIRECTIONAL | Swept + accepted | Discount / Bearish Continuation when source direction is bearish |

The actual research conclusion always takes precedence over a generic label.

### Example

If GBP/USD is:

```text
82% up 20D range
Deep Premium
DIRECTIONAL
Liquidity swept + accepted
Bullish direction
```

the website should NOT reject the trade because it is in Premium.

It should communicate that the setup is a **bullish continuation despite premium location**, if that is what the research says.

Conversely, if price is in Deep Premium but flow is RANGE and liquidity has been swept and rejected, the website can present a **reversal watch** context.

---

## 7. Required website architecture

The page contract currently defines three principal experiences.

### `/` — Landing / Dashboard

Purpose: give the visitor a fast understanding of the current market opportunity set.

Recommended hierarchy:

```text
Current Market Regime
        ↓
Strongest / Weakening Currencies
        ↓
Featured Trade Setups
        ↓
Flow Regime Distribution
        ↓
Liquidity Watch
        ↓
Latest Research
        ↓
CTA / Membership Boundary
```

Featured trade cards should expose at minimum:

```text
Pair
Direction
Setup Quality
Entry Quality
Action
20D Location
Flow Regime
Liquidity State
One-line reasoning
```

### `/trades/[pair]` — Trade Page

Should show:

1. Pair / Direction / Suggested Action
2. Entry / Stop / TP1 / TP2 / R:R
3. Setup Quality / Entry Quality
4. 20D Structural Location
5. Liquidity/Flow Regime
6. Liquidity State
7. Zone/Flow Relationship
8. Liquidity Target
9. Acceptance/Rejection Evidence
10. Macro Regime / Macro Alignment
11. Reasoning
12. Risk / Invalidation

The Trade Page is where the full location-versus-flow relationship should be visible.

### `/research/[slug]` — Research Article

Weekly:

- This week's view
- What's driving it
- Trade ideas
- Zone + Liquidity
- Institutional positioning
- What we're watching
- Risk Assessment
- Sources

Daily:

- Market takeaway
- What changed today
- Liquidity / Flow Regime Change, when present
- Impact on our outlook
- Risk Assessment
- What we're watching next
- Sources

### `/research` — Research Index

Should support filtering by:

- date/week
- publication type
- pair/currency
- flow regime
- direction
- status

---

## 8. Empty and historical states

If v6 flow fields are missing:

```text
Flow Unconfirmed
```

or the equivalent canonical `NOT_RECORDED` state should be used.

Do NOT infer flow from:

- Premium
- Discount
- 20D percentage
- a simple up/down price move

For historical v5 files:

```text
liquidity_flow_regime = NOT_RECORDED
liquidity_state = NOT_RECORDED
zone_flow_relationship = NOT_RECORDED
```

unless the source actually contains equivalent explicit evidence.

---

## 9. Parser acceptance fixtures

Before wiring the website to production data, the implementation should pass all cases in:

`PARSER_FIXTURE_V6.md`

The minimum cases are:

1. Premium + DIRECTIONAL + accepted liquidity.
2. Premium + RANGE + rejected liquidity.
3. TRANSITION + confirmation pending.
4. Historical v5 input.
5. Daily RANGE → DIRECTIONAL change.

The corresponding test requirements are in:

`PARSER_TEST_PLAN_v6.md`

---

## 10. What the existing page team should do now

If pages/components already exist, **do not throw them away**.

Instead:

### Step 1 — Inventory existing implementation

Identify:

- current page routes;
- current data source;
- current parser, if any;
- current JSON/data model;
- current trade-card fields;
- current research article renderer;
- current landing-page components;
- current deployment setup;
- current analytics/forms/CTA implementation.

### Step 2 — Compare against v6 contracts

Map existing fields to:

`WEBSITE_PARSER_CONTRACT_v6.md`

Do not rename or delete existing functionality until the mapping is understood.

### Step 3 — Identify conflicts

Look specifically for any code equivalent to:

```text
Premium → Short
Discount → Long
```

or:

```text
Premium = reversal
Discount = reversal
```

Those rules must be removed/replaced for v6.

### Step 4 — Preserve good existing work

Existing visual design, responsive layout, navigation, forms, analytics, assets, and components can remain if they do not contradict the v6 data model.

### Step 5 — Replace data interpretation, not necessarily the UI

The goal is **harmonization**, not a rewrite for its own sake.

---

## 11. Important architectural decision

The documentation repository currently contains the canonical contracts.

The connected GitHub account currently exposes:

`HipmarvinFx/hipmarvinfx-docs`

No separate website/application repository was visible when this handoff was created.

Therefore the website team's existing codebase should be treated as the implementation source until its repository/location is identified.

Do NOT create a second parser with a competing schema.

If the existing page team has an application repository, that repository should consume these contracts from `hipmarvinfx-docs` or reproduce them faithfully in its implementation.

---

## 12. Definition of done for harmonization

We are not finished when the pages merely look correct.

The implementation is harmonized when:

- the website consumes normalized v6 data;
- the parser passes all v6 fixtures;
- v5 research remains compatible;
- Premium/Discount is treated as location, not automatic direction;
- flow regime is displayed explicitly;
- liquidity state is displayed where available;
- acceptance/rejection evidence is preserved;
- conditional setups remain conditional;
- uncertainty is not converted into certainty;
- the website does not independently reparse research Markdown;
- the landing page and Trade Pages use the same normalized source;
- research articles and trade cards do not contradict each other.

---

## 13. Recommended next handoff from the website team

When the page team reviews this note, they should return with:

1. **Current repository name/URL**
2. **Current parser/data source**
3. **Current page routes**
4. **Current JSON/data model**
5. **Which existing fields already map to v6**
6. **Which fields are missing**
7. **Any existing Premium/Discount directional logic that must be replaced**
8. **Current landing-page status**
9. **Current Trade Page status**
10. **Current Research Article status**

That response becomes the reconciliation checklist before implementation changes are made.

---

## 14. Final instruction to the website team

**Do not start by redesigning the pages.**

First reconcile the existing implementation with the v6 contracts.

The objective is:

> **One analytical source of truth, one normalized data contract, one publication layer, multiple consistent website views.**

The website is the presentation layer of the research system — not a second trading-analysis engine.
