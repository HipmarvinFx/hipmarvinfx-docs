# HipMarvin FX — v6 Parser Fixtures

These fixtures are acceptance examples for the parser/build implementation. They are not market recommendations.

## Fixture A — Premium continuation

```text
**Priority 1 — GBP/USD Long — ★★★★★**
- **Entry:** 1.3520–1.3540
- **Stop:** 1.3450
- **TP1:** 1.3600
- **TP2:** 1.3660
- **R:R:** 2.1
- **Timeframe:** 4H
- **Correlation Class:** USD-quote group
- **Daily Zone:** 82% up the 20D range — Deep Premium
- **Tier:** 1
- **Macro Regime:** GBP constructive / USD weakening
- **Macro Alignment:** Aligned
- **Liquidity/Flow Regime:** DIRECTIONAL
- **Liquidity State:** Liquidity swept + accepted
- **Zone/Flow Relationship:** Location vs flow conflict — continuation justified
- **Reasoning:** Buy-side liquidity was taken and accepted; price held above the level, formed a higher low, and is targeting the next liquidity pool.
```

Expected normalized values:

- `direction = LONG`
- `daily_zone.percent = 82`
- `daily_zone.label = Deep Premium`
- `liquidity_flow_regime = DIRECTIONAL`
- `liquidity_state = Liquidity swept + accepted`
- `zone_flow_relationship = Location vs flow conflict — continuation justified`

Expected public card:

`Premium / Bullish Continuation`

## Fixture B — Premium rejection

```text
**Priority 2 — GBP/USD Short — ★★★★☆**
- **Entry:** 1.3550–1.3560
- **Stop:** 1.3610
- **TP1:** 1.3490
- **TP2:** 1.3440
- **R:R:** 1.8
- **Timeframe:** 1H
- **Correlation Class:** USD-quote group
- **Daily Zone:** 84% up the 20D range — Deep Premium
- **Tier:** 2
- **Macro Regime:** GBP neutral / USD neutral
- **Macro Alignment:** Mixed
- **Liquidity/Flow Regime:** RANGE
- **Liquidity State:** Liquidity swept + rejected
- **Zone/Flow Relationship:** Mean-reversion aligned
- **Reasoning:** Price swept the prior high and returned inside the range with no sustained acceptance.
```

Expected public card:

`Premium / Reversal Watch`

## Fixture C — Transition

```text
**Priority 3 — EUR/USD conditional, both sides — ★★★☆☆**
- **Long trigger / TP / Stop:** close above 1.1700 → target 1.1760 → stop 1.1650
- **Short trigger / TP / Stop:** close below 1.1630 → target 1.1570 → stop 1.1680
- **Timeframe / Correlation Class / Daily Zone / Tier:** 4H / USD-quote group / 68% Mild Premium / 2
- **Macro Regime (long side):** EUR constructive / USD weakening
- **Macro Regime (short side):** EUR weakening / USD strengthening
- **Macro Alignment:** per side
- **Liquidity/Flow Regime:** TRANSITION
- **Liquidity State:** Transition / confirmation pending
- **Zone/Flow Relationship:** Neutral / insufficient evidence
- **Reasoning:** A meaningful sweep occurred, but acceptance is incomplete; wait for confirmation.
```

Expected public card:

`Transition / Wait for Confirmation`

## Fixture D — Historical v5

```text
**Priority 1 — GBP/USD Long — ★★★★☆**
- **Entry:** 1.3500
- **Stop:** 1.3440
- **TP1:** 1.3600
- **Daily Zone:** 84% up the 20D range — Deep Premium
- **Tier:** 1
- **Macro Regime:** GBP constructive / USD weakening
- **Macro Alignment:** Aligned
- **Reasoning:** Historical v5 setup.
```

Expected normalized values:

- `schema_version = 5.x`
- `liquidity_flow_regime = NOT_RECORDED`
- `liquidity_state = NOT_RECORDED`
- `zone_flow_relationship = NOT_RECORDED`
- no inferred flow direction

## Fixture E — Daily transition

```text
### Liquidity / Flow Regime Check
- **Pair(s):** GBP/USD
- **20D Structural Location:** 82% up 20D range — Deep Premium
- **Prior Flow Regime:** RANGE
- **Current Flow Regime:** DIRECTIONAL
- **Regime Change:** RANGE → DIRECTIONAL
- **Liquidity State:** Liquidity swept + accepted
- **Liquidity Target / Pool:** prior weekly high
- **Acceptance / Rejection Evidence:** breakout held; retracement failed to reclaim prior range high
- **Zone/Flow Relationship:** Location vs flow conflict — continuation justified
- **Execution Implication:** continuation
```

Expected parser behavior:

- update daily flow observation;
- do not rewrite the weekly thesis automatically;
- do not close or alter an open Ledger position automatically.
