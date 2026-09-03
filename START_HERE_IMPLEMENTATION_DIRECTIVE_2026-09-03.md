# HipMarvinFX — Definitive Implementation Directive
## New-Session Team Handover — 3 September 2026

**Status:** CANONICAL NEW-SESSION HANDOVER  
**Audience:** Developers, AI coding agents, technical collaborators, and reviewers starting a new session  
**Repository:** `HipmarvinFx/hipmarvinfx-docs`  

---

## 1. PURPOSE

This document is the starting directive for any team member or AI agent entering the HipMarvinFX project in a new session.

The objective is to prevent the project from repeatedly reopening settled decisions, treating proposals as completed work, or rebuilding deprecated architecture.

**Do not start by coding. Start by establishing the current repository and application reality.**

This directive is a navigation and implementation-control document. It does not replace the canonical methodology documents listed below.

---

## 2. MANDATORY READING ORDER

Read these documents before making implementation decisions:

1. `ONBOARDING_START_HERE.md`
2. `HipMarvinFX_Implementation_Directive.md`
3. `V7_QMR_HTF_MASTER_IMPLEMENTATION_BRIEF.md`
4. `V7_EVIDENCE_ARCHITECTURE_AND_IMPLEMENTATION_DIRECTIVE.md`
5. `V7_INTEGRATION_BOUNDARY_LIFT.md`
6. `SHIFT_HANDOVER_2026-09-01_YahooEvidenceAdapter.md`
7. `HipMarvinFX_Roadmap_and_Handover_v6.2.md` — historical context only

Then inspect the **actual application repository, database state, deployment state, and current branch/commit** before claiming any implementation item is complete.

The public `hipmarvinfx-docs` repository is primarily the methodology/contracts/handover/provenance layer. It is not, by itself, proof that the production application implements every documented specification.

---

## 3. CURRENT GOVERNING PRINCIPLE

The project is moving toward a deterministic evidence-driven v7 architecture:

`External Source → Evidence Adapter → Validation → Evidence Store → Technical/Macro Engine → Evidence Packet → AI/Analyst → Research File → Parser → Publication`

The central rule is:

> **AI may interpret verified evidence, but AI must never manufacture missing market facts.**

No fabricated price, trend, structural break, liquidity state, QMR phase, QM/QML pattern, entry, stop, target, or R:R may enter the published system.

---

## 4. DEFINITIVE STATUS MODEL

Use these statuses when discussing project state:

- **CLOSED** — explicitly completed/verified in the documented state record.
- **OPEN** — explicitly unfinished or identified as the next implementation task.
- **SUPERSEDED** — replaced by a later decision or architecture.
- **UNKNOWN** — mentioned or implied but not sufficiently verified. Never convert UNKNOWN into CLOSED by assumption.

When a document claims something is complete but no implementation/runtime evidence exists, describe it as **documented as closed, runtime verification pending** rather than silently upgrading confidence.

---

## 5. CLOSED / LOCKED DECISIONS

### Methodology

- v7 trend-first doctrine is locked.
- Daily = broad structure/context.
- 4H = actionable trend/structure.
- 1H = minimum execution timeframe.
- No sub-1H execution.
- Daily/4H conflict means transition/conflict; do not force a directional conclusion.
- Premium/Discount and 20D location describe location, not direction.
- Liquidity sweep alone does not prove reversal.
- QMR means **Quality → Manipulation → Reaction**.
- QMR continuation is the primary use case.
- Countertrend requires the defined HTF structural-break gate.
- QM/QML is an entry refinement, not the bias engine.
- A cross must not be excluded merely because it is a cross.
- A trend must not be faded merely because price is in Premium.

### Evidence / data path

The September 1 handover records the following as completed/verified at that point:

- `yahoo-fx` adapter existed and conformed to the market-data provider interface.
- Yahoo daily OHLC was live-verified.
- Invalid/in-progress candles with null close were rejected by candle validation.
- Native Yahoo 4H boundary mismatch was identified.
- The new approach is to derive the required 4H structure from validated 1H data rather than depend on Yahoo's native 4H bucket boundaries.
- Gold/silver mapping was corrected to futures symbols (`GC=F` / `SI=F`) and live-verified.
- Daily/weekly cron provider order was changed to `[yahoo-fx, twelve-data]`.
- The buggy old `yahoo-finance` provider was removed from that provider list.
- Daily packet-variable shadowing was fixed.
- Evidence packet types were correctly re-exported from parser types.
- TypeScript check was reported clean after those fixes.

These are **documented state claims from the shift handover**. Re-verify against the live application before relying on them for production assertions.

---

## 6. SUPERSEDED ARCHITECTURE / DO NOT RESTORE

### Do not restore the v6 prompt chain

The request to “restore the v6 prompt” is shelved. Do not revive it as a shortcut.

### Do not make AI the factual data source

AI must not be used to invent or guess current prices or technical state.

### Do not revive the old authored-research persistence path as the core v7 architecture

The current decision separates:

- **Track B:** deterministic engines compute/expose technical data.
- **Track A:** analyst-authored/parser-imported trade ideas remain the controlled persistence path where applicable.

Track B does not author trade ideas, macro narrative, `trade_ideas`, or `research_cycles` merely to recreate the old workflow.

### Evidence Packet integration correction

The September 1 application inspection corrected an earlier architectural assumption: in the actual application, `buildEvidencePacket()` was not an upstream AI input gate. It was being used as parallel/non-blocking verification around parsing.

Do not assume the documentation diagram represents the current application wiring. Inspect the code before changing the integration boundary.

### Native Yahoo 4H as the canonical bucket source

Do not revert to native Yahoo 4H bars for the required 00/04/08 UTC-style structure buckets when the implementation is using derived 4H from validated 1H data.

---

## 7. CURRENT OPEN IMPLEMENTATION QUEUE

The following is the working queue unless a newer verified handover explicitly closes or supersedes an item.

### Priority 1 — HTF deterministic structure

Implement and test the v7 HTF chain:

1. Reliable market data.
2. Daily OHLC.
3. 4H OHLC derived from validated 1H data.
4. 1H OHLC.
5. Daily structure.
6. 4H structure.
7. Trend alignment.
8. Protected swing identification.
9. Structural-break detection.
10. QMR state.
11. QM/QML refinement.

The first immediate engineering focus after the market-data work is **Step 3: HTF structure engine**.

### Priority 2 — QMR / HTF implementation

Implement the canonical QMR/HTF brief deterministically.

The engine must not infer an entry simply because a QMR-looking sequence exists. It must enforce the documented trend, structure, liquidity, reaction, and countertrend gates.

### Priority 3 — Evidence-to-AI-to-parser vertical slice

Prove one complete path with real evidence:

`real market data → validated evidence → deterministic technical state → evidence packet → constrained AI interpretation → parser validation → eligible publication`

Required adversarial tests include:

- fabricated price rejected;
- rejected output produces zero persistence;
- valid evidence-backed output passes validation and becomes eligible for persistence.

### Priority 4 — Parser / admin synchronization

Complete the remaining v7 parser work, including the items recorded in the latest implementation directive/handover, notably:

- Zone/Tier/Correlation parser handling in `app/admin/page.tsx`;
- Week-Close Review parser rewrite;
- WEEK31 weekly-research field reconciliation;
- synchronization between parser contracts and actual application schema.

### Priority 5 — Known legacy/open defects

Investigate and close, without silently deleting historical provenance:

- `lookupPairsForCycle()` stub;
- old `YahooFinanceAdapter` 4H→1H mislabeling;
- orphaned `exchangerate-host.ts`;
- `week_start` / `week_end` bug;
- weekly-cycle mislabeling;
- `research_cycles_week_unique` constraint issue;
- Position Ledger DB table, if still absent in the application;
- Gemini/Groq provider issues only if the current architecture still requires them.

### Priority 6 — Macro evidence productionization

The full macro layer remains unfinished/deferred unless a newer verified state document says otherwise.

Do not claim production completion for:

- COT ingestion;
- central-bank ingestion;
- economic-calendar ingestion;
- inflation-data ingestion;
- news/RSS ingestion;
- full Macro Engine;
- production macro Evidence Store/ledger;

without implementation and runtime evidence.

The existing macro/evidence documents are specifications and architectural commitments unless separately verified as implemented.

### Priority 7 — Production cutover

Do not declare v7 production-ready until the vertical slice has been proven with real data, adversarial validation, zero-persistence-on-rejection, and successful synchronization/publication tests.

The existing v6.1 production path remains protected until the v7 shadow/validation path is demonstrably safe.

---

## 8. REQUIRED FIELD CONTRACT FOR TRADE OUTPUT

Where the application produces a trade/research object, the v7 contract must account for the canonical fields:

- Pair
- Direction
- Entry
- Stop
- TP1
- TP2
- R:R
- Execution TF
- Daily Zone
- Tier
- Macro Regime / Alignment
- HTF Trend
- Trend Alignment
- Structural Break
- QMR Phase
- QM/QML
- Liquidity / Flow
- Liquidity Target
- Zone / Flow relationship
- Reasoning

A field must not be populated with invented values merely to satisfy schema completeness.

Use explicit unavailable/pending states where the architecture permits them.

---

## 9. AI BOUNDARY

AI may:

- interpret verified evidence;
- explain relationships between verified fields;
- generate constrained narrative;
- produce analyst-facing reasoning from supplied facts.

AI may not independently create:

- current prices;
- OHLC values;
- HTF trend;
- protected swings;
- structural breaks;
- liquidity state;
- flow state;
- QMR phase;
- QM/QML pattern;
- entry;
- stop;
- target;
- R:R;
- macro facts that are absent from the evidence packet.

If evidence is unavailable, the correct output is **NOT_AVAILABLE / PENDING / STALE / INVALID**, as appropriate — not a guess.

---

## 10. IMPLEMENTATION DISCIPLINE FOR EVERY NEW SESSION

Before changing code:

1. Identify the actual application repository.
2. Identify current branch and commit.
3. Inspect current implementation, not just documentation.
4. Locate the relevant existing function/file before creating a replacement.
5. Check whether the task is CLOSED, OPEN, SUPERSEDED, or UNKNOWN.
6. Check the latest handover for changes to earlier assumptions.
7. Make the smallest architecture-consistent change.
8. Run typecheck/tests relevant to the change.
9. Record exact files changed and verification performed.
10. Commit and push only after verification.

Never say “already implemented” solely because a specification exists.

Never say “not implemented” solely because an old document says it was not implemented; inspect the latest application state first.

---

## 11. PROVENANCE RULE

Historical documents remain valuable evidence of what the team previously decided or attempted.

Do not rewrite history to make the project appear cleaner.

If an old directive has been superseded, leave the historical file intact and state clearly that the newer directive controls current implementation.

In particular, `DEVELOPMENT_DIRECTIVE_AUTOMATED_MACRO_EVIDENCE_V7.md` should be treated as a historical/proposal artifact unless a later decision explicitly reactivates its instructions. Do not silently treat it as the current implementation authority.

---

## 12. DEFINITION OF DONE

A feature is not DONE because:

- the Markdown specification exists;
- a function compiles;
- an AI response looks plausible;
- a parser accepts a field;
- a mock dataset passes;
- a developer says it works.

For v7 factual/market-data features, DONE requires appropriate evidence that:

`real source → adapter → validation → deterministic state → constrained interpretation → parser validation → persistence/publication`

works as intended, including rejection behavior for fabricated or invalid evidence.

---

## 13. FIRST ACTION IN A NEW SESSION

The first message/action should effectively be:

> **“I have read the canonical handover. I will now inspect the actual application repository and current runtime state, compare it against this directive, and report CURRENT REALITY vs REQUIRED before changing code.”**

Then produce a concise matrix:

| Area | Documented Status | Actual Runtime Status | Gap | Next Action |
|---|---|---|---|---|
| Market data | CLOSED/documented | verify | — | verify current provider path |
| HTF structure | OPEN | inspect | likely implementation gap | implement/test |
| Trend alignment | OPEN | inspect | — | implement/test |
| Structural break | OPEN | inspect | — | implement/test |
| QMR | OPEN | inspect | — | implement/test |
| QM/QML | OPEN | inspect | — | implement/test |
| Evidence → AI → parser | OPEN | inspect | — | prove vertical slice |
| Macro ingestion | OPEN/DEFERRED | inspect | — | do not claim complete |
| Publication cutover | OPEN | inspect | — | gated by adversarial proof |

Do not proceed on assumptions hidden behind documentation.

---

## 14. FINAL AUTHORITY RULE

If documents conflict:

1. A newer explicit implementation decision supersedes an older decision.
2. A verified application/runtime state supersedes an unverified assumption.
3. The canonical v7 methodology governs trading logic.
4. The latest state handover governs current implementation reality until contradicted by newer verified evidence.
5. Unknown remains unknown until verified.

**No developer or AI agent should reopen a superseded architecture without an explicit new decision.**

---

**End of canonical new-session implementation directive.**
