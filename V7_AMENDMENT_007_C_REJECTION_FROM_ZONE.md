# V7 Amendment 007 - C is the rejection from the nearest FVG, IFVG or order block after A is swept (DRAFT)

**Status:** DRAFT, not approved. Listed under "Not in force" in `V7_AMENDMENTS_IN_FORCE.md` until the owner approves it and sets the effective week.
**Number:** 007. 005 is Breakout Stop Rule B. 006 is reserved for the single-builder amendment (D1-D4).
**Effective:** the first weekly file generated after the engine change implementing this amendment merges to main. The week number is recorded here at that point.
**Authorship:** drafted by an AI session; the owner imports (Amendment 004, clause 5).
**Owner decisions (2026-11-28):** C rule accepted; zone definitions as a new engine function (existing entry-zone function unchanged); "nearest" means nearest to A; A definition below accepted; effective week tied to the engine merge.

## 1. What changes

`QMR_ABC_TECHNICAL_SPEC.md` Section 9 requires C to sit above A inside the 50%-75% premium area of the dealing range, and says FVG, order block and other confluence are not automatically mandatory gates. For the location of C only, this amendment overrides that:

- C is located by a rejection from a defined zone (FVG, IFVG or order block), not by the 50%-75% band.
- The 50%-75% band becomes a DISCLOSE flag.
- FVG, IFVG and order block become required for locating C. Swing liquidity, structural references and other documented confluence remain non-mandatory.

Spec text and Standing Protocol text are not edited (Rule 20). This amendment applies on top of them.

## 2. Definitions (bearish; bullish is the exact mirror)

- **B, dealing range, target:** unchanged.
- **A (owner-accepted):** the highest confirmed swing high (2-candle lookback) between B and the current candle, before any candle has traded above it. A is the liquidity waiting to be swept.
- **A swept:** a candle's high trades above A.
- **Gap (FVG):** a three-candle gap that passes the ATR filters: size at least MinGapATR x ATR, and the middle candle's range at least DispATR x ATR. Reference values (FVG indicator defaults): ATR period 14, MinGapATR 0.15, DispATR 0.80. The engine function must use the same values; a change to either side needs a parity check.
- **IFVG:** a gap that a later candle closes through (for a bearish C, a bullish gap closed through on the downside). Strict 1st IFVG applies: the closing candle must also close beyond the nearest earlier same-direction gap, and back inside the dealing range.
- **Order block:** the marking candle of the IFVG. A wick of 50% of the range or more is treated as a manipulation wick and the body edge is used (indicator default).
- **Sweep-to-confirmation window:** at most 40 candles (indicator default).
- **C zone:** the bearish FVG, IFVG or order block with its levels at or above A and not above DRH, nearest to A (smallest distance to A, not the newest).
- **Reaction candle:** a bearish close with a body of at least 10% of its range (unchanged).

## 3. The C rule

1. After A is swept, find the C zone.
2. C is the high of the candle that taps the C zone and is followed by the reaction candle.
3. If no zone qualifies, there is no C. The ABC stays at the stage before C. This is not a BLOCK.

## 4. Severity (per the 28 Oct severity model)

- C outside 50%-75% of the dealing range: **DISCLOSE**. The idea publishes with the visible label "C outside the 50%-75% band". Never BLOCK. This is the dealing-range premium band, not the 20-day Daily Zone, and it does not change Zone rules (28 Oct, item 3).
- No C found: not a BLOCK. The existing FALLBACK framework applies where ABC is absent.
- Machine check: [engine function name to be assigned]. Owner: the owner.

## 5. Relationship to other amendments

- **28 Oct, item 4 (trigger):** unchanged. "Triggered" is an entry touch on recorded price. This amendment only changes how ABC locates C.
- **28 Oct, FALLBACK framework:** fallback is gated from C onward. Because C is now harder to find, FALLBACK will apply more often. Its conditions are not changed.
- **Amendment 005 (Breakout Stop Rule B):** not affected. This amendment concerns the location of C, not stops or targets.

## 6. Unchanged

- The invalidation rules.
- The 10% sweep cap on B.
- The reaction-candle test.
- The dealing-range definition, and the 50%-75% default as a reference band.

## 7. Consequences

- Fallback gating starts at C and must be re-checked.
- Engine tests for C need new expected values from the fixture checker before any engine code changes.
- `buildExecutionZoneAnalysis` is not changed. The zone definitions above are a new function, so entry placement is unaffected.
- The engine's `targetB` (DRL) versus the chart's sweep-low target is a separate open item.

## 8. Not verified when drafted

- Engine and indicator code were not changed for this draft.
- The first fixture's new C value is not stated here. The fixture checker produces it.