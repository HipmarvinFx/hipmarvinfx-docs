# V7 Amendment 005 - Stop rule B and liquidity-pool target for breakout constructions

**Status:** IN FORCE by owner decision (Elijah Agom, 14 Nov 2026). **Recorded retrospectively** on 23 Nov 2026 from the code comments in `lib/engine/breakout-stop.ts` and `lib/engine/htf-pair-builder.ts` and the project record. The wording is a reconstruction: confirm it in review of this change.
**Nature:** a documented exception and tool-behaviour clarification (Rule 20). Rule text is not edited.

## Rule
When the default dealing-range construction cannot be used because its structural target is already consumed (price has broken out of the range), a retry is made with:
1. **Stop rule B.** LONG: stop = min(low of the FVG or order-block origin candle, swept level) minus buffer. SHORT: stop = max(high of the origin candle, swept level) plus buffer. The stop lies beyond the extreme of the origin candle and never inside the swept level. Buffer defaults to 0.
2. **Target 2 from a liquidity pool (Rule 33).** An explicit liquidity target wins; otherwise the nearest sourced pool beyond entry whose reward exceeds 1R. If no such pool exists, no target is stated and no idea is published.
3. **Target 1** at 2R when it lies before target 2, else 1R.

## Not changed
Rules 1-4, Rule 26, Rule 35, Amendment 003 and Amendment 004.