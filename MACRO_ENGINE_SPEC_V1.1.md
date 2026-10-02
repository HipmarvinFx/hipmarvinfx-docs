# HipMarvinFX Macro Engine Specification v1.1 (AMENDMENT — DRAFT)

**Status:** DRAFT for approval. Amends `MACRO_ENGINE_SPEC_V1.md`; does not replace it.
**Implementation status:** Contract only. First slice = COT Positioning.
**Date:** 2 October 2026
**Branch for implementation:** `v7-macro-engine`

Everything in v1 not mentioned here is unchanged. Items marked **[PROPOSED]**
are new methodology/threshold choices that v1 §6 and §20 require to be
explicitly approved before production activation. They are not approved by
the existence of this draft.

---

## A. Rulings

| # | Ruling | Basis |
|---|---|---|
| A1 | Macro output **ranks** pair candidates. It never excludes or gates one. | v1 §13, §14 |
| A2 | `lib/engine/currency-strength.ts` is a **technical, price-derived** input (20D OHLC). It is **not** the v1 §7 macro Currency Relative Strength, which remains unbuilt. The two must stay separately labeled and must never be summed in a score. | Handover 25 Aug §7; v1 §3, §7 |
| A3 | Manual-origin evidence is eligible for engine calculation **[PROPOSED — provisional default, owner to confirm]**, but every output derived from it MUST carry `origin: "manual"`, and it must never be presented as auto-verified. | Developer Handover §19 |
| A4 | Legacy ledger fields `CentralBankEvidence.policyDirection` / `currencyImplication` and the string-typed `MacroRegimeEvidence` fields are **deprecated** and not to be populated by any engine, AI, or adapter. Replacement is the engine outputs defined here and in v1 §5. | Existing rejection of text-derived policy direction |

## B. Output completeness (closes the "status without analysis" gap)

Upstream evidence status (`VERIFIED`, etc.) describes whether an input is
well-formed and fresh. It does **not** mean the macro state is known.

Every engine output MUST carry:

```text
completeness: COMPLETE | DEGRADED | INSUFFICIENT_DATA
reason: string            (required unless COMPLETE)
engine_version
rule_set_version
input_evidence_ids[]
calculated_at
origin: "automated" | "manual" | "mixed"
```

Consequences:

1. `ledger.macroRegime.status` MUST NOT be set to `VERIFIED` unless the engine
   output behind it is `COMPLETE`. Today `daily.ts` copies the macro
   *assembly* status into `macroRegime.status` while `currencies` is `[]`;
   `validateLedger` then treats the field as present. This MUST be corrected.
2. `ledger.macroRegime.ranking` currently holds the price-derived strength
   ranking. It MUST be relabeled as price-derived (A2) or removed from
   `macroRegime`.

## C. First slice — COT Positioning (spec §11)

### C1. Inputs
- Evidence `type: "cot"`, classification FACT, eligible status `VERIFIED`.
- The engine reads the raw `CotPositioning[]` arrays. It MUST NOT consume
  `lib/evidence/cot.ts`'s `COTEvidence` projection (it drops pair direction,
  and sets WoW to `null`).

### C2. Selection (does not trust query order)
`queryEvidence` orders by `created_at`, and the store holds duplicate rows
for the same report. The engine MUST:
1. Flatten all eligible rows' arrays.
2. Deduplicate on `(pair, reportDate)`.
3. For each pair, select the row with the maximum `reportDate`.
4. Record the evidence IDs used.

### C3. Report-date checks
- **Freshness [PROPOSED, rule_set `cot-rules/1`]:** age = calculation date −
  `reportDate` in calendar days. Normal maximum is 10 days (Tuesday positions,
  read on the Friday before the next release). Age > 10 → `STALE` →
  that pair is `INSUFFICIENT_DATA`, reason `report_stale`. Freshness is
  measured from `reportDate`, never from `retrievedAt`.
- **Weekday sanity:** CFTC TFF positions are as of Tuesday. A `reportDate`
  that is not a Tuesday → `DEGRADED`, reason `report_date_not_tuesday`, until
  the discrepancy is explained against the CFTC publication.

### C4. Sign convention
Pair direction, exactly as the adapters store it. Where `inverted: true`
(USD/JPY, USD/CAD, USD/CHF), `net` is already positioning **in the pair's
direction** (positive = long the pair). Output is per **pair**, with
`inverted` carried through. No per-currency COT value is emitted.

### C5. Flow
```text
leveragedFundsFlow = weekChangeLong − weekChangeShort   (null if either is null)
assetManagersFlow  = weekChangeLong − weekChangeShort   (null if either is null)
```
A null flow makes any state requiring flow `INSUFFICIENT_DATA`.

### C6. Positioning state (per pair, per idea direction D ∈ {LONG, SHORT})
Definitions are taken from `PUBLISHING_PIPELINE.md` (Layer 1), so no new
thresholds are introduced except where marked.

- **ACTIVELY_OPPOSES(D):** both Leveraged Funds and Asset Managers have net
  positioning against D **and** both flows moved further against D.
  A net or flow of exactly zero does not count as "against".
- **SUPPORTS(D):** both Leveraged Funds and Asset Managers have net
  positioning with D **and** both flows moved further with D.
  A net or flow of exactly zero does not count as "with".
  No dominant-category rule: per PUBLISHING_PIPELINE.md v1.8, size-based
  dominance would make Asset Managers dominant by construction.
- **MIXED(D):** any other case with complete inputs, including the two
  categories disagreeing. (Disagreement is never force-classified as opposed.)
- **INSUFFICIENT_DATA:** missing/stale/degraded input.

### C7. Coverage limits
- USD has no direct COT series (it is the other side of every pair) →
  `INSUFFICIENT_DATA`, reason `no_direct_series`. Not inferred from the pairs.
- XAU/XAG are not covered → `INSUFFICIENT_DATA`.
- CHF, JPY, CAD, EUR, GBP, AUD, NZD are covered via their pair rows.

## D. Blocked outputs and required source upgrades

| v1 output | v1.1 state | Blocker (verified against code/data, 2 Oct 2026) |
|---|---|---|
| COT Positioning (§11) | Buildable now | — |
| Macro Surprise (§9) | `INSUFFICIENT_DATA` | FinanceCalendar: 0 of 39 sampled events carry `actual`; 3 carry `consensus`; 0 carry `currency` |
| Event Impact (§12) | `INSUFFICIENT_DATA` | `currency` null for all events; feed includes non-macro items |
| Policy Direction (§10) | `INSUFFICIENT_DATA` | Central-bank evidence holds titles/dates only — no rate, decision, or vote fields. Deriving direction from titles remains rejected |
| Macro Regime, Driver Ranking, macro-derived Relative Strength, Pair Candidate Ranking | `INSUFFICIENT_DATA` | Depend on the above plus approved thresholds |

Required source upgrades, in order:
1. **Manual calendar actuals** (type `calendar-actual`, section E) as the v1
   mechanism, sourced from the ForexFactory screenshot per Standing Protocol
   Rule 2. A replacement automated provider is a later, separate decision.
2. **Structured central-bank fields** (policy rate, decision, vote split) from
   official statement pages, as a new adapter capability.

## E. Manual evidence hardening (`/api/evidence/ingest-manual`)

The route exists but has no callers and is unconstrained. Before it carries
engine inputs:

1. **Type allowlist.** Only listed types are accepted. Manual `price` and
   `htf` are NOT allowed.
2. **Per-type value schema**, rejected with 422 when violated.
3. **Separate type for manual calendar data:** `calendar-actual`. It must not
   share `type: "calendar"` with the automated feed, because
   `assembleMacroEvidence` takes the newest `calendar` row and casts it to
   `FinanceCalendarEvent[]`.
4. `calendar-actual` schema (all required unless noted):
   `event`, `currency` (ISO 4217, from a fixed allowlist), `unit`,
   `actual` (number), `forecast` (number, may be null), `previous` (number,
   may be null), `releasedAt` (ISO timestamp), `sourceNote`
   (e.g. "ForexFactory screenshot, captured <time>").
5. Provenance keeps `source: "<type>-manual"` and `metadata.manualEntry: true`.
   The engine maps these to `origin: "manual"`.

## F. Pre-implementation fixes (found 2 Oct 2026)

| # | Finding | Required action |
|---|---|---|
| F1 | Two validators exist. `source-registry.validateEvidenceItem` (provenance and timestamps only) is used by `calendar.ts`, `cot.ts`, `central-bank.ts`. The generalized `validator.ts` (type rules) is used only by the ingest routes. | Decide the canonical validator and apply type rules on every write path. |
| F2 | `COT_RULES` expects `value.netPosition` (a scalar) but COT evidence is a `CotPositioning[]` array; its freshness check uses `retrievedAt`. | Rewrite COT rules to the real shape and to `reportDate`. |
| F3 | `cot.ts` stores a new row id every run; duplicates accumulate. | Reuse the row per `(type, source)` or per `reportDate`. |
| F4 | `mockPriceAdapter` was registered unconditionally at priority 999 and could emit a `VERIFIED` EURUSD price on a double outage. | **Done:** unregistered in `adapters/index.ts` (evidence tests: 106 pass). Optionally guard `registerSourceAdapter` against `mock*` names outside tests. |
| F5 | `macroRegime.ranking` carries price-derived strength. | See B.2. |
| F6 | Evidence rows dated 2026-09-21 (a Monday) all come from `cftc-cot-html-fallback`. The Socrata adapter (`cftc-cot`) stored the same report as 2026-09-22 (Tuesday), which is the correct as-of date. The fallback's `reportDate` is one day early. Cause confirmed: `parseReportDate` used `new Date("September 22, 2026")` (local midnight) then `toISOString()`, shifting the date a day early in timezones ahead of UTC. Fixed in app-repo commit `3a69bda` (timezone-safe UTC parse + test); the six bad rows were marked INVALID on 2 Oct 2026. | Read `parseReportDate` in `cftc-cot-html-fallback.ts`, fix with a timezone-safe parse, add a test, and mark the six existing fallback rows INVALID (they were created during 27 Sep testing and the adapter was never live-tested). The C3 weekday check is what exposed this. |
| F7 | `loadLatestCotEvidence` takes `limit: 1` ordered by `created_at`, so it currently returns the newest fallback row (dated Monday, wrong) instead of the Tuesday-dated Socrata row. The daily ledger's COT section therefore carries the wrong `reportDate`. | Superseded by C2 for the engine; fix or retire the projection in `cot.ts`. |

## G. Required tests

- Same inputs and rule version produce identical output (determinism).
- Duplicate rows, and rows returned in any order, give the same result.
- Max `reportDate` wins per pair.
- `reportDate` older than 10 days → `INSUFFICIENT_DATA` / `report_stale`.
- Non-Tuesday `reportDate` → `DEGRADED`.
- Inverted pair (USD/CAD) sign follows pair direction.
- Missing WoW → states needing flow are `INSUFFICIENT_DATA`.
- USD and XAU → `INSUFFICIENT_DATA`.
- Disagreeing LF/AM → `MIXED`, never `ACTIVELY_OPPOSES`.
- Manual-origin input → output `origin: "manual"`.
- Non-`VERIFIED` input is never used.
- Engine output never contains entry, stop, target, or any exclusion of a pair (A1, v1 §14).

## H. Non-goals

No change to HTF, QMR, ABC, trade construction, or pair-discovery scoring.
No macro points added to technical scores. No AI involvement in any
calculation. No provider selection.

## I. Approval checklist

- [ ] A3 manual-origin eligibility confirmed
- [ ] C3 freshness threshold (10 days) approved
- [ ] C6 COT SUPPORTS rule (both categories agree, per Pipeline v1.8) approved
- [ ] F1 canonical validator chosen
- [ ] F6 fallback `reportDate` bug confirmed in `parseReportDate` and fixed; bad rows marked INVALID
- [ ] F7 daily ledger COT projection fixed or retired
