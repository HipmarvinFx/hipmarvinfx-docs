# HipMarvinFX — Phase 11 Scope Finding: Publication Layer Trace

**Date:** 2026-09-20
**Type:** Read-only investigation finding, no implementation performed
**Related commit:** `516f118` — "fix: preserve null tier as Not Rated" (unrelated bug fix, committed separately during this investigation; see Appendix)

---

## Purpose

The V7 QMR/HTF Master Implementation Brief names "Phase 11 — Website Contract" in its phase sequence but contains no dedicated requirements section for it (confirmed: the exact string "PHASE 11" appears only once in `V7_EVIDENCE_ARCHITECTURE_AND_IMPLEMENTATION_DIRECTIVE.md`, in the phase-flow diagram; "Website Contract" as a phrase appears only there and in the document's generic opening description). Before writing any Phase 11 code, this investigation traced the actual, current production code to establish what already exists, what is wired, and what is genuinely missing — so Phase 11 work extends real infrastructure rather than duplicating it or inventing a contract that doesn't need to exist.

Everything below is derived directly from command output against the live repository, not from documentation, comments, or inference. Where a comment in the code made a claim, that claim was independently verified against the actual code path before being trusted.

---

## Finding 1 — Weekly Outlook: pre-v7 publication gap, fully traced end-to-end

**Chain traced:**

```
Supabase trade_ideas table
  (has tier, correlation_class, zone_position_pct columns)
        ↓
GET /api/trade-ideas/route.ts
  .select('id, cycle_id, pair, direction, bias, entry, stop_loss,
           target_1, target_2, conviction_score, status, atr, rank,
           stars, analysis, risk_reward, created_at')
  — EXCLUDES tier, correlation_class, zone_position_pct
        ↓
app/outlook/weekly/[cycle_id]/page.tsx
  safeFetch('/api/trade-ideas?cycle_id=...') → stored as raw Row[]
  Zero references to: rating, setupQuality, entryQuality, suggestedAction
  (confirmed by direct pattern search — no matches anywhere in the file)
        ↓
TradePriorityTab component (renders what the reader sees)
  Displays only: rank, pair, direction, stars, analysis,
                 entry, target_1, target_2, stop_loss, risk_reward
  No Setup Quality / Entry Quality / Suggested Action badge exists
  anywhere in this component's JSX.
        ↓
deriveTradeRatings() / getRatingsForCycle(): NEVER CALLED
```

**Conclusion:** Setup Quality, Entry Quality, and Suggested Action are not published on the Weekly Outlook page, and fixing this requires **two** separate changes, not one:

1. Extend `/api/trade-ideas/route.ts`'s `.select()` to include `tier`, `correlation_class`, `zone_position_pct` (and whatever else `TradeIdeaInput` needs).
2. Add the derivation call (via the adapter — see Finding 3) and corresponding badge rendering to `TradePriorityTab`.

Neither change exists today. This is a **v6-era gap**, unrelated to v7.

---

## Finding 2 — Dashboard (`app/page.tsx`): pre-v7 product gap, not a wiring task

**Confirmed:**
- `Select-String -Path ".\app\page.tsx" -Pattern "supabase|createClient|from\('|\.select\("` returned **zero matches**.
- The page's only "Weekly Outlook" references are static marketing/FAQ copy (e.g. "Weekly market outlook every Sunday"), not data-fetching code.

**Conclusion:** `app/page.tsx` performs no database or API calls of any kind. The Dashboard described in `PUBLISHING_PIPELINE.md` v1.5 ("Website Experience Layers," Layer 1 — `Pair | Opportunity (★1–5) | Entry | Recommendation`) **does not exist as a live page**. This is not an existing implementation waiting to be wired to ratings data — it is unbuilt product work. Treating this as in-scope for Phase 11 would silently expand Phase 11 from "wire v7 evidence into the existing website contract" into "build the website contract's Dashboard layer from scratch."

---

## Finding 3 — The one real, complete v6 implementation, confirmed orphaned

**`lib/derivation/tradeRatings.ts`** — pure derivation functions (`deriveSetupQuality`, `deriveEntryQuality`, `deriveSuggestedAction`, `deriveTradeRatings`). Correct, tested-by-reading, and as of commit `516f118`, fixed for the null-tier coercion bug (see Appendix).

**`lib/derivation/tradeRatingsAdapter.ts`** — a complete, spec-compliant (`PUBLISHING_PIPELINE.md` v1.8, Layer 1) adapter: `getRatingsForCycle()` fetches `trade_ideas` + `research_cycles` + `cot_positioning` for one cycle, gates on `cycleHasRatingsData()` (at least one idea with non-null tier), and returns rated ideas.

**Confirmed via full-repo search** (`Get-ChildItem -Recurse -Include *.ts,*.tsx | Select-String -Pattern "getRatingsForCycle|cycleHasRatingsData"`): exactly **3 matches, all inside `tradeRatingsAdapter.ts` itself** (its own two function definitions plus one internal call from `getRatingsForCycle` to `cycleHasRatingsData`). **Zero external callers anywhere in the codebase.**

**`app/live-trades/page.tsx`** — the only page anywhere that calls `deriveTradeRatings` and renders `setupQuality`/`entryQuality`/`suggestedAction` badges. It does **not** use the adapter — it reimplements the same `TradeIdeaInput`-construction logic inline, per-idea, inside a `.map()` over a **multi-cycle** query (`gte('created_at', thirtyDaysAgo)`, `.in('cycle_id', cycleIds)` for COT). This is a genuine, legitimate difference in query shape from the adapter's single-cycle contract (`.eq('cycle_id', cycleId).single()`) — not a careless duplication to blindly consolidate.

**Conclusion:**
- `/live-trades` is the **only confirmed public consumer** of the ratings derivation system, anywhere in the application.
- The adapter is correct and unused — a "built, not shipped" gap.
- No page anywhere references any v7 engine output (`HtfEvidencePacket`, `QmrAnalysis`, `eligibleDirection`, `qmrEligible`, `twentyDayPercent`, etc.) — confirmed by dedicated full-repo search returning zero hits in any non-admin page.

---

## Finding 4 — What Phase 11 is, once separated from what it is not

Given Findings 1–3, "Phase 11 — Website Contract" cannot honestly be scoped as a single task. It decomposes into three genuinely separate pieces of work:

| # | Task | v7-related? | Status |
|---|---|---|---|
| 1 | Wire Weekly Outlook to the ratings derivation (extend `/api/trade-ideas` select + call adapter + render badges) | No — pure v6 gap | Not started |
| 2 | Build the Dashboard page described in Publishing Pipeline v1.5 | No — new product work | Not started |
| 3 | Extend the ratings derivation with v7 `HtfEvidencePacket` evidence (QMR eligibility, HTF trend alignment, engine-computed 20D location) | Yes — the actual Phase 11 task | Not started; has no confirmed public consumer to reach beyond `/live-trades` until #1 and #2 progress |

**Boundary, established and to be respected going forward:**
- Do not combine these three into one implementation pass.
- Do not build the Dashboard (#2) merely because it would make Phase 11 "feel complete" — that decision needs to be made explicitly, as new product scope, not backed into via a v7 documentation phase.
- Do not extend the ratings system with v7 fields (#3) until a real consumer and its intended presentation are decided — extending `TradeIdeaInput` speculatively, with no page to render the result, risks building the same kind of orphaned code the adapter (Finding 3) already demonstrates is easy to produce in this codebase.

---

## Appendix — Unrelated fix committed during this investigation

While tracing `tradeRatingsAdapter.ts`, a genuine, independent bug was found and fixed:

**Bug:** `toTier(raw: number | null): Tier` returned `1` for `raw === null`, silently coercing "tier was never set" (the adapter's own documented signal for pre-schema / insufficient-data rows) into a false, stronger claim — "tied to a real, sourced catalyst this week." This caused `cycleHasRatingsData`'s gate (intended to suppress ratings for pre-schema cycles) to be defeated downstream, since by the time `deriveSetupQuality` ran, the null had already been erased.

**Fix (commit `516f118`):**
- `toTier` now returns `Tier | null`, preserving `null` for `raw === null`.
- `Tier` type widened to `1 | 2 | null`.
- `deriveSetupQuality` now returns `'Not Rated'` when `input.tier === null`, alongside its existing `Proxy-standalone`/missing-COT checks.

**Verification performed:** diff scoped to exactly these three changes (confirmed via `git diff`), `npx tsc --noEmit` clean. **No automated test exists for `lib/derivation/tradeRatings.ts`** (confirmed via full-repo search for `*.test.ts` files referencing this module — zero results) — this fix shipped on static/type verification only, by explicit, logged choice, not oversight. Adding coverage for `tradeRatings.ts` is recommended whenever Finding 4, item #3 is picked up, since that work will touch the same file.

This fix is independent of the Phase 11 scope question above and was committed on its own (`516f118`), not bundled with any Phase 11 work.
