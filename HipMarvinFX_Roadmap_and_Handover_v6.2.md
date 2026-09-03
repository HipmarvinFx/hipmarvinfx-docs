# HipMarvinFX v6.2 — Comprehensive Roadmap & Handover Brief

**Date:** 2026-08-27  
**Session Context:** Post-pipeline-debug, pre-prompt-restore  
**Status:** Technical infrastructure complete. Research output architecture drifted from original vision.

---

## 1. Executive Summary

The automated pipeline is **operationally live** and **technically sound**: market data fetches, validates, ranks pairs, runs AI synthesis, persists to Supabase, and renders in the admin UI. Every run today that reached the database had clean, verified prices.

However, the system has **drifted from the original v5/v6 research architecture**. What was built is a **technical pair screener with AI commentary**. What was intended is a **macro-fundamental + technical unified research desk** that produces parser-ready structured output: G10 Macro Regime, Scenario Matrix, COT Positioning, structured Trade Priority Lists with Entry/Stop/TP/R:R, Daily Game Plans, and Position Ledger updates.

**This document is the historical v6.2 roadmap/handover record. It is preserved for provenance and session continuity. It is not the current implementation directive where a later decision record supersedes it.**

---

## 2. Original Vision (v5/v6 Prompt Architecture)

The HipMarvinFX research output is defined by `STANDING_PROTOCOL_v7.md`, `HipMarvinFX_Generation_Prompts_v6_0.md`, `WEEKLY_RESEARCH_TEMPLATE_v4.md`, and `DAILY_UPDATE_TEMPLATE_v3.md`.

The research hierarchy is:

```text
Macro Regime (G10 table: Inflation vs Target, Labour Trend, Policy Pressure, Macro Regime)
    |
Weekly Thesis (Core Macro Question -> Institutional Answer -> Pair-Level Translation)
    |
Macro Drivers (up to 4 events, 6-subfield analysis: regime tested / confirms / strengthens / weakens / breaks / FX implication)
    |
Scenario Matrix (per-event: macro trigger, liquidity destination, flow regime, confirmation, invalidation, trade implication)
    |
COT Positioning (Leveraged Funds vs Asset Managers, wk/wk change, read)
    |
Trade Priority List (Entry, Stop, TP1, TP2, R:R, Timeframe, Correlation Class, Daily Zone, Tier, Macro Regime, Macro Alignment, Liquidity/Flow Regime, Liquidity State, Zone/Flow Relationship, Reasoning)
    |
Position Ledger (auto-updated when new idea published, copies Rule 23 fields)
    |
Daily Game Plan (forward guidance: what to set/watch, pre-event discipline, size-reduction windows, reassessment triggers)
```

**Core principle (v6):**
> The 20-day range tells us where price is. Liquidity tells us where price is reaching. Acceptance/rejection tells us whether to follow or fade.

The **Daily Zone is location, not direction**. Premium does not automatically mean sell; Discount does not automatically mean buy. Direction comes from flow regime + macro alignment + acceptance evidence.

**Parser integrity (Rule 20):** Headings, field names, order, markers, and confidence language are parser contracts. The AI output must be parser-ready so structured data can be extracted automatically.

---

## 3. What Got Built (Current Reality)

The current pipeline produces:

```text
Price Data (Twelve Data + Yahoo Finance)
    |
Price Validation (cross-provider, anti-fabrication)
    |
Currency Strength Ranking
    |
20D Location + Liquidity + Flow Regime
    |
Pair Discovery (SELECT / WATCH / REJECT)
    |
AI Synthesis (Gemini -> Groq -> OpenRouter failover)
    |
Price Verification (post-AI check against real closes)
    |
Supabase Persist (research_cycles + trade_ideas)
    |
Admin UI (PipelinePreview)
```

**Trade ideas in the database today:**

| pair | direction | conviction_score | status | analysis |
|------|-----------|------------------|--------|----------|
| AUDUSD | Short | 75 | Waiting | Auto: DIRECTIONAL flow, HIGH, zone 73% |
| EURUSD | Short | 60 | Waiting | Auto: RANGE flow, LOW, zone 45% |

Compare to the v6 template requirement for a single trade idea: **14 structured fields** (Entry, Stop, TP1, TP2, R:R, Timeframe, Correlation Class, Daily Zone, Tier, Macro Regime, Macro Alignment, Liquidity/Flow Regime, Liquidity State, Zone/Flow Relationship, Reasoning).

**Current output coverage: ~1% of required structure.**

---

## 4. Gap Analysis: Drift Assessment

| Component | Original Requirement | Current Status | Severity |
|-----------|---------------------|----------------|----------|
| **G10 Macro Regime** | 10-currency table with inflation, labour, policy, regime ranking | Absent. `macro_thesis` = `TBD` or AI fluff | Critical |
| **Core Macro Question** | One-sentence dominant cross-currency question | Absent | Critical |
| **Institutional Macro Read** | Inflation -> labour -> CB reaction -> relative strength | Absent | Critical |
| **FX Implication** | Structural favour/sell backdrop before any pair named | Absent | Critical |
| **Macro Drivers** | 4 events x 6 subfields | Absent. Evidence ledger = `COT: N/A, Central Banks: N/A, News: N/A` | Critical |
| **Scenario Matrix** | Per-event headers with triggers, liquidity, confirmation, invalidation | Absent | Critical |
| **COT Positioning** | Table: Pair / Net Position (Leveraged+Asset Mgr) / Direction / Read | Stub only (`cot?.status || N/A`) | High |
| **Daily Game Plan** | Forward guidance: set/watch, discipline windows, reassessment | Absent | High |
| **Trade Priority List** | Full 14-field structured ideas | Only `pair, direction, score, status, analysis` | Critical |
| **Entry/Stop/TP/R:R** | Every idea gets a stop immediately | Absent | Critical |
| **Daily Zone** | Calculated from chart, % + label (Discount/Premium/etc) | Computed in engine, never written to DB or AI output | Medium |
| **Liquidity/Flow Regime** | DIRECTIONAL / RANGE / TRANSITION with acceptance evidence | Computed in engine, not in DB schema or prompt | Medium |
| **Correlation Check (Rule 5)** | Flag when multiple ideas are really one correlated bet | Absent | Medium |
| **Zone Alignment Check (Rule 21B)** | Flag when Buy is in Premium or Sell in Discount | Absent | Medium |
| **Position Ledger** | Auto-update with Rule 23 fields on new idea | `trade_results` table exists, no integration | High |
| **Parser-Ready Output** | Structured markdown with parser-visible headings/markers | Raw AI text dumped to `macro_thesis` | Critical |
| **Price Verification** | Post-AI check against real closes | Working | Low (done) |

---

## 5. Confirmed Working End-to-End

These components were tested on real runs at the time of this handover and were production-ready:

1. Silent persist failures surface real errors (`PARTIAL_SUCCESS` + `persistError`).
2. `week_number` schema bug removed; `research_cycles` writes succeed.
3. `trade_ideas_status_check` violation resolved (`draft` -> `Waiting`).
4. `research_cycles_title_unique` violation handled by existence check keyed on `title`.
5. All-Long direction bug fixed; `getDirectionFromStrength()` produces real Long/Short mixes.
6. Duplicate accumulation fixed with delete-then-insert per cycle, scoped to `Waiting` only.
7. AI price hallucination mitigated by grounding prompts with exact `priceLevels`.
8. Output-side price verification catches mismatches and blocks persistence.
9. Weekly pipeline parity with daily pipeline for price grounding, verification and persistence.
10. Pair format normalization removed `EUR/USD` vs `EURUSD` split.
11. Title encoding uses ASCII hyphen.
12. Currency strength ranking produces real scores and direction logic.
13. 20D location calculation computes `locationPct`, `locationWord`, `low20`, `high20`.
14. Liquidity engine detects consolidation zones.
15. Flow regime detection classifies `DIRECTIONAL/RANGE/TRANSITION`.
16. Pair discovery matrix implements SELECT/WATCH/REJECT.
17. AI failover wiring exists.
18. Supabase persistence works for `research_cycles` + `trade_ideas`.
19. Admin UI / PipelinePreview renders cycle + ideas.
20. Cron endpoint protection uses `CRON_SECRET`.

---

## 6. Broken / Unaddressed at Handover Time

Known issues included weekly cycle labeling, weekly uniqueness constraints, AI provider failures, week date calculation, retry behavior, missing structured trade extraction, missing macro data ingestion, missing executable trade levels, missing Position Ledger automation, missing Daily Game Plan and Scenario Matrix generation, and missing parser integration.

These are preserved here as the **2026-08-27 historical state**, not as a claim that every item remains unresolved today.

---

## 7. Original Recommended Roadmap

### Phase 0: Hotfixes

Fix weekly cycle labeling, weekly uniqueness handling, AI provider model/limit issues, week date calculation, dead duplicate files, and retry behavior.

### Phase 1: Restore the Research Architecture

The original handover proposed rewriting the compact prompt to the full v6 structure, expanding trade idea schema, wiring all v6 fields, building a research parser, adding entry/stop/TP calculation, Zone Alignment Check, and Correlation Concentration Check.

### Phase 2: Macro Data Ingestion

The original handover proposed:

| # | Task | File |
|---|------|------|
| 2.1 | COT data ingestion (CFTC API) | `lib/data/cot.ts` |
| 2.2 | Central bank policy / speech RSS scraping | `lib/data/central-banks.ts` |
| 2.3 | Economic calendar integration | `lib/data/economic-calendar.ts` |
| 2.4 | Inflation data | `lib/data/inflation.ts` |
| 2.5 | News filtering | `lib/data/news.ts` |
| 2.6 | Macro regime scoring engine | `lib/engine/macro-regime.ts` |
| 2.7 | Wire data sources into evidence ledger | `lib/engine/evidence-ledger.ts` |

**Important provenance:** Phase 2 was explicitly a planned/future phase in this handover; it was **not recorded as already implemented**.

### Phase 3: Position Ledger & Execution

Auto-update Position Ledger, add thesis invalidation, daily reassessment triggers, Position Ledger UI, and trade result tracking.

### Phase 4: Polish & Documentation

Update briefs, API shape, pair-format handling, API documentation and production verification.

---

## 8. Original Immediate Next-Step Recommendation

The handover originally recommended running the hotfixes, rebuilding, testing the weekly pipeline, then choosing between restoring the full v6 prompt or adding macro data sources first.

**This recommendation was later superseded by `HipMarvinFX_Implementation_Directive.md`, which explicitly rejected the premise that an LLM should author factual macro/research content as the system of record.**

---

## 9. File Inventory at Handover

The handover documented the state of the admin UI, cron routes, Supabase writer, AI adapters, technical engines, evidence ledger, pipeline, market-data adapters, validator, cache and related files as of 2026-08-27.

The important distinction is that this was a **historical implementation snapshot**, not a current audit.

---

## 10. Critical Rules & Constraints

1. **Never fabricate missing data** — mark as `PENDING` or `NOT_AVAILABLE`.
2. **Treat 20D zone as location, not direction** — Premium can still continue higher.
3. **Flow regime determines direction** — not Premium/Discount assumptions.
4. **Cross-provider validation is mandatory** — never trust a single source.
5. **Every trade idea gets a stop immediately** — no exceptions.
6. **Parser integrity is non-negotiable** — headings, field names and markers are contracts.
7. **Macro Regime affects Setup Quality, not Entry Quality.**
8. **Emergency override exists for a reason** — use it when automation fails.
9. **Correlation concentration must be flagged.**
10. **Zone alignment conflicts must be stated plainly** — soft flag, not necessarily a hard block.

---

## 11. Session Audit Trail

**2026-08-27 Session — recorded work:** pipeline preview and persistence fixes, direction logic, database cleanup, price grounding and verification, weekly parity, and diagnosis of the remaining architecture gaps.

**What was not completed at that time:** the remaining hotfixes, full v6 prompt restoration, Phase 2 macro ingestion, Position Ledger automation, and related parser/research architecture work.

---

## 12. Environment Variables

The original handover recorded the following required environment variables and their purpose: `TWELVE_DATA_API_KEY`, `GEMINI_API_KEY`, `GROQ_API_KEY`, `OPENROUTER_API_KEY`, `CRON_SECRET`, `NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`, and `SUPABASE_SERVICE_ROLE_KEY`.

**No secret values belong in this public documentation repository.**

---

## Provenance Notice

This file is preserved in the public documentation repository so future team members and AI/development sessions can distinguish the **historical v6.2 roadmap/handover** from later decisions. It must not be interpreted as proof that every proposed Phase 2 component was ever implemented.
