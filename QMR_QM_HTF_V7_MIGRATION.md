# HipMarvin FX — QMR/QM Higher-Timeframe v7 Migration

**Date:** 2026-08-23

## Purpose

v7 converts the existing v6 Liquidity/Flow framework into a higher-timeframe trend-following execution doctrine.

## What changes

1. **Trend continuation is the default.** Follow Daily/4H direction.
2. **1H is the minimum execution timeframe.** No 30M, 15M, 5M or lower execution logic.
3. **Countertrend trades require a confirmed HTF structural break.** Premium/Discount, range extension, liquidity sweeps and patterns alone cannot reverse the bias.
4. **QMR means Quality → Manipulation → Reaction.** It organizes the setup sequence rather than acting as a standalone reversal system.
5. **QM/QML is an entry refinement.** It can improve location after a valid directional thesis and reaction; it cannot create the thesis.
6. **20D remains a location map.** It does not dictate direction.
7. **Liquidity/Flow remains the destination/acceptance layer.** Accepted directional flow can justify continuation through Premium or Discount.
8. **Patterns never override structure.** A textbook-looking QM pattern is rejected when it conflicts with the prevailing HTF structure unless a confirmed structural break has occurred.

## Canonical execution sequence

`Macro Regime → Daily/4H Trend → 20D Location → Liquidity → QMR → Displacement/Acceptance or Rejection → QM/QML Refinement → Liquidity Target → Risk`

## Countertrend gate

A countertrend setup is eligible only when a meaningful Daily/4H protected swing is decisively broken and the break demonstrates acceptance/follow-through. A wick or temporary breach is not enough.

## Required v7 fields

- HTF Trend
- Trend Alignment
- Structural Break
- QMR Phase
- QM/QML Refinement
- Liquidity Target
- Execution TF

## Historical-data policy

Do not silently rewrite historical v5/v6 research. v7 applies to new research and deliberately revised templates. Existing Rule 23 data remains valid historical context.

## Implementation impact

The following canonical files should be synchronized to v7 before the production generator/parser is treated as v7-complete:

- `STANDING_PROTOCOL_v7.md` — canonical rules.
- `WEEKLY_RESEARCH_TEMPLATE_v5.md` — weekly v7 fields.
- `DAILY_UPDATE_TEMPLATE_v4.md` — daily HTF/QMR regime block.
- `POSITION_LEDGER_TEMPLATE_v3.md` — carry-through fields.
- `HipMarvinFX_Generation_Prompts_v7_0.md` — generator synchronization.

v6 files remain historical references.
