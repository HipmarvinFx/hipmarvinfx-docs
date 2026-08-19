# HipMarvin FX — Website Page Schema v6

**Version:** 6.0  
**Date:** 2026-08-19

This document defines the page-level contract for the v6 public website. It complements `WEBSITE_PARSER_CONTRACT_v6.md`; it does not replace the research templates or Standing Protocol.

## Page architecture

### `/` — Landing / Dashboard

Purpose: 10-second understanding of the current weekly opportunity set.

Required sections:

1. Hero: current market regime / weekly thesis.
2. Market snapshot: strongest / weakening currencies.
3. Featured setups: selected Trade Priority List ideas.
4. Flow regime strip: RANGE / TRANSITION / DIRECTIONAL distribution.
5. Liquidity watch: selected upcoming pools and confirmation conditions.
6. Latest research: weekly and daily articles.
7. CTA / membership boundary as defined by the product, without inventing offers.

Featured setup card fields:

`pair → direction → setup quality → entry quality → action → 20D location → flow regime → liquidity state → one-line reasoning`

### `/trades/[pair]` — Trade Page

Purpose: one-pair decision context.

Required fields:

- Pair / direction
- Suggested action
- Entry / stop / TP1 / TP2 / R:R
- Setup Quality
- Entry Quality
- 20D Structural Location
- Liquidity/Flow Regime
- Liquidity State
- Zone/Flow Relationship
- Liquidity Target
- Acceptance/Rejection Evidence
- Macro Regime
- Macro Alignment
- Reasoning
- Invalidation / risk

The page must make a location-vs-flow conflict visible. Do not collapse it into a generic "bullish" or "bearish" badge.

### `/research/[slug]` — Research Article

Purpose: full reader-facing analysis.

Weekly article sections:

- This week's view
- What's driving it
- Trade ideas
- Zone + Liquidity
- Institutional positioning
- What we're watching
- Risk Assessment
- Sources

Daily article sections:

- Market takeaway
- What changed today
- Liquidity / Flow Regime Change, when present
- Impact on our outlook
- Risk Assessment
- What we're watching next
- Sources

### `/research` — Research Index

Purpose: browse and filter publications.

Filters:

- Week/date
- Publication type
- Currency/pair
- Flow regime
- Direction
- Status

## Public label rules

Internal canonical fields remain exact. Public labels may be shorter:

| Canonical | Public label |
|---|---|
| `RANGE` | Range |
| `TRANSITION` | Transition |
| `DIRECTIONAL` | Directional |
| `Liquidity swept + rejected` | Sweep Rejected |
| `Liquidity swept + accepted` | Sweep Accepted |
| `Sequential liquidity consumption` | Sequential Flow |
| `Transition / confirmation pending` | Confirmation Pending |
| `Location vs flow conflict — continuation justified` | Continuation Despite Location |
| `Location vs flow conflict — reversal not yet confirmed` | Reversal Not Confirmed |

Public labels must never imply greater certainty than the canonical field.

## Visual hierarchy

The primary visual hierarchy should be:

**Market context → trade location → flow regime → liquidity state → action.**

The 20D zone should be visually subordinate to the flow regime when the two conflict. This is a presentation decision, not a change to the analytical hierarchy.

## Empty / unknown states

If v6 flow fields are missing:

- show `Flow Unconfirmed`;
- do not infer a regime;
- retain the 20D location as historical/structural context;
- suppress any UI language that implies continuation or reversal certainty.

## Responsive requirement

The landing card must remain understandable on mobile without requiring a chart or hover interaction. Full evidence belongs on the Trade Page.

## Accessibility / semantics

Every flow badge must have visible text, not color alone. Direction and action must also be text-labelled. Do not rely on green/red alone to communicate trade state.

## Content fidelity

All displayed analytical text must trace to the parsed research object. The UI can shorten, but may not invent, reverse, strengthen, or suppress a material risk/conflict.
