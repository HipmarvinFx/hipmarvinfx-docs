# HIPMARVINFX — TEAM HANDOVER REFERENCE

**Effective:** 12 September 2026

## Primary handover

The comprehensive project-state handover is:

`HANDOVER_2026-09-12_PROJECT_STATE.md`

GitHub repository:

`HipmarvinFx/hipmarvinfx-docs`

## Team rule

Treat the handover as the current project-state reference for development decisions. Do not restart, replace, or create a competing architecture without first reconciling it with this document.

## Immediate priority

The next engineering task is **production access-control verification**.

Test `/live-trades` directly while logged out/incognito and inspect every API/network request used by the page. Removing the page from navigation is not sufficient protection.

If protected live-trade/research data is publicly accessible, fix the server/API authorization boundary before adding new features.

## Development sequence

1. Security and route/API protection
2. Evidence/data-integrity verification
3. Deterministic ABC rules
4. QMR + ABC integration
5. Time + price + catalyst integration
6. Historical backtesting
7. AI interpretation
8. UI/cosmetic expansion

## Core principles

- No evidence = no invented fact.
- AI interprets evidence; it does not manufacture evidence.
- A pattern cannot override structure.
- ABC integrates with QMR; it does not replace it.
- CISD is not mandatory for every ABC setup.
- Hiding a route is not securing a route.
- Deployment success does not prove runtime correctness.
- Backtesting follows deterministic rule definition.

## Reference link

Use the repository's `HANDOVER_2026-09-12_PROJECT_STATE.md` as the starting point for the current state, open items, and next actions.
