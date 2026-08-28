# HipMarvin FX — Position Ledger Template v3

**v3 — HTF Trend / QMR sync · 2026-08-28**

The Position Ledger remains the single source of truth for open positions.
v3 adds the Rules 24–34 directional-eligibility fields carried from the
Weekly Trade Priority List, alongside the existing v2 Liquidity/Flow
fields, so the full execution context — location, flow, and now trend
eligibility — is auditable without allowing any of it to silently change
position status.

## Ledger row

| Pair | Direction | Entry | Stop | TP1 | TP2 | Timeframe | Execution TF | Correlation Class | Daily Zone | Tier | HTF Trend | Trend Alignment | Structural Break | QMR Phase | QM/QML Refinement | Liquidity/Flow Regime | Liquidity State | Zone/Flow Relationship | Status | Notes |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| [PAIR] | [LONG/SHORT] | [level] | [level] | [level] | [level] | [TF] | [1H/4H/Daily] | [class] | [X% / label] | [1/2] | [Bullish/Bearish/Transition/Conflict] | [With-trend/Countertrend after structural break/Not eligible] | [None/Confirmed bullish/Confirmed bearish/Not applicable] | [Quality/Manipulation/Reaction/Confirmed continuation/Confirmed reversal] | [Yes/No/Not applicable] | [RANGE/TRANSITION/DIRECTIONAL] | [state] | [relationship] | [OPEN/STOPPED/CLOSED @TP/CLOSED @breakeven] | [notes] |

## Field rules

**Unchanged from v2:**
- **Daily Zone:** copy the exact value from the Weekly Trade Priority List. Do not re-derive it in the Ledger.
- **Liquidity/Flow Regime:** copy the classification that existed when the idea was published or triggered.
- **Liquidity State:** copy the published state; update only when a documented daily research update materially changes the analytical state.
- **Zone/Flow Relationship:** preserve whether the idea is mean-reversion aligned, continuation aligned, or explicitly location-vs-flow conflict.
- **Status:** changes only under the existing status rules and actual recorded price action. A Flow Regime change never closes a trade by itself.

**New in v3:**
- **HTF Trend:** copy the Daily/4H trend classification that existed at the
  moment the idea was published or triggered. Do not re-derive it live in
  the Ledger — if the trend has since changed, that's a Rule 22 disclosure
  (see below), not a silent field overwrite.
- **Trend Alignment:** copy exactly. A row marked "Countertrend after
  structural break" must have a Notes entry (or a linked Daily Update)
  citing the specific broken level — the Ledger doesn't re-litigate the
  eligibility call, it records what was already established.
- **Structural Break:** copy the value as of trigger/publish time. If a
  break is later confirmed or invalidated by subsequent price action, log
  that as a new Notes entry with a date, not as an edit to this field in
  place — the row should show its history, not just its current read.
- **QMR Phase / QM/QML Refinement:** copy exactly, same discipline as
  above — these describe the setup at entry, not a live-updating read.
- **Execution TF:** copy the 1H/4H/Daily value the entry was actually
  confirmed on (Rule 24). Distinct from the pre-existing free-text
  "Timeframe" column, which is preserved for schema/database compatibility
  (matches the existing `trade_ideas.timeframe` field) — do not conflate
  the two or attempt to merge them without a separate, deliberate migration
  decision.
- **Notes:** use for Rule 22 thesis invalidation flags, Rule 18 same-pair
  opposing rows, material Rule 23 flow changes, and now material Rule
  24–33 trend/structure changes relevant to an open position (see examples
  below).

## Rule 23 position note examples (unchanged from v2)

- `per Day 3 Flow Regime: RANGE → DIRECTIONAL BULLISH; 20D Premium retained as location, not reversal signal.`
- `per Day 4: liquidity swept + rejected; continuation thesis weakened.`
- `per Rule 23: price accepted above prior buy-side liquidity; no automatic status change.`

## Rule 24–33 position note examples (new in v3)

- `per Day 4 HTF check: Daily/4H trend flipped Bearish → Bullish; this SHORT position's original with-trend basis no longer holds (Rule 22 flag — status unchanged, Rule 3 governs any actual close).`
- `per Rule 26: the countertrend LONG that would reverse this row's thesis has not cleared the structural-break bar — wick only, no follow-through confirmed as of Day 3. No action required on this row yet.`
- `per Day 5: QMR Reaction phase confirmed on the 4H, consistent with this row's original Quality/Manipulation read at entry; no change to Structural Break field.`

## Discipline

The Ledger records **position state**, not a live discretionary override
engine. Macro, zone, liquidity, flow, and now HTF trend/structure analysis
can each create a disclosure note or affect newly considered entries, but
none of them silently modify an existing Stop/TP/Status. A confirmed
structural break against an open position is exactly the kind of thing
Rule 22 exists for: record it, cite where it was established, and let Rule
3 (actual price action) or a separately logged discretionary close do the
rest.

## Compatibility note

v2 files remain the historical reference for prior weeks and for positions
opened before v7 was adopted — per `STANDING_PROTOCOL_v7.md`'s
historical-data policy, existing rows are not retrofitted with HTF/QMR
fields they were never recorded with. New rows, from Week 36 forward,
should use this v3 schema.
