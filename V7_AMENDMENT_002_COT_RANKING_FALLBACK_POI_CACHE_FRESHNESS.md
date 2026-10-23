# HipMarvinFX v7 — Methodology Amendment 002
## COT Positioning Bias in Pair Ranking · Fallback Watchlist Points of Interest · Cache-Hit Freshness Admission

**Status:** DRAFT v0.2 — NOT APPROVED. Not in force until Section 9a is completed and this file is committed to `HipmarvinFx/hipmarvinfx-docs`.
**Date drafted:** 23 October 2026 (v0.2 update the same day)
**v0.2 changes:** C.3 resolved (HEAD admitted cache hits without freshness assertions); B.6 as-of candle identity resolved; J2/J3 confirmed untested; J6 coverage stated precisely; new open items for the weekly COT-ingest relocation and the `AGENTS.md` wording conflict.
**Depends on:** Amendment 001 (ABC primary, QMR diagnostic-only). Amendment 001 is itself not yet approved.
**Authority:** v7 Developer Handover — contract first, then commit, then implement; frozen methodology changes need an explicitly approved decision.

Tags used below: **[V]** output seen by the reviewer; **[C]** Copilot's report, not independently confirmed; **[U]** unverified.

---

## 1. Why a separate amendment

Amendment 001 covers only QMR→ABC. After it was drafted, the working tree gained three further subsystems. They are different in kind (new data input to ranking, new output type, a change to data admission), so they are recorded and approved separately rather than folded into 001.

## 2. Changes covered

| | Change | Primary files |
|---|---|---|
| A | COT positioning bias used as a pair-ranking input | `lib/engine/fundamental-bias.ts` (new), `pair-discovery.ts`, `macro-cot.ts`, `lib/evidence/cot.ts` |
| B | Fallback watchlist points of interest (FVG, order block, key level) when ABC is absent | `htf-pair-builder.ts`, `htf-evidence-packet.ts`, `market-context.ts` (new), `htf-composite.ts`, `best-expression.ts`, `write-htf-evidence.ts`, AI prompt files |
| C | Freshness assertion on cache-hit paths in the daily and weekly pipelines | `lib/pipeline/daily.ts`, `weekly.ts` |

## 3. Proposed decisions

### A. COT positioning bias

1. **Name and scope.** The input is *COT positioning bias* (CFTC data only). It is not a macro-fundamental ranking: rates, inflation, central-bank and news feeds are not built, and the August handover forbids presenting price-derived or partial inputs as macro fundamentals.
2. **Admission.** A bias is VERIFIED only when the COT report is COMPLETE, the pair's rows are COMPLETE, and provenance timestamps are valid (not future-dated, within the COT freshness window). Anything else yields NOT_AVAILABLE, score 0, and no ranking effect. Neutral netting yields direction null and no ranking effect. [C] with test coverage in `fundamental-bias.test.ts` [V, 6 tests pass].
3. **Effect.** A VERIFIED, directional bias adjusts `compositeScore` by `±min(20, score/5)`, only when an HTF preferred direction exists (agrees: add; disagrees: subtract). [C, quoted `pair-discovery.ts:78-85`]
4. **Limits.** The bias never changes `eligible` (computed from HTF alignment only). It cannot turn an HTF-ineligible pair into SELECT.
5. **Consequence to confirm.** By the thresholds read (SELECT ≥ 45, WATCH ≥ 25): a WITH-TREND row has a pre-bias floor of 65 and cannot fall below 45; a countertrend-after-structural-break row has a floor of 40 and can move to 60 (SELECT) with supporting COT or to 20 (PASS) with opposing COT. PASS removes the pair from best expressions. **The owner must confirm that COT may demote a countertrend row to PASS.** [derived from code read; boundary behaviour not yet seen in a test]

### B. Fallback watchlist points of interest

1. **Emission.** Only when the ABC candidate status is ABSENT. Developing or confirmed ABC emits none. [V, test]
2. **Kinds and sources.** FVG and order block from validated 4H candles; key levels from validated daily context. Zones are DERIVED.
3. **Non-executable.** Every point carries `executable: false` (typed as a literal in the packet). Points are excluded from `readyForAiSynthesis`, are never mapped to entry/stop/target, and are presented under a separate watchlist heading. [C, consumer trace]
4. **Data conditions.** Candles must be VERIFIED, correctly ordered, finite and consistent (high ≥ open/close ≥ low), not future-dated, with exact `symbol`/`timeframe` match, and must pass the 48h daily / 24h 4H freshness limits. Stale 4H withholds zones; stale daily withholds key levels, independently. [V, test]
5. **Classification decision required.** Key levels are currently all labelled FACT. Daily open and previous-day high/low are direct candle values; weekly/monthly opens and previous-week/month highs/lows are aggregated from candles. Under the v7 taxonomy aggregation is DERIVED. **The owner must decide the label** (recommend DERIVED for aggregated levels, FACT only for direct provider OHLC).
6. **Time basis.** The builder currently evaluates freshness with wall-clock time. As-of and historical reconstruction call the same builder, so replays withhold points built from old candles and results vary with run time. **Recommended:** an optional as-of `nowMs` passed through `buildHtfForPairs`. [C: as-of path confirmed to call the builder] As-of candles do carry the identity and status the builder requires (`toNormalizedOhlc` sets symbol, timeframe and status VERIFIED), so the remaining as-of issue is time basis only. [C]

### C. Cache-hit freshness admission

1. A cached series must pass the same integrity and freshness assertions as a freshly fetched one before it is admitted: daily (≥ 20 candles, synthetic-spread check, `assertFreshDaily`) and 4H (≥ 10 candles else skipped, `assertFresh4H`). [C, quoted `daily.ts:81-90, 138-143`; `weekly.ts:65, 100-105`]
2. A short cached series is skipped, not admitted. No `assertFresh*` call was removed from the fetch paths. [C]
3. **Recorded:** HEAD admitted cached daily and 4H series without the freshness assertions (the HEAD cache branches set the data and continued); the assertions ran only on fetched data. [C, checked against `git show HEAD:`] The cache TTL is 5 minutes (`lib/market-data/cache.ts:8`), which bounds how long data sits in the cache but does not guarantee candle freshness. How stale data reached the cache is not shown. [U]

## 4. Not changed

HTF alignment, structural-break gate, ABC reaction-confirmation requirement, the R:R floor to TP2 of 1.0 in `permission.ts`, the freshness limits, the evidence-status model, and the rule that only `technicalPermission` establishes executability. Fallback points never reach trade construction or permission, so they do not bypass those gates. [C]

## 5. Invariants (testable)

- **J1.** A bias that is not VERIFIED, or whose direction is null, leaves `compositeScore` unchanged. [test exists for stale, incomplete and neutral cases]
- **J2.** A bias never changes `eligible` or `eligibleDirection`. [confirmed missing: the verified-bias test asserts score direction and checks `eligible` on one row only, not against the unbiased baseline — required]
- **J3.** A VERIFIED opposing bias on a countertrend row yields PASS; a supporting bias yields SELECT; effect confined to the ±20 cap. [confirmed missing: no test asserts a decision change, and none covers a countertrend row — required]
- **J4.** No fallback point is executable; none exist when ABC status is not ABSENT. [test exists]
- **J5.** Stale or unverified candles withhold dependent points. [test exists]
- **J6.** A cache hit older than the limit throws on daily and 4H, in both pipelines. [tests exist for a stale cached daily and a stale cached 4H series in both pipelines, and a stale fetched daily series in the daily pipeline; NOT covered: a stale fetched 4H series in either pipeline, a stale fetched daily series in the weekly pipeline]
- **J7.** Fallback points and COT bias never alter `tradeConstruction` or `technicalPermission` output. [required]

## 6. Verification status

| Check | Result | Basis |
|---|---|---|
| Per-file added/removed line counts match Copilot's inventory exactly (42 tracked files, 18 untracked lib/.github files) | Match | [V] cross-check with `git diff --numstat` |
| New tests (`fundamental-bias`, `htf-pair-builder-fallback`) | 11 passed | [V] |
| Full `vitest`, `tsc`, `build` on the current tree | Not run | — |
| Copilot's code quotes and consumer trace | Consistent with the diffs seen | [C] |
| HEAD admitted cache hits without freshness assertions | Reported | [C] |
| Cached stale daily / 4H rejection tests, both pipelines | Exist; assertion lines quoted | [C] |
| Pair-discovery tests | 4 tests; none asserts a decision change or eligibility invariance under bias | [C] |
| Earlier sample comparison (Amendment 001, B1) | Did not exercise fallback points: the harness set candle `symbol` and `timeframe` to empty strings, which this builder withholds | [V] |

## 7. Open items

1. Owner decisions: A.5 (COT may demote countertrend rows), B.5 (key-level classification), B.6 (as-of time basis).
2. Read where the weekly pipeline's COT ingest (`ingestLatestCotEvidence`, a write-capable call) and evidence load were relocated, and confirm they remain guarded by the persist flag. Copilot reports them relocated, not removed. [U] Copilot also reports a BOM on the replaced import lines in `daily.ts` and `weekly.ts`; cosmetic, strip or accept.
3. Add tests for J2, J3, J7 and the uncovered J6 cases (confirmed missing).
4. Resolved: as-of candle identity is satisfied (B.6). `yahoo-historical.ts` now fetches 1H in 30-day chunks; whether chunk boundaries can duplicate bars is not shown. That file belongs in a separate commit.
5. `htf-composite.ts` (+16) and `market-context.ts` (+160): confirm what they feed into the composite and that no gate input changed. [U]
6. Whether generated AI prose could present a point as a trade level cannot be determined from the serializers. [U]
7. Exclude from the code commit: the five `*.pre-*-backup` files, `AGENTS.md`, `.github/copilot-instructions.md` (pending owner decision), and the unrelated app/config changes listed in Amendment 001, plus `yahoo-historical.ts`.
8. `AGENTS.md` strategy item 1 says fundamentals set pair priority using evidence-backed macro drivers, but only COT positioning is built. Align the wording, or hold the file out of the commit, so an agent file does not assert methodology ahead of the contract.

## 8. Rollback

Revert the code commit. COT bias is additive (removing it restores the prior scoring); fallback points are an optional packet field; cache admission checks can be reverted independently of A and B.

## 9. Approval and audit records

### 9a. Approval record (completed BEFORE the docs commit)

| Field | Entry |
|---|---|
| Decision | ☐ Approved ☐ Rejected ☐ Approved with changes |
| Amendment 001 status at approval | ____________ |
| Owner decisions A.5 / B.5 / B.6 | ____________ |
| Approved by / date | ____________ |

### 9b. Post-commit audit record (completed AFTER commits)

| Field | Entry |
|---|---|
| Docs commit hash | ____________ |
| Code commit hash | ____________ |
| Files staged (explicit paths) | ____________ |
| Post-commit `vitest` / `tsc` / `build` | ____________ |

---

*End of amendment.*
