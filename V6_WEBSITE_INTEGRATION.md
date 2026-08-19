# HipMarvin FX — v6 Website Integration Bridge

**Date:** 2026-08-19  
**Status:** Canonical bridge for implementation

## Purpose

The existing Publishing Pipeline v1.8 defines the three website experience layers and their publication responsibilities. v6 does not replace that architecture. This bridge updates the website-facing interpretation for the new Liquidity/Flow Regime schema.

For implementation, use this bridge together with:

- `STANDING_PROTOCOL_v6.md`
- `WEEKLY_RESEARCH_TEMPLATE_v4.md`
- `DAILY_UPDATE_TEMPLATE_v3.md`
- `POSITION_LEDGER_TEMPLATE_v2.md`
- `HipMarvinFX_Generation_Prompts_v6_0.md`
- `WEBSITE_PARSER_CONTRACT_v6.md`
- `WEBSITE_PAGE_SCHEMA_v6.md`
- `PARSER_FIXTURE_V6.md`

## v6 publication mapping

### Dashboard

Existing fields remain:

- Pair
- Setup Quality
- Entry Quality
- Suggested Action

Add/display:

- 20D Structural Location
- Liquidity/Flow Regime
- Liquidity State
- Zone/Flow Relationship

The dashboard must not convert Premium/Discount into an automatic reversal signal.

### Trade Page

Existing full-context page remains. Insert the v6 flow block immediately after the 20D location:

`20D Location → Liquidity/Flow Regime → Liquidity State → Zone/Flow Relationship → Liquidity Target → Acceptance/Rejection Evidence`

This is the reader-facing explanation of why a setup may continue while in Premium or Discount.

### Research Article

Weekly articles should translate the former "Correlation and zone check" concept into:

**Correlation + Structural Location + Liquidity/Flow Check**

The article must explain whether liquidity was rejected or accepted when that distinction materially affects the trade interpretation.

Daily articles should include the Daily Liquidity / Flow Regime Check whenever the daily research file contains one.

## Derivation constraint

The publishing layer may derive concise labels, but it may not create a new trading conclusion.

Correct:

`Deep Premium + DIRECTIONAL + accepted liquidity → Premium / Bullish Continuation`

Incorrect:

`Deep Premium → Buy`

Correct:

`Deep Premium + RANGE + swept/rejected → Premium / Reversal Watch`

Incorrect:

`Deep Premium → Sell`

## Parser/build dependency

The parser must normalize the v6 fields before publication derivation runs. The landing page must consume normalized parser output rather than parsing raw Markdown itself.

Recommended build chain:

`Research Markdown → v6 parser → validation → normalized JSON → publication derivation → page data → Landing / Trade / Research pages`

A failed validation should block publication of the affected item rather than silently falling back to an inferred interpretation.

## Historical compatibility

v5 historical records remain valid. When they lack v6 fields, the website should show `Flow Unconfirmed` and retain their original 20D location as historical context. Never infer a flow regime retrospectively.

## Implementation acceptance

The website implementation is v6-ready only when `PARSER_FIXTURE_V6.md` Cases A–E pass and the landing page cannot produce a direction solely from a Premium/Discount location.
