# HipMarvinFX — Developer Onboarding: START HERE

## PURPOSE

This file is the starting order for any developer joining after an expired or interrupted development session.

Do not begin by rebuilding, redesigning, or “restoring” anything.

First reconstruct the project's authoritative state, then verify the live implementation.

---

## 1. START WITH THE CURRENT DECISION RECORD

Read:

`HipMarvinFX_Implementation_Directive.md`

This is the first authority for current architectural decisions in the documented handover.

Rules:
- Follow its decisions unless a newer dated decision record explicitly supersedes them.
- Do not revive the shelved “restore the v6 prompt” approach.
- Do not make the AI/LLM the author of trade ideas, macro narrative, or database research records unless a newer explicit decision authorizes that change.
- Track A analyst-authored/parser-imported research remains the authoritative trade-idea write path unless superseded.
- Track B technical engines compute/expose technical data; they do not author trade ideas.

---

## 2. READ THE HISTORICAL HANDOVER SECOND

Read:

`HipMarvinFX_Roadmap_and_Handover_v6.2.md`

Treat this as historical context and provenance, NOT as proof of current implementation.

Critical rule:
- Phase 2 macro-data ingestion in this document was planned work and was explicitly not started.
- Do not describe COT, central-bank, calendar, inflation, news/RSS, macro-regime, or evidence-ledger ingestion as previously implemented unless the actual repository proves it.

---

## 3. READ THE CURRENT CANONICAL TRADING/METHODOLOGY DOCUMENTS

Locate and read the latest `STANDING_PROTOCOL_*` document in the repository.

Use the highest-numbered/current canonical version actually present in the repository.

Pay particular attention to:
- Rule 1: never fabricate.
- Evidence/source separation.
- Actual-price-only status changes.
- HTF hierarchy: Daily → 4H → 1H.
- Trend-first doctrine.
- Structural-break countertrend gate.
- 20D location.
- Liquidity.
- Flow regime.
- QMR.
- QM/QML.
- Premium/discount context.
- Liquidity-target discipline.

Never replace these rules with personal trading preferences without an explicit project decision.

---

## 4. AUDIT THE ACTUAL APPLICATION REPOSITORY

The public `hipmarvinfx-docs` repository is the documentation/provenance repository. It is NOT, by itself, sufficient evidence of the live application implementation.

Obtain access to the actual application/deployment repository before claiming implementation status.

Then verify, directly in code and configuration:

- Next.js/application routes.
- Supabase schema and tables.
- Cron routes and authentication.
- Market-data adapters.
- Price validation.
- Parser firewall.
- AI adapters/providers.
- Technical engines.
- Admin UI.
- Research import/publish paths.
- Deployment configuration.
- Environment-variable names only; NEVER expose secret values.

Do not infer code existence from documentation.

---

## 5. VERIFY DATABASE AND DEPLOYED STATE

Confirm the real state of:

- `research_cycles`
- `trade_ideas`
- `trade_results`
- `calendar_events`
- `scenario_matrix`
- `daily_actionable_blocks`
- `derived_publications`

Also verify which routes currently write to these tables.

The question is not “what does the documentation say?”

The question is:

`What does the current code + database + deployment actually do?`

---

## 6. PRESERVE THE ANTI-FABRICATION FIREWALL

Never fabricate:

- market prices
- economic releases
- COT figures
- central-bank statements
- calendar events
- news
- technical signals
- database state
- implementation status

If evidence is unavailable, preserve the project's explicit unavailable/pending state rather than inventing a value.

Do not “repair” missing data by guessing.

---

## 7. CURRENT DOCUMENTED ARCHITECTURAL POSITION

Technical engines may compute and expose numerical/technical snapshots such as:

- currency strength
- 20D location
- liquidity state
- flow regime
- pair discovery
- best expression

These outputs must not automatically become analyst-authored trade ideas unless a newer explicit decision changes the architecture.

The existing analyst-authored/parser-imported research workflow remains authoritative for trade-idea persistence.

---

## 8. DO NOT RESURRECT OLD OR UNSUPPORTED WORK

Before implementing anything described as “restore,” “re-enable,” “bring back,” or “previously existed”:

1. Find the relevant historical document.
2. Determine whether it says IMPLEMENTED, PLANNED, PROPOSED, DEPRECATED, or NOT STARTED.
3. Verify the actual code.
4. Verify database/deployment behavior.
5. Only then classify the work.

A historical plan is not evidence of historical implementation.

---

## 9. REQUIRED FIRST DELIVERABLE FROM THE NEW DEVELOPER

Before changing code, produce a short:

`CURRENT REALITY vs DOCUMENTED REQUIREMENT`

matrix containing:

| Area | Documented state | Actual code state | DB/deployment state | Gap | Action |
|---|---|---|---|---|---|

Include at minimum:
- Market data
- Price validation
- Parser firewall
- Technical engines
- AI providers
- Cron routes
- Research persistence
- Admin UI
- COT
- Economic calendar
- Central-bank data
- Inflation data
- News/RSS
- Macro regime engine
- Evidence layer

Do not implement major changes until this matrix is produced and checked against the actual repository.

---

## 10. ORDER OF AUTHORITY

When documents conflict, use this order:

1. Newer explicit project decision record.
2. Current canonical `STANDING_PROTOCOL_*`.
3. Current implementation contract/specification.
4. Verified application code.
5. Database/deployment behavior.
6. Historical handovers and roadmaps.
7. Old recommendations/proposals.

However, implementation code and deployment state must always be verified before claiming what is actually live.

---

## 11. FIRST COMMANDMENT

**DO NOT START CODING UNTIL YOU KNOW WHAT CURRENTLY EXISTS.**

The objective is continuation, not reconstruction by assumption.

Preserve provenance.
Preserve the anti-fabrication rules.
Verify before modifying.
Document every material architectural change.
