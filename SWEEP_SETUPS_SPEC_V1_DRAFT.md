# HipMarvinFX: Sweep Setups Spec v1 (DRAFT, not implemented)

Status: draft for owner review. No code may be written until the OPEN items are resolved and this is committed to hipmarvinfx-docs.
Scope: two NEW detectors. They do not modify the frozen v7 trend, permission or QMR logic, Rule 32, or any threshold.
Tags: **[C]** confirmed by owner, **[A]** assumed by Claude, needs confirmation.
Direction below is bearish. Bullish is the exact mirror **[C]**.

## Shared definitions
- Manipulation: price trades beyond a prior period high (or low) then closes back inside the prior range; a wick alone does not count **[A]**.
- Displacement: candle range >= 1.5 x the 20-bar average range (from the owner's own Smart FVG HTF source) **[A]**.
- Point of Interest (POI): the FVG or order block left by the displacement **[C]**.
- Opens (Daily Open, Weekly Open) must come from ONE defined source and timezone before any rule uses them. Feed Daily candles appeared date-shifted in the AUDCAD check **[A]**. OPEN.
- Missing or unsourced inputs: write NOT_AVAILABLE, never infer (Rule 1).

## Setup A: Sweep + FVG + CISD (reversal from premium)
| Element | Rule |
|---|---|
| Context | Price inside a bearish H4 FVG in premium **[C]**. Premium = upper half of dealing range, or >= 60% by Rule 21 **[A]** |
| Sweep | Price trades above a swing high (a) inside the FVG; Previous Week High counts as a target **[C]** |
| Trigger | CISD after the sweep **[C]**. Definition: OPEN (see below) |
| Entry | Inside the FVG after CISD **[C]**. Proximal edge, or CISD close if already inside **[A]** |
| Default stop | The wider of Previous Month High, or the high reached after the draw on liquidity and before the CISD **[C]** |
| Too wide | Stop distance above 1.5 x Daily ATR(14) **[A]** -> setup rejected by default **[C]** |
| Fallback stop | High of the bearish candle that created the FVG + 5 to 10 pips **[C]**; 7 pips **[A]**. Scaled by pip size for JPY and metals **[A]** |
| Tight variant | Stop at sweep high (c) only if displacement after (c) creates a new FVG immediately **[C]**; "immediately" = within a few candles **[A]** |
| Fallback also too wide | Reject the setup **[A]** |
| Target | (b), the sell-side liquidity the sweep rallied from **[C]**; TP2 = Previous Week Low or next low beyond **[A]** |
| Risk filter | Account risk is lot-size dependent **[C]**; stop-distance rejection above is the quality filter **[C]**. `MAX_RISK_PCT = 2` is unchanged and not used here **[A]** |

CISD definition options (owner to choose): (1) close below the open of the last bullish candle or run that led into the sweep; (2) close below the sweep candle's low; (3) other. **OPEN.**

## Setup B: Nested Weekly/Daily manipulation (continuation with the week's intent)
| Element | Rule |
|---|---|
| Bias dictator | The manipulation state sets bias for Weekly and Daily **[C]** |
| Weekly manipulation | Sweep of a prior weekly high (or low) against the intended direction, then reversal **[C]** |
| Daily manipulation | Must align with the weekly manipulation **[C]**. Weekly down -> Daily manipulation is an up-sweep (e.g. Tuesday) **[C]** |
| Conflict | Weekly and Daily manipulation disagree -> NO SETUP **[A]** |
| Displacement | Strong move through the Open in the intended direction after the manipulation (Daily Open for Daily, Weekly Open for Weekly) **[C]** |
| POI / entry | FVG or order block left by the displacement; entry inside it **[C]**. Valid only if manipulation is confirmed first **[C]** |
| Stop | Above the manipulation high **[C]**. 5 to 10 pip buffer on top: OPEN |
| Target | Previous Week Low, i.e. (c) in the owner's chart **[C]** |
| Day-of-week | Tuesday manipulation shown in example; whether Monday or Wednesday also qualify: OPEN |
| Displacement must create an FVG | Same as Setup A: OPEN |
| Macro event context | No feed exists (COT 403, no central-bank data). Macro is a manual tag on a setup, not a computed input **[A]** |

## Relationship to the v7 engine
- ABC/QMR remains the long-term framework; Setup B is the short-term continuation within it **[C]**.
- These detectors would feed the watchlist and idea composer. They do not relax Rule 32, the permission thresholds, or the parser firewall.
- Every published level must trace to a sourced candle (Rule 2). The parser must reject unsupported levels.

## Watchlist output (per pair)
Pair | Direction | Setup (A or B) | Level being watched | Awaited condition | Status: WATCHING / TRIGGERED / INVALIDATED. No entry, stop or target is published until the setup is TRIGGERED and levels are sourced.

## OPEN items (owner decisions)
1. CISD definition.
2. Confirm 1.5 x Daily ATR(14) as "too wide" and 7 pips as buffer.
3. Setup B: buffer on the manipulation-high stop; strict Tuesday-only or any weekday; must displacement create an FVG.
4. Manipulation confirmed = close back inside prior range (vs wick only).
5. Single source and timezone for Daily Open and Weekly Open.
6. Backtest plan: replay past cutoffs with the as-of runner (stored outcomes are only 6 ideas and 3 results, all wins).

## Process
Define contract -> commit contract -> implement -> test -> update documentation (v7 handover Section 22). Methodology change requires explicit owner approval.
