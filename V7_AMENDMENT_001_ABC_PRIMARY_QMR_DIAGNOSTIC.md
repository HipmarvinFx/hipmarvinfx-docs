# HipMarvinFX v7 — Methodology Amendment 001
## ABC Primary Technical Setup; QMR Diagnostic-Only

**Status:** DRAFT v0.5 — NOT APPROVED. Not in force until Section 9 (Approval record) is completed and this file is committed to `HipmarvinFx/hipmarvinfx-docs`.
**Release blockers (must be closed before approval):** (B1) SELECT/WATCH/PASS sample check, (B2) dedicated I4 freshness/validity coverage.

**v0.5 changes:** B1 ABC gap closed by direct observation (NZDCHF `abcFormation` = TARGET_ACTIVE); new open question on R:R for the READY pair; new test sources read (5c); corrected the v0.1 verification-table claim that freshness-bearing files were outside the diff (`lib/pipeline/daily.ts` and `weekly.ts` are modified); extra modified file (31 vs 30) logged. Approval status unchanged.

**v0.4 changes:** B2 test result recorded (Section 5c). Corrected an overstatement in v0.3: it said existing tests had no stale-throws coverage, but only `freshness-order.test.ts` had been read; `freshness.test.ts` and `freshness-4h.test.ts` already covered stale daily/4H cases (per Copilot's reading). Approval status unchanged.

**v0.3 changes:** B1 result recorded (Section 5b); B2 test plan specified (Section 6). Approval status unchanged.

**v0.2 changes:** I1 reworded to decision-level invariance; I4 and the sample check promoted to release blockers; approval record separated from commit hashes (Section 9).
**Date drafted:** 22 October 2026
**Amends:** v7 methodology locked in `V7_EVIDENCE_ARCHITECTURE_AND_IMPLEMENTATION_DIRECTIVE.md` (frozen items: "QMR sequencing", "QM/QML refinement")
**Authority:** Required by the v7 Developer Handover (28 Aug 2026): frozen methodology changes need a separate, explicitly approved methodology-change decision, and contracts are committed before implementation.

---

## 1. Decision proposed

1. **ABC is the primary technical setup.**
2. **QMR (phase and eligibility) is diagnostic only.** It may be computed, stored, displayed and passed to the AI as context. It must not gate setup eligibility, permission, trade construction, pair discovery selection, or any score.
3. **QM/QML, FVG, order blocks and key levels are supporting or fallback evidence,** used only when explicitly present in the evidence packet. They never override a retained gate.
4. **Fundamentals remain the directional priority.**

## 2. What changes

| Area | Before | After |
|---|---|---|
| Trade construction | Returned invalid unless `qmrEligible` and a confirmed QMR phase | No QMR condition. Still requires a deterministic direction and ABC reaction confirmation (`PARTIAL` otherwise) |
| Pair discovery eligibility | Derived from QMR-based `eligibleDirections` | `htfEligible`: trend alignment present, not `NOT-ELIGIBLE`, with a LONG or SHORT preferred direction. No QMR input |
| Pair discovery output | Label stamped `[eligible]` / `[REJECTED]` from QMR | QMR shown as `QMR diagnostic <phase>` text only |
| ABC formation | — | States QMR is not an ABC prerequisite |
| QM/QML classifier | QMR inputs part of classification context | HTF direction is the only gate; QMR values appended as diagnostic evidence strings |
| AI prompts and adapters | QMR presented as part of the setup chain | ABC primary; QMR labelled diagnostic; AI told not to use QMR as an eligibility gate |

## 3. What does NOT change (retained gates)

- HTF-first analysis; Daily / 4H / 1H minimum framework; trend-first doctrine
- Independent HTF trend alignment requirement
- Structural-break countertrend gate
- ABC reaction-confirmation requirement
- 20D location, liquidity and flow regime
- Premium ≠ automatic short; Discount ≠ automatic long
- Minimum 1H execution timeframe (no sub-1H)
- Evidence statuses (VERIFIED / PENDING / STALE / NOT_AVAILABLE / INVALID), provenance, freshness, and the v7 parser firewall
- Standing Protocol Rules 1–4 (never fabricate, sourcing separation, status from price action, every idea has a stop)

**Explicit prohibition:** stale or unverified data must not be made eligible to increase setup availability. Any change to freshness or validity gates requires its own amendment.

## 4. Invariants (testable)

- **I1.** For identical non-QMR inputs, the eligibility, permission and trade-construction **decisions** (eligibility flag, permission status, construction status and its levels) are identical across all QMR phases (including undefined) and all `qmrEligible` values (true, false, undefined). Diagnostic fields that echo QMR (evidence strings, `qmrPhase` pass-through) may differ, because QMR remains visible as diagnostic information. *Note:* the tests reported by Copilot assert the stricter full-result equality in those two modules; that is acceptable, but only the decision-level form is the contract, so a future diagnostic echo must not be treated as a violation.
- **I2.** No QMR value appears in an `if`, filter, sort, or score term that decides eligibility, in `lib/engine/` or `lib/ai/`.
- **I3.** The AI prompt text must not describe QMR as a gate or prerequisite.
- **I4 (RELEASE BLOCKER).** ABC and HTF alignment passing with stale or invalid evidence must still be blocked. The permission and trade-construction modules take no freshness input, so this must be enforced and tested where freshness lives (`lib/integrity`, evidence validator, parser), or at the point where those modules' inputs are assembled. Required: at least one dedicated test in which ABC and HTF alignment both pass and evidence is STALE or INVALID, asserting no eligible setup is produced. Until that test exists and passes, this amendment cannot be approved.

## 5. Verification status (as of drafting)

| Check | Result | Basis |
|---|---|---|
| `npx vitest run` | 123 files, 863 tests passed | Run output seen |
| `npx tsc --noEmit` | exit 0 | Run output seen |
| `npm run build` | exit 0 | Run output seen |
| Tracked-file hash guard around verification run | PASS, 438 files identical | Run output seen |
| QMR read audit of 8 engine files | No QMR value reaches a gating condition | Copilot report; consistent with the diff hunks seen; not independently re-read in full |
| I1 invariance matrices (18 combinations each, permission and trade-construction) | Added | Copilot report; diff not reviewed |
| Removed-lines keyword scan | Only gate-keyword code removal was the QMR rejection in trade-construction | Output seen; keyword-based, not exhaustive |
| Freshness gates | `lib/pipeline/daily.ts` and `weekly.ts`, which call the freshness asserts, ARE in the modified set (4 lines each); their diffs have not been read. Daily-path behaviour is covered by `daily-freshness.test.ts` (5c) | Corrected v0.5; earlier text wrongly said the diff did not touch freshness files |

### 5b. Blocker B1 — sample check result (22 Oct 2026)

**Method.** One live Yahoo fetch frozen to a file (taken 2026-10-22T09:22Z, 30 pairs). The same candles were run through the working tree and through committed HEAD (extracted with `git archive` to a temp folder). Flow, liquidity and location maps were empty, so discovery scores omit the production bonuses. The harness bypasses the pipeline freshness asserts and labels every candle VERIFIED.

| | HEAD (baseline) | Working tree |
|---|---|---|
| Pair discovery | SELECT 6, WATCH 1, PASS 23 | SELECT 6, WATCH 1, PASS 23 |
| Trend alignment | WITH-TREND 6, COUNTERTREND 1, NOT-ELIGIBLE 23 | identical |
| Trade construction | INELIGIBLE 30 | READY 1 (NZDCHF), PARTIAL 4 (USDCAD, EURGBP, EURAUD, NZDUSD), INELIGIBLE 25 |

**Observations.**
- No collapse to all-SELECT. Discovery decisions and alignment are identical per pair before and after.
- The refactor's effect appears in trade construction. At HEAD no pair could pass; now one is READY and four are PARTIAL (ABC reaction confirmation absent). ABC still thins the field: 1 of 6 HTF-eligible pairs is READY.
- The READY pair (NZDCHF) had baseline QMR phase QUALITY, an unconfirmed phase. It was blocked at HEAD by the QMR gate and is READY now on the ABC path alone. This is the substantive methodology consequence the owner is being asked to approve.

**Caveats.**
- Baseline is committed HEAD, so the comparison covers all uncommitted `lib/` changes, not only the QMR edit. Discovery output was identical, so no effect from `structural-break.ts` or `trend-alignment.ts` was observed on this snapshot.
- The ABC lifecycle column was blank in the first run because the harness read `abcFormation.lifecycle`, but the packet's `abcFormation` is the lifecycle string itself. Read directly afterwards (working-tree code, frozen snapshot): NZDCHF `abcFormation` = TARGET_ACTIVE, and the trade-construction evidence reads "ABC reaction confirmation is present." The gap is closed.
- Daily series for AUDUSD, EURAUD and GBPAUD were labelled STALE by the adapter. EURAUD is one of the six SELECT pairs; the harness did not apply the pipeline freshness gate. NZDCHF, the READY pair, was VERIFIED on both series.
- GBPJPY and CADJPY are WITH-TREND SELECT but trade-construction INELIGIBLE; the cause was not examined.
- **Open question, not examined:** NZDCHF is READY with riskReward 0.37 (entry 0.46670, stop 0.46203, target2 0.46845, target1 NOT_AVAILABLE). Whether trade construction or permission is meant to enforce a minimum R:R was not checked; at HEAD no pair could reach READY, so the question could not previously arise. The packet's `technicalPermission` for this pair was not printed.
- Single snapshot, single market state.

### 5c. Blocker B2 — I4 coverage (22 Oct 2026; test sources read from pasted output; execution results per Copilot report)

- `lib/integrity/freshness-stale.test.ts` (new): exact boundaries (just inside the 48h daily / 24h 4H limits passes, beyond throws), weekend exclusion, unreadable timestamps. Pre-existing tests already covered stale daily/4H, Friday-to-Monday weekend handling and unreadable timestamps; the new file adds the exact-limit cases.
- `lib/pipeline/daily-freshness.test.ts` (new): drives `runDailyPipeline` offline through the existing `config.marketData.fetchOHLC` seam with one stale pair and five fresh pairs. The stale pair gets a "Stale daily" error and is absent from the pair-discovery rows and the HTF packets; all five fresh pairs remain in both. Pipeline evidence I/O is stubbed inside the test.
- No non-test file was edited, per the report. Full suite: 125 files, 868 tests, exit 0 (previously 123 / 863; +2 files, +5 tests, consistent with the report).

**Read of the test source.** The stale pair (EURUSD) is 10 days old, not borderline. Fresh pairs use synthetic candles. Mocks cover the COT, calendar, central-bank, macro, research-packet, price-evidence and HTF-evidence writers, with `persist: false`. Because `pair-discovery.ts` emits a row for every pair in its locations map regardless of eligibility, the stale pair's absence shows pipeline exclusion, not merely non-selection, so a separate eligibility control is not needed.

**Not covered:** the weekly pipeline (same guard pattern, `weekly.ts` lines 69-75 and 107); stale 4H exclusion at pipeline level; non-stale INVALID evidence status; persistence and evidence-writer behaviour for a stale pair (stubbed).

**Status:** daily 1D path covered. The owner decides whether the remaining gaps are accepted as recorded limits or required before approval.

## 6. Open items before this amendment is final

1. **Owner approval** (Section 9, Approval record).
2. **[BLOCKER B1 — recorded in 5b; remaining: observe NZDCHF's ABC lifecycle directly]** Sample run of pair discovery on identical frozen inputs, before and after the change. Record actual SELECT / WATCH / PASS counts for both, and the per-pair differences. `htfEligible` is intentionally broader than the old QMR-derived rule; an unexplained collapse to all-SELECT is a failure to investigate, not an expected result. If no pre-change baseline can be reproduced, record that explicitly and treat the post-change counts as unbaselined.
3. **[BLOCKER B2] I4 coverage** at the freshness layer. Located: `lib/integrity/freshness.ts` (`assertFreshDaily`, limit 48 market hours; `assertFresh4H`, limit 24). In `lib/pipeline/daily.ts` (lines 96-101) and `weekly.ts` (69-75) the assert runs inside a try/catch before the series is added to the cache and the OHLC map, so a stale pair is recorded as an error and excluded per pair; the 4H catch body was not seen. `freshness-order.test.ts` covers candle ordering and the fresh-passes case; `freshness.test.ts` and `freshness-4h.test.ts` (not read when v0.3 was written) already covered stale daily/4H data per Copilot's reading. Required to close B2:
   - (a) Unit tests: stale daily throws past 48 market hours; stale 4H throws past 24; boundary case; weekend closure not counted; unreadable timestamp throws.
   - (b) A pipeline-level test with injected market data returning one stale and several fresh series: assert the stale pair produces an error entry and does not appear in the pair-discovery matrix or HTF outputs, and the fresh pairs are unaffected. (a) alone proves the guard but not that the pipeline uses it to exclude the pair. If (b) cannot be driven offline, record the smallest seam needed instead of omitting it.
4. **Dead code check:** `htf-pair-builder.ts` may still compute `eligibleDirections` that nothing consumes.
5. **Diff review of all changed `lib/` files**, kept separate from unrelated working-tree changes. Priority: `lib/pipeline/daily.ts` and `weekly.ts` (4 lines each; they hold the freshness asserts), `yahoo-historical.ts` and `structural-break.ts`. Also: 31 modified tracked files were observed against 30 earlier; the extra file is unidentified.
5a. **R:R question** for READY setups (see 5b): confirm whether a minimum R:R is intended and where it would be enforced.
6. **Reconcile the frozen-methodology source text.** The brief referenced in code comments ("QMR/HTF Master Implementation Brief", Sections 12 and 20) was not reviewed for this draft. Confirm it is updated or marked superseded for these points.

## 7. Implementation and commit rules

- Record owner approval (Section 9) **before** the docs commit. Commit this amendment to the docs repo next, then commit the code.
- Commit hashes do not exist at approval time. They are captured afterward in the post-commit audit record (Section 9), not by editing the approval.
- Stage code by explicit file path. Never `git add .`. Unrelated changes (`app/admin/page.tsx`, `app/api/reports/slug`, `app/api/research-cycles`, `.gitignore`, `tsconfig.json`, `lib/market-data/adapters/yahoo-historical.ts`) are not part of this change.
- No secrets, keys or `.env` content in either repository.
- Code commit message should cite "v7 Amendment 001".

## 8. Rollback

Revert the code commit. QMR code paths remain in the repository (computed, not deleted), so restoring gating means re-adding the removed `trade-construction.ts` guard and the QMR-derived eligibility input to pair discovery. The 18-combination invariance tests must be removed or inverted at the same time.

## 9. Approval and audit records

### 9a. Approval record (completed BEFORE the docs commit)

| Field | Entry |
|---|---|
| Decision | ☐ Approved ☐ Rejected ☐ Approved with changes |
| Amendment version approved | v0.2 (or later: ____) |
| Approved by | ______________________ |
| Date | ______________________ |
| Blocker B1 (sample check) result recorded | ☐ Yes — counts: ______ |
| Blocker B2 (I4 coverage) test named | ☐ Yes — test: ______ |
| Conditions / changes | ______________________ |

### 9b. Post-commit audit record (completed AFTER the commits; a recorded follow-up, not a rewrite of 9a)

| Field | Entry |
|---|---|
| Docs commit hash | ______________________ |
| Code commit hash | ______________________ |
| Date committed | ______________________ |
| Files staged (explicit paths) | ______________________ |
| Post-commit `vitest` / `tsc` / `build` | ______________________ |

---

*End of amendment.*
