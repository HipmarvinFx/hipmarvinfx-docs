# HipMarvinFX v7 - Methodology Amendment 001
## ABC Primary Technical Setup; QMR Diagnostic-Only

**Status:** DRAFT v0.8 - NOT APPROVED. Not in force until Section 9a is completed and this file is committed to `HipmarvinFx/hipmarvinfx-docs`.

**Release blockers:**
- B2 - I4 coverage: daily 1D path covered; owner must accept or reject the remaining gaps (5c).
- B3 - Spec/brief reconciliation (PROPOSED by reviewer, owner to confirm): the amendment removes the QMR requirements in `QMR_ABC_TECHNICAL_SPEC.md` sec. 2 and 18 and in the frozen brief (sec. 9, 12, lines 261 and 531), and does not yet identify them or satisfy spec sec. 24 change control.
- B4 - READY exposure (PROPOSED by reviewer, owner to confirm): AI prompts do not carry `technicalPermission`, so a construction READY with a WAIT permission (NZDCHF) is presented to the AI without the permission verdict (6.2).
- B1 - CLOSED (5b).

**v0.8 changes:**
- All non-test diffs are now read. All 17 removed/renamed test titles have replacements (5d).
- Two corrections to earlier drafts: (a) `permission.ts` changed beyond the QMR gate - it gained two READY guards (reaction confirmed, validated armed levels) that tighten EXECUTABLE; the 5b tally proves less than v0.7 implied (5b). (b) v0.7 and the v0.8 review notes said the amendment promotes the legacy ABC to the primary gate; the evidence shows the permission and trade-construction path reads the v2-style lifecycle (TARGET_ACTIVE, REACTION_CONFIRMED). Which ABC module feeds the gate is not yet confirmed (6.3).
- `historical-backtest.ts` and packet readiness added to the change table; `app/review/weekly/[cycle_id]/page.tsx` added to the unrelated list.
- I2 and I3 closed on scans; code proof that NOT-ELIGIBLE never carries a direction.
- Proposed explicit staging list (Section 7).

**History:** v0.2 reworded I1, promoted I4 and the sample check to blockers, separated approval from commit hashes. v0.3-v0.4 recorded B1 and B2 work. v0.5-v0.7 closed the NZDCHF ABC gap, resolved the R:R question, recorded the permission tally and `AGENTS.md`.

**Date drafted:** 22 October 2026
**Amends:** v7 methodology locked in `V7_EVIDENCE_ARCHITECTURE_AND_IMPLEMENTATION_DIRECTIVE.md` (frozen items: "QMR sequencing", "QM/QML refinement")
**Authority:** v7 Developer Handover (28 Aug 2026): frozen methodology changes need a separate, explicitly approved decision; contracts are committed before implementation.

---

## 1. Decision proposed

1. ABC is the primary technical setup.
2. QMR (phase and eligibility) is diagnostic only. It may be computed, stored, displayed and passed to the AI as context. It must not gate setup eligibility, permission, trade construction, pair discovery selection, or any score.
3. QM/QML, FVG, order blocks and key levels are supporting or fallback evidence, used only when explicitly present in the evidence packet. They never override a retained gate.
4. Fundamentals remain the directional priority.

## 2. What changes

| Area | Before | After |
|---|---|---|
| Trade construction | Invalid unless `qmrEligible` and a confirmed QMR phase; direction from `qmr.eligibleDirection` | No QMR condition; direction from `trendAlignment.preferredDirection`. Still needs a deterministic direction and ABC reaction confirmation (`PARTIAL` otherwise) |
| Pair discovery eligibility | QMR-derived `eligibleDirections` | `htfEligible`: alignment present, not NOT-ELIGIBLE, LONG or SHORT preferred direction |
| Pair discovery output | `[eligible]` / `[REJECTED]` from QMR | `QMR diagnostic <phase>` text only |
| ABC (v1 classifier) | NOT-APPLICABLE unless QMR phase confirmed | NOT-APPLICABLE only when HTF gives no direction; signature drops the `qmr` argument |
| QM/QML classifier | NOT-APPLICABLE unless QMR confirmed | HTF direction is the only gate; QMR values appended as diagnostic evidence |
| AI synthesis readiness (packet) | `qmrEligibility` in missing fields; QMR not eligible forced WAIT/NO-TRADE prompt | QMR not a readiness input; readiness = trend not CONFLICT, alignment not NOT-ELIGIBLE, nullable gaps only |
| Permission | QMR-eligible required for ALIGNED; READY went straight to EXECUTABLE | No QMR condition. **Additional tightening:** READY -> EXECUTABLE now also requires ABC reaction confirmed AND validated armed levels (R:R to TP2 >= 1.0, risk <= 2%); otherwise WAIT |
| Historical backtest | Direction from QMR | Direction from `preferredDirection` (backtests before and after are not comparable) |
| AI prompts and adapters | QMR part of the setup chain | ABC primary; QMR labelled diagnostic; AI told not to use QMR as a gate |

**Beyond QMR removal:** the permission READY guards are a tightening that is not about QMR. The packet-readiness change loosens AI synthesis for pairs that previously failed on QMR. Both need explicit owner acceptance.

## 3. What does NOT change (retained gates)

- HTF-first analysis; Daily / 4H / 1H minimum framework; trend-first doctrine
- Independent HTF trend alignment requirement
- Structural-break countertrend gate (COUNTERTREND requires a confirmed Daily break against the Daily trend with 4H agreeing; `trend-alignment.ts` lines 129-136 unchanged)
- ABC reaction-confirmation requirement
- 20D location, liquidity and flow regime
- Premium is not an automatic short; Discount is not an automatic long
- Minimum 1H execution timeframe (no sub-1H)
- Evidence statuses, provenance, freshness, and the v7 parser firewall
- Standing Protocol Rules 1-4

**Explicit prohibition:** stale or unverified data must not be made eligible to increase setup availability. Any change to freshness or validity gates requires its own amendment.

## 4. Invariants (testable)

- **I1.** For identical non-QMR inputs, eligibility, permission and trade-construction decisions are identical across all QMR phases (including undefined) and all `qmrEligible` values. Diagnostic echoes of QMR may differ.
- **I2.** No QMR value appears in an `if`, filter, sort, or score term that decides eligibility, in `lib/engine/` or `lib/ai/`. (Scanned 22 Oct: only pass-through, display, evidence strings and prompt text. Same-line scan only.)
- **I3.** AI prompt text must not describe QMR as a gate or prerequisite. (Unfiltered QMR line listing of `lib/ai` read 22 Oct: all say diagnostic or are pass-through.)
- **I4 (RELEASE BLOCKER, daily path covered).** ABC and HTF alignment passing with stale or invalid evidence must still be blocked, enforced where freshness lives. See 5c.
- **I5 (proposed).** Construction READY is not an execution permit. EXECUTABLE requires construction READY, ABC reaction confirmed, and validated armed levels. Tests: "does not grant EXECUTABLE when READY lacks validated levels", "...when ABC reaction is unconfirmed" (`permission.test.ts`).

## 5. Verification status

| Check | Result | Basis |
|---|---|---|
| `npx vitest run` | 125 files, 868 tests passed | Run output seen / Copilot report (5c) |
| `npx tsc --noEmit`, `npm run build` | exit 0 | Run output seen |
| Tracked-file hash guard | PASS, 438 files identical, repeated after every review run | Run output seen |
| QMR gating audit | No QMR value reaches a gating condition | Copilot report plus I2/I3 scans |
| NOT-ELIGIBLE never carries a direction | Code: `trend-alignment.ts` sets `preferredDirection` only in WITH-TREND and COUNTERTREND branches (lines 99-143); both NOT-ELIGIBLE branches (110, 147) leave it null. Snapshot: 0 leaks | Code read and script output |
| Freshness gates | `daily.ts` / `weekly.ts` asserts untouched; only the unused `eligibleDirections` argument removed | Diffs read |
| Packet readiness inputs | Trend CONFLICT, NOT-ELIGIBLE alignment, nullable gaps. No freshness input; freshness is enforced upstream by exclusion at pipeline level | Code read (`htf-evidence-packet.ts` 125-149) |

### 5b. Blocker B1 - sample check (CLOSED)

Frozen Yahoo snapshot (2026-10-22T09:22Z, 30 pairs) run through committed HEAD and the working tree.

| | HEAD | Working tree |
|---|---|---|
| Pair discovery | SELECT 6, WATCH 1, PASS 23 | identical |
| Trend alignment | WITH-TREND 6, COUNTERTREND 1, NOT-ELIGIBLE 23 | identical |
| Trade construction | INELIGIBLE 30 | READY 1 (NZDCHF), PARTIAL 4 (USDCAD, EURGBP, EURAUD, NZDUSD), INELIGIBLE 25 |
| Permission | WAIT 30, armed 0 | WAIT 30, armed 0 |

**Per-pair causes of the 25 INELIGIBLE (observed):** 23 are NOT-ELIGIBLE alignment. GBPJPY and CADJPY are WITH-TREND LONG with ABC reaction present but "structural TP2 does not lie beyond entry in the required profit direction" (geometry, not QMR).

**Correction to v0.7:** the permission tally does not show the refactor is harmless at the permission layer. At HEAD no pair reached READY, so the old READY -> EXECUTABLE path was never exercised on this snapshot. In the working tree, NZDCHF (READY, R:R 0.37, entry 0.46670, stop 0.46203, TP2 0.46845, TP1 NOT_AVAILABLE) is held at WAIT only because of the new READY guards in `permission.ts` (evidence: "validated armed levels are unavailable; risk and entry checks have not passed"). Without those guards it would have been EXECUTABLE. The R:R floor (`MIN_RR_TO_TP2` 1.0, `MAX_RISK_PCT` 2) existed at HEAD in `buildArmedSetup`; the diff is the guard that applies it to READY. The specific rejection reason for NZDCHF is not printed (the READY branch drops it); R:R 0.37 < 1.0 is the consistent explanation (risk is about 1.0% of entry, under the 2% cap).

**Other observations:**
- NZDUSD is the COUNTERTREND pair (Daily BEARISH, 4H BULLISH, confirmed bullish 4H break, direction LONG). ABC reaction not present, so PARTIAL; permission WAIT.
- NZDCHF was VERIFIED on both series. EURAUD (a SELECT pair) had a STALE daily series in the harness, which bypasses the pipeline freshness assert.
- Flow, liquidity and location maps were empty; the snapshot cannot exercise the arming path. Arming is covered by unit tests only.
- Single snapshot, single market state.

### 5c. Blocker B2 - I4 coverage

- `lib/integrity/freshness-stale.test.ts` (new, untracked): exact limits (48h daily, 24h 4H), weekend exclusion, unreadable timestamps.
- `lib/pipeline/daily-freshness.test.ts` (new, untracked): offline `runDailyPipeline` with one stale pair (EURUSD, 10 days old) and five fresh pairs; stale pair gets "Stale daily" error and is absent from discovery rows and HTF packets.
- Full suite 125 files, 868 tests (was 123 / 863).
- **Not covered:** weekly pipeline (`weekly.ts` 69-75, 107); stale 4H exclusion at pipeline level; non-stale INVALID evidence status; persistence behaviour for a stale pair (stubbed).
- **Required before approval:** add deterministic tests for the weekly pipeline freshness gate; stale 4H exclusion at pipeline level; INVALID evidence rejection even when ABC and HTF alignment pass; and persistence behaviour for stale pairs (replace the current stub-only check with an assertion that verifies the intended outcome).
- **Acceptance evidence:** each test must fail when its corresponding gate is bypassed and pass with the gate enforced. Record exact test names and results in this section.
- **Owner decision:** B2 remains OPEN until all four coverage gaps above are tested and results are recorded. Do not accept the gaps as release-ready by default.

### 5d. Test-change audit (existing tests were modified)

Every removed test title maps to a replacement tied to this amendment:

| Removed assertion | Replacement |
|---|---|
| abc: NOT-APPLICABLE when QMR phase not confirmed | NOT-APPLICABLE when HTF alignment provides no direction |
| abc: PRESENT LONG/SHORT when legs confirmed (QMR fixture) | Completed LONG/SHORT ABC classified without requiring QMR |
| qm 1-4: QUALITY / MANIPULATION / REACTION / null direction return NOT-APPLICABLE | Same cases now assert QMR phase does not block HTF-directed classification |
| qm 5-9: "confirmed + ..." | Same scenarios without the QMR precondition |
| qm 10: evidence starts with QMR entries | Evidence labels QMR fields as diagnostic |
| packet: not ready when QMR not eligible | QMR phase/eligibility not a readiness gate (asserts ready true, direction LONG) |
| trade-construction: rejects unconfirmed QMR setup | Same construction for every QMR phase/eligibility; no construction before ABC reaction |
| permission: grants EXECUTABLE only when engine built levels | Same result for every QMR value; EXECUTABLE with validated levels; WAIT when levels missing; WAIT when ABC unconfirmed; risk-check failures not executable |
| permission: arms aligned setup waiting for ABC (with QMR wait) | Arms without a QMR wait gate |
| adapters/index.test: QMR fixture fields | `qmrEligible:false`, phase QUALITY; asserts prompt shows "ABC setup (primary)" and "QMR diagnostic phase QUALITY (eligibility NO)" |
| best-expression-prompt.test: "QMR confirmed" line | "Trade direction: LONG." |

Spec sec. 21 forbids editing tests merely to make an implementation pass. These edits follow this amendment's methodology change; approval of the amendment is what authorises them.

## 6. Open items

1. **Owner approval** (9a).
2. **B2 decision** (5c).
3. **B3 - spec and brief reconciliation (PROPOSED).**
   - `QMR_ABC_TECHNICAL_SPEC.md` sec. 2 and 18 require QMR confirmed continuation (continuation) and confirmed reversal (reversal). The amendment removes both requirements.
   - Frozen brief (two identical copies, SHA256 B3B81BD3...47FC4C0, last commit 189d194) still states: sec. 9 "QMR continuation is the primary use"; line 261 "QMR then confirms the new direction"; sec. 12 flow "VALID QMR -> REACTION -> ..."; line 531 acceptance case "bearish QMR -> countertrend short eligible".
   - Spec sec. 24 change control requires: affected rule, reason, supporting evidence, deterministic tests, implementation impact, backtest impact.
   - Owner chooses: (a) amend spec sec. 2/18 under sec. 24, or (b) state that this amendment supersedes those passages and mark the brief passages superseded. Either way the backtest-impact statement is needed (`historical-backtest.ts` changed direction source).
   - **Unconfirmed:** which ABC implementation feeds trade construction and permission. Permission reads lifecycle values (REACTION_CONFIRMED, TARGET_ACTIVE) that match the spec lifecycle, and the packet takes an `AbcFormationV2Analysis`, which suggests the gate uses v2, not the legacy classifier named in spec sec. 23. To be confirmed by reading `htf-pair-builder.ts` and `trade-construction.ts` imports.
4. **B4 / 5.2 - READY semantics (PROPOSED).**
   - Verified: no `technicalPermission` reference in `lib/ai`. The packet prompt reports ABC lifecycle and trade-construction status, so the AI can be told READY without being told permission is WAIT. At HEAD READY was unreachable, so this is new exposure.
   - Prompts' "READY / NOT READY" wording refers to packet readiness, a different concept (clean).
   - `translateEngine.ts` wording ("close to being ready") is non-committal; the producer of its `state` value is not yet found.
   - Admin integrity page shows trade status without permission (admin-only, low risk).
   - Options: pass permission into the packet prompt, or add a prompt rule that construction READY is not actionable, or accept as a recorded limit.
5. **Follow-ups (non-blocking):** `eligibleDirections` in `htf-pair-builder.ts` is now a duplicate of `trendAlignments` preferred direction and is read only by one test; `permission.ts` header still says "Additive: no existing gate is changed" and is now inaccurate; the READY branch drops the armed-levels rejection reason.
6. **`AGENTS.md`:** appends "General AI Agent Operating Rules" (no commit/push/deploy without authorization, etc.); no conflict with this amendment; provenance unknown (last committed 44f1dd3, 18 Oct). Keep-or-revert is a separate owner decision. Excluded from the code commit.
7. **Weekly 4H/INVALID coverage** - see 5c.

## 7. Implementation and commit rules

- Record approval (9a) before the docs commit. Commit this amendment to the docs repo, then the code.
- Stage by explicit path. Never `git add .`.
- **Proposed code commit (23 tracked + 2 untracked, to be confirmed with `git status`):**
  - `lib/ai/`: `adapters/index.ts`, `adapters/index.test.ts`, `compact-router.ts`, `htf-prompt.ts`
  - `lib/engine/`: `abc-formation.ts`, `abc-formation.test.ts`, `best-expression-prompt.test.ts`, `historical-backtest.ts`, `htf-evidence-packet.ts`, `htf-evidence-packet.test.ts`, `htf-pair-builder.ts`, `pair-discovery.ts`, `permission.ts`, `permission.test.ts`, `qm.ts`, `qm.test.ts`, `structural-break.ts`, `trade-construction.ts`, `trade-construction.test.ts`, `trend-alignment.ts`, `trend-alignment.test.ts`
  - `lib/pipeline/`: `daily.ts`, `weekly.ts`
  - Untracked tests: `lib/integrity/freshness-stale.test.ts`, `lib/pipeline/daily-freshness.test.ts`
- **Excluded (unrelated):** `.gitignore`, `AGENTS.md`, `tsconfig.json`, `app/admin/page.tsx`, `app/api/reports/slug/[slug]/route.ts`, `app/api/research-cycles/route.ts`, `app/review/weekly/[cycle_id]/page.tsx`, `lib/market-data/adapters/yahoo-historical.ts`.
- `yahoo-historical.ts` and `.gitignore` have mixed line endings; `yahoo-historical.ts` also gained a BOM (probably a PowerShell write). Not part of this change; its 30-day chunking has no dedupe at chunk edges, which matters for backtests.
- `pair-discovery.ts` changed but no test file for it appears in the diff. Confirm whether `htfEligible` has a test (possibly untracked) before commit.
- No secrets or `.env` content. Commit message cites "v7 Amendment 001".

## 8. Rollback

Revert the code commit. QMR code paths remain (computed, not deleted). Reverting also removes the permission READY guards and the packet readiness change, so decide whether those two should be kept independently if QMR gating is restored. The invariance tests must be removed or inverted at the same time.

## 9. Approval and audit records

### 9a. Approval record (before the docs commit)

| Field | Entry |
|---|---|
| Decision | [ ] Approved  [ ] Rejected  [ ] Approved with changes |
| Amendment version approved | v0.8 (or later: ____) |
| Approved by | ______________________ |
| Date | ______________________ |
| B1 sample check recorded | [x] Yes - SELECT 6 / WATCH 1 / PASS 23 both sides; construction INELIGIBLE 30 -> READY 1 / PARTIAL 4 / INELIGIBLE 25; permission WAIT 30, armed 0 |
| B2 I4 coverage named | [x] `daily-freshness.test.ts`, `freshness-stale.test.ts`; gaps accepted: [ ] Yes [ ] No |
| B3 resolution | [ ] Spec amended under sec. 24  [ ] Amendment supersedes; brief marked  [ ] Not a blocker |
| B4 resolution | [ ] Permission in AI prompt  [ ] Prompt rule  [ ] Accepted limit |
| Permission READY guards accepted (2) | [ ] Yes [ ] No |
| Conditions / changes | ______________________ |

### 9b. Post-commit audit record (after the commits)

| Field | Entry |
|---|---|
| Docs commit hash | ______________________ |
| Code commit hash | ______________________ |
| Date committed | ______________________ |
| Files staged (explicit paths) | ______________________ |
| Post-commit `vitest` / `tsc` / `build` | ______________________ |

---



## B3 — Proposed supersession and change control (NOT APPROVED)

**Affected rules:** `QMR_ABC_TECHNICAL_SPEC.md` Sections 2 and 18; `V7_QMR_HTF_MASTER_IMPLEMENTATION_BRIEF.md` Section 9, Section 12 flow, line 261, and acceptance case at line 531.

**Reason:** The current documents make confirmed QMR continuation/reversal part of eligibility, conflicting with this amendment's proposed ABC-primary, QMR-diagnostic-only methodology.

**Supporting evidence:** The frozen brief states “QMR CONTINUATION IS THE PRIMARY USE”, requires “QMR then confirms the new direction”, places “VALID QMR” in the setup flow, and lists bearish QMR as part of countertrend-short eligibility. These passages conflict with Sections 1–3 of this amendment.

**Deterministic tests:** I1–I3; pair-discovery baseline B1; freshness/validity blocker I4/B2; construction and permission tests; prompt tests ensuring QMR is not described as an eligibility gate. Test results must be recorded before approval.

**Implementation impact:** Supersession is proposed only. The technical specification and frozen brief remain unchanged until separately reviewed and approved. The actual ABC implementation feeding trade construction and permission must be verified before implementation is accepted.

**Backtest impact:** `lib/engine/historical-backtest.ts` changes direction sourcing from QMR eligibility to `trendAlignment.preferredDirection`. Pre-change and post-change backtest results are therefore not directly comparable without a frozen-input baseline and explicit methodology annotation.

**Proposed resolution:** On approval, this amendment supersedes only the conflicting QMR eligibility requirements identified above. The HTF structural-break gate for countertrend setups, ABC reaction confirmation, freshness/validity gates, and all other retained gates remain in force. The frozen brief must receive a corresponding supersession notice through a separately reviewed documentation change.

**Status:** DRAFT / NOT APPROVED. This record does not authorize code changes, staging, commits, or publication.

---

*End of amendment.*
