# HipMarvin FX — Weekly Research Template v5.1

**Changelog:**
- **v5.1 (2026-10-04):** Harmonizes `WEEKLY_RESEARCH_TEMPLATE_v5.md` (v7 analysis content) with the importer's parser contract, as read from `app/admin/page.tsx` on `origin/main` (`b1ad0e6`), lines 841–1440. **Kept from v5:** the ten v7 idea fields, the Higher-Timeframe Trend Map, the Rule 26 eligibility gate, the Rule 27 reasoning sequence. **Restored from v3 (parser-visible shape that v5 had changed):** bold cycle fields with `Research Cycle Label` and `Week Start (ISO)` / `Week End (ISO)`; Macro Thesis on its own line; `Status: active`; bold-line Macro Drivers; per-event `## SCENARIO MATRIX — <event>` headers with `**Branch N —**` lines; four single-line Daily Game Plan slots. **Changed from v5 to fit the importer:** the conditional-idea format uses one line per field (v5 combined five fields on one line, which the importer cannot split); the two `###` check blocks inside the Trade Priority List became bold closing lines; `## RESEARCH CYCLE / CARRYOVER` became `## CARRYOVER`; `Timeframe` is kept alongside `Execution TF` (the importer and database both have a `timeframe` field; the Protocol's required list names only `Execution TF`). **Not verified:** importer code after line 1440 (calendar-event derivation, COT, Game Plan, publish) was not read in this pass; those sections follow the Week 34 / v3 format that is known to have imported. Per Rule 20 this is a deliberate, versioned schema change.
- **v5 (2026-08-28):** HTF Trend / QMR schema (Rules 24–35).
- **v3 (2026-08-17):** G10 Macro Regime, Weekly Thesis subsections, Macro Regime / Macro Alignment fields.
- **v2 (2026-07-26):** Timeframe, Correlation Class, Daily Zone, Tier fields.

> **Core rule (Rule 25):** Trend continuation is the default. A countertrend idea is not eligible for this list unless Rule 26 (confirmed HTF structural break) is satisfied. This is a hard gate, evaluated before an idea is written up, not a disclosure added afterward. Per Rule 32, unclear Daily/4H structure means WAIT / NO TRADE; the list may be shorter than usual, and a pair that is worth tracking but not eligible goes in the Daily Game Plan as "watching, not yet eligible."

> **Unchanged:** the 20-day range is location, not direction (Rule 21). Liquidity/Flow Regime, Liquidity State and Zone/Flow Relationship (Rule 23) are required and are copied verbatim to the Position Ledger.

---

## RESEARCH CYCLE
*(Parser note: bold lines, no leading dash. The importer reads `Research Cycle Label`, `Week Start (ISO)` and `Week End (ISO)` with the parentheses. The week number is read from the label text and the title month from the start date, so Week Start must be the Sunday the file's week begins. Status: write `active` while the week runs, `closed` afterward. Published must include a time, e.g. `Sunday, October 4, 2026, 21:00 WAT`; without one, no publish time is stored.)*

**Research Cycle Label:** Week [N] — [Day Range, Month Year]
**Week Start (ISO):** YYYY-MM-DD
**Week End (ISO):** YYYY-MM-DD
**Event Label:** [Short label for the week's dominant events]
**Impact Level:** [High / Medium / Low]
**Thematic Focus:** [One line — the core cross-pair theme this week]
**Overall Bias:** [Directional lean, or mixed/conditional across pairs]
**Status:** active
**Macro Thesis:**
[2–4 sentences on their own line, directly under the label. The text ends at the next line that starts with `**`, so leave a blank line before `**Analyst:**`.]

**Analyst:** Elijah Agom
**Published:** [Weekday, Month D, YYYY, HH:MM WAT]
**Import Status Note:** [Anything the importer/reviewer should know about completeness: missing COT, missing charts, an un-resupplied ledger, prior weeks not logged]

---

## PRICE STRIP
*(Presentational only. Sourced chart values only; label each as the analyst's own read per Rule 2.)*

| Pair | Level | Bias |
|------|-------|------|
| DXY | | |
| EUR/USD | | |
| GBP/USD | | |
| USD/JPY | | |
| XAU/USD | | |
| AUD/USD | | |
| NZD/USD | | |
| USD/CAD | | |

---

## G10 MACRO REGIME
*(Presentational only. Fill before the Weekly Thesis. Establishes the fundamental backdrop; it does not create a trade by itself.)*

### Macro Regime Snapshot

| Currency | Inflation vs Target | Inflation Trend | Labour Trend | Policy Pressure | Macro Regime |
|----------|---------------------|-----------------|--------------|-----------------|--------------|
| USD | [Above / At / Below] | [↑ / → / ↓] | [Strengthening / Stable / Weakening] | [Hawkish / Neutral / Dovish] | [Strong / Constructive / Neutral / Weakening / Weak] |
| EUR | | | | | |
| GBP | | | | | |
| JPY | | | | | |
| CHF | | | | | |
| CAD | | | | | |
| AUD | | | | | |
| NZD | | | | | |
| NOK | | | | | |
| SEK | | | | | |

### Macro Regime Ranking

**Strongest:** [Currency] → [Currency]
**Constructive / Improving:** [Currency] → [Currency]
**Neutral / Transitional:** [Currency] → [Currency]
**Weakening:** [Currency] → [Currency]
**Weakest:** [Currency]

### Institutional Macro Read

[3–5 sentences: inflation vs target; direction of inflation; labour direction; central-bank reaction-function pressure; strongest and weakest relative macro impulse.]

### FX Implication

[Which currencies are structurally favoured or sold this week. Does not create a trade by itself.]

### Macro Regime Changes From Previous Week

- [Currency]: [Previous] → [Current] — [reason]

*(If none: "No regime changes this week." State the baseline week when the previous file is more than one week old.)*

> **Macro Regime Rule:** The G10 Macro Regime establishes the fundamental backdrop; it does not create a trade by itself. Trade direction must still pass the Weekly Thesis, Macro Driver / Scenario Matrix, correlation, positioning, price-zone and risk controls. Macro Regime affects Setup Quality, not Entry Quality.

---

## HIGHER-TIMEFRAME TREND MAP
*(Presentational only. Rule 24. One row per pair with an active or candidate idea. Read from actual Daily and 4H charts; nothing below 1H.)*

| Pair | Daily Trend | 4H Trend | Trend Agreement | Classification |
|------|-------------|----------|-----------------|----------------|
| [PAIR] | [Bullish / Bearish / Choppy] | [Bullish / Bearish / Choppy] | [Agree / Conflict] | [Directional / Transition-Conflict] |

If Daily and 4H disagree, the pair is Transition / Conflict and is not forced into a direction from a lower timeframe.

---

## WEEKLY THESIS
*(Presentational only. Must be consistent with the G10 Macro Regime above.)*

### Core Macro Question

[One sentence.]

### Institutional Answer

[2–4 sentences: G10 macro regime → central-bank reaction function → relative currency strength → major FX implications.]

### Pair-Level Translation

- **USD:** [bias + macro reason]
- **EUR:** [bias + macro reason]
- **GBP:** [bias + macro reason]
- **JPY:** [bias + macro reason]
- **CHF:** [bias + macro reason]
- **CAD:** [bias + macro reason]
- **AUD:** [bias + macro reason]
- **NZD:** [bias + macro reason]
- **NOK:** [bias + macro reason]
- **SEK:** [bias + macro reason]

---

## MACRO DRIVERS
*(Parser note: bold lines, no leading dash, capped at 4 drivers. The Subline must contain day, month and WAT time, e.g. `Tue 6 Oct, 7:35am WAT`, or no calendar event is derived. The importer stores only the text on the same line as `**Analysis:**` and takes affected pairs from an `FX implication:` on that line, so keep the one-line summary. The five sub-lines below it are for readers and are not stored until the importer is changed; the duplicated FX implication is interim.)*

**Driver 1**
**Tag:** RELEASED / SCHEDULED
**Headline:** [Event name — Day, Date]
**Subline:** [Day-of-week D Mon, time WAT — Actual/Forecast/Prior]
**Analysis:** Regime tested: [Currency / regime]. FX implication: [pair(s) / direction / consequence]
- Macro regime being tested: [Currency / regime]
- Confirms regime if: [condition]
- Strengthens regime if: [condition]
- Weakens regime if: [condition]
- Breaks / invalidates regime if: [condition]
- FX implication: [pair(s) / direction / consequence]

**Driver 2** *(same fields)*

---

## TRADE PRIORITY LIST
*(Parser note: each field on its own dash line, labels exactly as written. Reasoning must be a single line. No `##` or `###` headings inside this section. Daily Zone format: `X% up the 20D range — Label`. Tier: 1 or 2. Evaluate Rules 24–26 before writing an idea; an idea that fails eligibility is not listed.)*

**Priority 1 — [Pair] [Long/Short/Buy/Sell] — ★★★★★**
- Entry: [price or zone]
- Stop: [price]
- TP1: [price]
- TP2: [price]
- R:R: [ratio]
- Timeframe: [chart timeframes used for general context, e.g. Daily + 4H]
- Execution TF: [1H / 4H / Daily — never below 1H]
- Correlation Class: [USD-quote group / USD-base group / Proxy-standalone / Other]
- Daily Zone: [e.g. 62% up the 20D range — Mild Premium]
- Tier: [1 or 2]
- Macro Regime: [e.g. USD Strong / EUR Weakening]
- Macro Alignment: [Aligned / Mixed / Contrarian]
- HTF Trend: [Bullish / Bearish / Transition / Conflict]
- Trend Alignment: [With-trend / Countertrend after structural break / Not eligible]
- Structural Break: [None / Confirmed bullish / Confirmed bearish / Not applicable]
- QMR Phase: [Quality / Manipulation / Reaction / Confirmed continuation / Confirmed reversal]
- QM/QML Refinement: [Yes / No / Not applicable]
- Liquidity/Flow Regime: [RANGE / TRANSITION / DIRECTIONAL]
- Liquidity State: [Untaken liquidity / Liquidity swept + rejected / Liquidity swept + accepted / Sequential liquidity consumption / Transition / confirmation pending]
- Liquidity Target: [level or pool, sourced from chart; Rule 33]
- Zone/Flow Relationship: [Mean-reversion aligned / Continuation aligned / Location vs flow conflict — continuation justified / Location vs flow conflict — reversal not yet confirmed / Neutral / insufficient evidence]
- Reasoning: [One line, following Macro Regime → Weekly Thesis → HTF Trend/Structure → 20D Location → Liquidity Map → QMR → Displacement/Acceptance or Rejection → QM/QML Refinement → Liquidity Target → Risk. State plainly any Rule 21B conflict (Buy in Premium, Sell in Discount) and any conflict between steps. For a Countertrend idea, name the broken HTF level, the displacement/acceptance evidence, the follow-through and the QMR reaction or retest (Rule 26).]

*(Repeat per idea, ranked by conviction.)*

### Conditional / dual-trigger format
*(Parser note: its own parsing path. The trigger lines stay on one line each, in this exact pattern. One line per field; do not combine fields on one line. Importer limitation: per-side labels such as `HTF Trend (long side)` are not matched, so both sides currently receive the first (long-side) value. Until the importer is fixed, state in Reasoning anywhere the short side differs.)*

**Priority N — [Pair] conditional, both sides — ★★★☆☆**
- Long trigger / TP / Stop: close above [price] → target [price] → stop [price]
- Short trigger / TP / Stop: close below [price] → target [price] → stop [price]
- Timeframe: [e.g. Daily + 4H]
- Execution TF: [1H / 4H / Daily]
- Correlation Class: [value]
- Daily Zone: [X% up the 20D range — Label]
- Tier: [1 or 2]
- Macro Regime (long side): [regimes]
- Macro Alignment (long side): [Aligned / Mixed / Contrarian]
- Macro Regime (short side): [regimes]
- Macro Alignment (short side): [Aligned / Mixed / Contrarian]
- HTF Trend (long side): [value]
- Trend Alignment (long side): [value]
- Structural Break (long side): [value]
- QMR Phase (long side): [value]
- QM/QML Refinement (long side): [value]
- HTF Trend (short side): [value]
- Trend Alignment (short side): [value]
- Structural Break (short side): [value]
- QMR Phase (short side): [value]
- QM/QML Refinement (short side): [value]
- Liquidity/Flow Regime: [RANGE / TRANSITION / DIRECTIONAL]
- Liquidity State: [state]
- Liquidity Target: [level or pool]
- Zone/Flow Relationship: [relationship]
- Reasoning: [One line. What confirmation would convert each conditional side into an eligible trade; for a countertrend side, what a qualifying structural break would look like.]

**Correlation check (Rule 5):** [Plain statement: are 2+ ideas one correlated bet, and does the list lean against the Scenario Matrix's base case?]
**Zone alignment check (Rule 21):** [Plain statement: is any idea fighting its own zone? The 20D range is location, not direction.]
**HTF eligibility check (Rules 24–26, 31):** [Plain statement: Daily/4H agreement per idea; any countertrend candidate and whether Rule 26 was met; any textbook QM/QML pattern seen and rejected because HTF structure was unbroken, named rather than omitted.]

---

## SCENARIO MATRIX — [Exact FF event name]
*(Parser note: one section per red-folder event. The header text after the dash is the match key; put a disambiguating word (country or currency) in the header itself, never in a trailing parenthetical. Branch lines must follow this exact pattern, bold-wrapped and ending in `:**`; the text after it may carry the v5 items in one flow. No headings inside.)*

**Branch 1 — [Short label] (X% probability):** Macro trigger: [what the release or statement does]; liquidity destination: [pool the market is expected to reach]; flow regime: [expected]; confirmation: [what must be seen]; invalidation: [what cancels it]; trade implication: [pairs / direction / action]
**Branch 2 — [Short label] (Y% probability):** [same items]

*(Probabilities sum to 100%; state whether they are the analyst's own estimate or sourced. Repeat one section per event.)*

---

## COT POSITIONING
*(Parser note: rows with at least four cells are stored, and any row whose first cell does not contain "pair" is treated as data. If COT data was not supplied, write the single sentence below and no table, since a placeholder row would be stored as a pair named "—".)*

COT data not supplied this cycle — see Import Status Note.

| Pair | Net Position (Leveraged Funds) | Net Position (Asset Mgr/Institutional) | Direction (wk/wk change) | Read |
|------|--------------------------------|------------------------------------------|----------------------------|------|

---

## DAILY GAME PLAN
*(Parser note: four bold slots only, each on a single line: Monday/Tuesday combined, Wednesday, Thursday, Friday. A standalone Tuesday field is not captured until the Tuesday column migration is run. Forward-only per Rule 6; resolve nothing. Inside each slot, cover: catalyst and watch window; liquidity pool to monitor; expected flow regime; HTF structure to watch (only if a countertrend candidate exists); confirmation required; reassessment trigger; no-trade condition. Candidates worth tracking but not yet eligible go here as "watching, not yet eligible.")*

**Monday/Tuesday:** [one line]
**Wednesday:** [one line]
**Thursday:** [one line]
**Friday:** [one line]

---

## CARRYOVER
*(Presentational only. Rule 7: carryover is noted here, resolved in the Position Ledger. Open positions at publication, as listed in the Ledger.)*

- [Pair / direction / status per Ledger / any same-pair opposite-direction flag (Rule 18)]

---

## POSITION LEDGER LINK
*(Presentational only.)* When a new idea is published, Rule 17 requires the Ledger row to be created at the same time. Carry over Daily Zone, Liquidity/Flow Regime, Liquidity State, Zone/Flow Relationship, HTF Trend, Trend Alignment, Structural Break, QMR Phase, QM/QML Refinement and Execution TF exactly as written. Do not re-derive any of them downstream.

---
**Sources:** [FF screenshot date/time · web sources for speeches · chart timestamps (Daily and 4H per pair) · COT report date or "not supplied" · Position Ledger status]

---

## Interpretation note
Evaluate in this order: Rules 24–26 first (is the idea eligible at all: trend-following, or countertrend with a confirmed break), then Rule 21's zone check and Rule 23's flow-regime check on whatever survives. An idea can fail the first gate and never reach the second. An idea that passes can still carry a zone or flow caveat; those remain soft flags disclosed in Reasoning, not blockers.

## Known importer limitations (for the developer; not part of the research record)
1. Per-side dash labels with parentheses (`HTF Trend (long side)`) are used as regular expressions, so they never match; short-side fields fall back to the long-side value.
2. `**Analysis:**` is read from its own line only, so sub-lines under it are not stored.
3. `Reasoning` is read from one line only.
4. No Tuesday column yet (migration pending).
5. In the range read, there was no code for the G10 Macro Regime, Higher-Timeframe Trend Map or Weekly Thesis sections; they are treated as presentational. Code after line 1440 was not read.

## Compatibility note
v4 and v5 files remain the historical reference. New research should use this v5.1 schema with `STANDING_PROTOCOL_v7.md` and `HipMarvinFX_Generation_Prompts_v7_0.md`; those prompts should be re-synchronized to v5.1 before the next generation run (Generator–Template Synchronization Principle).
