# HipMarvin FX — Position Ledger Template v2

**v2 — Liquidity/Flow Regime sync · 2026-08-19**

The Position Ledger remains the single source of truth for open positions. v2 adds the analytical state carried from the Weekly Trade Priority List so the execution context is auditable without allowing analysis to change position status.

## Ledger row

| Pair | Direction | Entry | Stop | TP1 | TP2 | Timeframe | Correlation Class | Daily Zone | Tier | Liquidity/Flow Regime | Liquidity State | Zone/Flow Relationship | Status | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [PAIR] | [LONG/SHORT] | [level] | [level] | [level] | [level] | [TF] | [class] | [X% / label] | [1/2] | [RANGE/TRANSITION/DIRECTIONAL] | [state] | [relationship] | [OPEN/STOPPED/CLOSED @TP/CLOSED @breakeven] | [notes] |

## Field rules

- **Daily Zone:** copy the exact value from the Weekly Trade Priority List. Do not re-derive it in the Ledger.
- **Liquidity/Flow Regime:** copy the classification that existed when the idea was published or triggered.
- **Liquidity State:** copy the published state; update only when a documented daily research update materially changes the analytical state.
- **Zone/Flow Relationship:** preserve whether the idea is mean-reversion aligned, continuation aligned, or explicitly location-vs-flow conflict.
- **Status:** changes only under the existing status rules and actual recorded price action. A Flow Regime change never closes a trade by itself.
- **Notes:** use for Rule 22 thesis invalidation flags, Rule 18 same-pair opposing rows, and material Rule 23 flow changes that are relevant to an open position.

## Rule 23 position note examples

- `per Day 3 Flow Regime: RANGE → DIRECTIONAL BULLISH; 20D Premium retained as location, not reversal signal.`
- `per Day 4: liquidity swept + rejected; continuation thesis weakened.`
- `per Rule 23: price accepted above prior buy-side liquidity; no automatic status change.`

## Discipline

The Ledger records **position state**, not a live discretionary override engine. Macro, zone, liquidity, and flow analysis can create a disclosure note or affect newly considered entries, but they do not silently modify an existing Stop/TP/Status.
