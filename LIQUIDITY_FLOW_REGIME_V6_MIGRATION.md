# HipMarvin FX — Liquidity/Flow Regime v6 Migration

**Date:** 2026-08-19

## Why this change

The existing 20-day range framework was useful for locating price within recent structure, but its Premium/Discount interpretation could become too directional. Recent review of GBP/USD and JPY crosses showed a recurring pattern where liquidity was being consumed in the direction of the trend and acceptance/continuation was overriding a simple mean-reversion read from the 20D range.

## v6 decision

The 20D range remains mandatory, but its role is now explicitly **structural location**.

The new execution layer is **Liquidity/Flow Regime**:

- **RANGE** — sweep/rejection and mean reversion dominate.
- **TRANSITION** — evidence of a regime change exists but acceptance/continuation is incomplete.
- **DIRECTIONAL** — liquidity is swept and accepted, displacement follows through, and subsequent liquidity is pursued in the same direction.

## Canonical files

- `STANDING_PROTOCOL_v6.md` — Rule 23 and the new hierarchy.
- `WEEKLY_RESEARCH_TEMPLATE_v4.md` — weekly schema additions.
- `DAILY_UPDATE_TEMPLATE_v3.md` — daily regime-change record.
- `POSITION_LEDGER_TEMPLATE_v2.md` — Ledger carry-through fields.
- `HipMarvinFX_Generation_Prompts_v6_0.md` — generator synchronization.

## Interpretation change

Old interpretation:

`Premium → favour sell` / `Discount → favour buy`

New interpretation:

`20D location → identify where price is`

then:

`Liquidity → identify destination`

then:

`Acceptance/rejection → determine continuation vs mean reversion`

## Example

A GBP/USD Buy at 82% of the 20D range is not automatically invalid because it is in Deep Premium. If price has swept prior buy-side liquidity, accepted above it, held the breakout, formed a higher low, and is targeting the next liquidity pool, the setup is classified **DIRECTIONAL / Liquidity swept + accepted / Location vs flow conflict — continuation justified**.

Conversely, if the same sweep is immediately rejected and price returns inside the range, the setup is classified **RANGE / Liquidity swept + rejected**, and mean reversion becomes the appropriate interpretation.

## Historical data policy

Do not retroactively rewrite old weekly or daily files simply to apply v6. Historical Rule 21 flags remain valid as historical records. v6 applies to new research and to deliberately revised templates.

## Parser policy

The new fields are additive. Existing fields and status semantics remain intact. No open position is closed or modified merely because the analytical flow regime changes.

## Review checklist before merge

- [ ] Rule 23 present and numbered after Rule 22.
- [ ] 20D range explicitly described as location, not universal direction.
- [ ] Weekly fields present.
- [ ] Daily regime-change block present.
- [ ] Ledger carries the new fields without changing status rules.
- [ ] Generator explicitly instructs the new fields.
- [ ] No historical research files are silently rewritten.
