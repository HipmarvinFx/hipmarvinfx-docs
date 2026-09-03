# HipMarvinFX — Implementation Directive

**Status:** Decision record / implementation handoff

This document supersedes the "Option A / Phase A: restore the v6 prompt" recommendation in `HipMarvinFX_Roadmap_and_Handover_v6.2.md` and its predecessor session.

This is a decision record, not a new proposal — the analysis is done; this is what to go build.

## Decision confirmed: Option 2

Track B's engine computes and exposes technical data. It does not author trade ideas, macro narrative, or write to `trade_ideas` / `research_cycles` going forward. Track A's existing analyst-authored + parser-imported workflow remains the only path that writes a trade idea to the database.

No schema migration is required to execute this. That was only needed under Option 1, which is not the path being taken.

---

## Stop / deprecate

| File / area | Action |
|---|---|
| `app/api/cron/daily/route.ts` | Remove or disable the block that calls the AI provider chain and writes `research_cycles`/`trade_ideas`. The cron can still run for data-fetch purposes, but its output stops being persisted as authored research. |
| `app/api/cron/weekly/route.ts` | Same — disable the AI-authored persist path. |
| `lib/ai/adapters/index.ts`, `gemini.ts`, `groq.ts`, `openrouter.ts` | Retire as a content-generation chain. Do not spend further effort fixing provider failures solely to preserve this deprecated authored-research path. |
| `app/lib/supabase/cron-writer.ts` | Stop calling this to write `macro_thesis`, `conviction_score`, or free-text `analysis` as authored trade research. If retained, it should only write clearly separated engine-derived numeric fields. |
| "restore the v6 prompt" work item | Fully shelved. Not deferred. The premise that an LLM should originate macro/scenario content is rejected. |

## Keep unchanged

- `lib/engine/currency-strength.ts`
- `lib/engine/twenty-day-location.ts`
- `lib/engine/liquidity.ts`
- `lib/engine/flow-regime.ts`
- `lib/engine/pair-discovery.ts`
- `lib/engine/best-expression.ts`
- `lib/market-data/adapters.ts`
- `lib/market-data/validator.ts`
- `lib/market-data/cache.ts`

These are tested infrastructure that produces inputs rather than trading conclusions.

## Repurpose

Turn the pipeline from **generate and persist research** into **compute and expose a snapshot**:

- The cron job, or a simpler on-demand endpoint, runs the engine layer and returns/stores numbers only: per-pair currency strength score, 20D zone percentage + label, flow regime classification, and liquidity state.
- Surface this in `app/admin/page.tsx` as a read-only panel clearly separated from the Weekly Outlook / Daily Update importer UI.
- Label it as a **technical snapshot**, not research output.
- The analyst reads the panel while writing the Weekly Outlook manually. It never auto-populates a trade idea field.

## No action needed

- `trade_ideas` schema: no new columns under this decision. The Week 31 columns (`timeframe`, `correlation_class`, `zone_position_pct`, `zone_label`, `tier`) stay exactly as Track A already built them, written only by the analyst-import path.
- Position Ledger, `/trades`, the admin importer, and Daily Actionable Blocks remain Track A and are untouched by this directive.

---

## Actually next

This directive removes a distraction; it does not create a new urgent workstream. The open items already logged in the project handover remain the critical path, including:

1. Day 3 FOMC content — rewrite into the real Daily Update structure.
2. Zone/Tier/Correlation parser code — drafted but not pasted into `app/admin/page.tsx`; perform the identified line-number check before replacement.
3. Week-Close Review parser — write from scratch against the real `## WEEK N RESULTS` structure rather than patching.
4. `WEEK31_WEEKLY_RESEARCH.md` missing Timeframe/Correlation/Zone/Tier fields — check directly against real `trade_ideas` rows.

---

## Who should execute this

The file-level implementation changes require actual repo/deployment access. This directive is intended to be handed directly to a development session connected to the real `hipmarvinfx` repository.

---

## Provenance / governance notice

This decision record is now stored in the public documentation repository alongside the historical v6.2 handover so future team members and AI sessions can distinguish:

**historical roadmap → later implementation decision → current development work**.

Do not treat the v6.2 roadmap as the current implementation authority where this decision record explicitly supersedes it.

**No secrets or API key values belong in this public documentation repository.**
