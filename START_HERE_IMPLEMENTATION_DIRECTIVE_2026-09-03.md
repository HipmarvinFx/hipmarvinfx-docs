# HipMarvinFX — Definitive New-Session Implementation Directive
## Mandatory Starting Point for Every New Development Session

**Date:** 3 September 2026  
**Status:** CANONICAL / MANDATORY NEW-SESSION STARTING POINT  
**Audience:** Developers, AI coding agents, technical collaborators, and reviewers  
**Documentation repository:** `HipmarvinFx/hipmarvinfx-docs`  
**Application repository:** `HipmarvinFx/hipmarvinfx`  
**Current application commit at handoff:** `a82d8c2`

---

## 1. THIS IS THE FIRST DOCUMENT TO READ IN EVERY NEW SESSION

Every new HipMarvinFX development session must begin here.

The purpose is to prevent:

- repeated reconstruction of project history;
- reopening decisions that have already been superseded;
- treating documentation as proof of implementation;
- treating proposed work as completed work;
- restoring deprecated architecture;
- allowing AI to invent factual market information;
- starting implementation before current application reality is established.

**The project is NOT complete.**

The previous session completed the documentation/state handoff. It did **not** complete the v7 application implementation.

Therefore:

> **Handoff complete ≠ project complete.**

---

## 2. MANDATORY FIRST ACTION

Do NOT begin by coding.

First:

1. Read this document completely.
2. Read the canonical documents listed below.
3. Inspect the actual application repository.
4. Confirm the current branch and commit.
5. Inspect the current implementation, database/schema, tests, and deployment/runtime state where applicable.
6. Reconcile the documentation against the actual application.
7. Produce a **CURRENT REALITY vs REQUIRED** matrix.
8. Only then select the highest-priority verified implementation gap.

The first engineering deliverable is therefore **state verification**, not feature coding.

---

## 3. MANDATORY READING ORDER

Read these before making implementation decisions:

1. `ONBOARDING_START_HERE.md`
2. `HipMarvinFX_Implementation_Directive.md`
3. `V7_QMR_HTF_MASTER_IMPLEMENTATION_BRIEF.md`
4. `V7_EVIDENCE_ARCHITECTURE_AND_IMPLEMENTATION_DIRECTIVE.md`
5. `V7_INTEGRATION_BOUNDARY_LIFT.md`
6. `SHIFT_HANDOVER_2026-09-01_YahooEvidenceAdapter.md`
7. `HipMarvinFX_Roadmap_and_Handover_v6.2.md` — historical context only

Then inspect the actual application repository at the current commit.

The September 1 shift handover **exists in the repository**. Its later continuation supersedes earlier tentative/open assumptions inside that document where the two differ.

---

## 4. SOURCE-OF-TRUTH HIERARCHY

When determining project state, use this hierarchy:

1. **Verified current application/runtime state**
2. **Latest explicit implementation decision**
3. **Latest state handover**
4. **Canonical v7 methodology/specification**
5. **Older roadmap/history**

Documentation does not prove implementation.

A commit does not by itself prove runtime correctness.

A function existing in source does not by itself prove that the feature works.

If implementation status cannot be verified, classify it as **UNKNOWN** rather than CLOSED.

---

## 5. DEFINITIVE STATUS MODEL

Use exactly these classifications when reconciling state:

- **CLOSED** — implementation/decision is sufficiently verified and no remaining work is required for the defined scope.
- **OPEN** — known work remains.
- **PARTIALLY IMPLEMENTED** — some implementation exists, but the defined requirement is not fully proven/complete.
- **SUPERSEDED** — replaced by a later decision or architecture.
- **UNKNOWN** — insufficient evidence to classify confidently.

Do not convert UNKNOWN into CLOSED by assumption.

Do not convert a documentation claim into runtime verification.

---

## 6. CURRENT PROJECT PRINCIPLE

HipMarvinFX v7 is being built around a deterministic, evidence-driven architecture:

`External Source → Evidence Adapter → Validation → Evidence Store → Technical/Macro Engine → Evidence Packet → AI/Analyst → Research File → Parser → Publication`

The governing anti-fabrication rule is:

> **AI may interpret verified evidence, but AI must never manufacture missing market facts.**

No fabricated:

- price;
- OHLC value;
- trend;
- structural break;
- liquidity state;
- flow state;
- QMR phase;
- QM/QML pattern;
- entry;
- stop;
- target;
- R:R;
- macro fact

may become authoritative published data.

---

## 7. LOCKED v7 TRADING METHODOLOGY

Do not reopen these rules without an explicit new project decision.

Core hierarchy:

**Macro Regime → Weekly Thesis → HTF Trend/Structure → 20D Location → Liquidity → QMR → Reaction → QM/QML → Liquidity Target → Risk**

Locked principles include:

- trend first;
- Daily = broad structure/context;
- 4H = actionable trend/structure;
- 1H = minimum execution timeframe;
- no sub-1H execution;
- Daily/4H conflict = transition/conflict, not forced direction;
- Premium/Discount is location, not automatic direction;
- 20D location is location, not automatic direction;
- liquidity sweep alone does not prove reversal;
- QMR = Quality → Manipulation → Reaction;
- QMR continuation is a primary use case;
- countertrend requires the defined HTF structural-break gate;
- QM/QML is entry refinement, not the bias engine;
- never exclude a cross merely because it is a cross;
- never fade a trend merely because price is in Premium.

---

## 8. MARKET-DATA STATE AT HANDOFF

The September 1 shift handover recorded these items as completed/verified at that point:

- `yahoo-fx` adapter existed and conformed to the market-data provider interface;
- Yahoo daily OHLC was live-verified;
- invalid/in-progress candles with null close were rejected by candle validation;
- native Yahoo 4H bucket boundaries did not match the required canonical buckets;
- the intended approach is to derive canonical 4H structure from validated 1H data;
- XAU/XAG mappings were corrected to `GC=F` / `SI=F` and live-verified;
- daily/weekly cron provider order was changed to `[yahoo-fx, twelve-data]`;
- the buggy old `yahoo-finance` provider was removed from those provider lists;
- daily packet-variable shadowing was fixed;
- EvidencePacket/EvidenceItem parser exports were fixed;
- TypeScript typecheck was reported clean after those fixes.

These are **documented historical verification claims**. The next session must verify their current state in the application rather than assuming nothing has changed.

---

## 9. COT STATE HAS CHANGED

Do not use older documentation that describes COT as completely unimplemented without checking the current application.

At the current handoff, application commit `a82d8c2` is reported to contain:

- COT fallback;
- manual COT ingest endpoint.

Therefore the initial classification is:

> **COT = PARTIALLY IMPLEMENTED / VERIFY CURRENT CODE AND RUNTIME**

The next session must determine whether the COT path is now fully complete, partially complete, or still has integration gaps.

---

## 10. CURRENT OPEN WORK — VERIFY BEFORE CODING

The following are the principal known work areas. They form the working queue, but each must be reconciled against the current application before being declared OPEN or CLOSED.

### Priority 1 — HTF deterministic structure

Verify/implement:

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

The immediate historical engineering focus after market-data stabilization was **HTF structure**.

### Priority 2 — Trend alignment and structural-break gate

Implement/verify:

- Daily ↔ 4H trend alignment;
- protected HTF swing identification;
- meaningful structural-break detection;
- displacement/acceptance requirements;
- follow-through requirements.

Countertrend must not pass without the documented HTF gate.

### Priority 3 — QMR

Implement/verify deterministic:

**Quality → Manipulation → Reaction**

The engine must distinguish, where applicable:

- liquidity sweep;
- rejection;
- acceptance;
- continuation;
- sequential liquidity consumption;
- genuine reaction.

A sweep alone must not become a reversal signal.

### Priority 4 — QM/QML

Implement/verify QM/QML as entry refinement only.

It must not override HTF trend, structural, or countertrend rules.

### Priority 5 — Evidence → AI → Parser → Persistence

Prove a complete real-data vertical slice:

`real market data → validated evidence → deterministic technical state → evidence packet → constrained AI interpretation → parser validation → eligible publication/persistence`

The actual application wiring must be inspected. Do not assume the architecture diagram describes the current code.

### Priority 6 — Adversarial anti-fabrication testing

At minimum prove:

**Fabricated price:** rejected.

**Missing evidence:** cannot be filled by AI invention.

**Valid evidence:** valid evidence-backed output can proceed.

**Rejected output:** produces zero persistence/publication of rejected factual fields.

### Priority 7 — Parser/admin synchronization

Verify/complete where still necessary:

- Zone parser;
- Tier parser;
- Correlation parser;
- Week-Close Review parser;
- WEEK31 weekly-research reconciliation;
- parser v7 contract alignment;
- synchronization between parser and actual application schema.

### Priority 8 — Known legacy/open defects

Investigate each against current code:

- `lookupPairsForCycle()` stub;
- old `YahooFinanceAdapter` 4H→1H mislabeling;
- orphaned `exchangerate-host.ts`;
- `week_start` / `week_end` issue;
- weekly-cycle mislabeling;
- `research_cycles_week_unique` constraint issue;
- Position Ledger DB table, if still absent;
- Gemini/Groq provider issues only if the current architecture still requires them.

### Priority 9 — Macro evidence productionization

Do not claim full production macro infrastructure without proof for:

- COT;
- central-bank ingestion;
- economic-calendar ingestion;
- inflation data;
- news/RSS;
- macro drivers;
- macro regime engine;
- production Evidence Store/ledger.

The full Macro Engine was intentionally deferred until the evidence-to-AI vertical slice is proven unless a newer explicit decision changes that order.

### Priority 10 — Production cutover

v7 is not production-ready merely because components exist.

Publication cutover requires successful end-to-end evidence, validation, rejection, persistence, synchronization, and adversarial testing.

Protect the existing operational path until the v7 path is demonstrably safe.

---

## 11. REQUIRED TRADE/RESEARCH FIELD CONTRACT

Where the application produces a trade/research object, account for:

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

Schema completeness must never be achieved by inventing missing values.

Use explicit unavailable/pending/stale/invalid states where the contract permits them.

---

## 12. AI BOUNDARY

AI may:

- interpret verified evidence;
- explain relationships between verified fields;
- produce constrained narrative;
- produce analyst-facing reasoning from supplied facts.

AI may NOT independently create factual current-state values for:

- price;
- OHLC;
- HTF trend;
- protected swing;
- structural break;
- liquidity;
- flow;
- QMR;
- QM/QML;
- entry;
- stop;
- target;
- R:R;
- absent macro facts.

If evidence is unavailable, return the appropriate controlled state such as:

**NOT_AVAILABLE / PENDING / STALE / INVALID**

rather than a guess.

---

## 13. SUPERSEDED ARCHITECTURE — DO NOT RESTORE

The following are not current implementation directions:

### Shelved: restore the v6 prompt chain

Do not revive the old v6 prompt as a shortcut.

### Shelved: AI as factual market-data source

Do not use AI to invent/guess current prices or technical state.

### Superseded: old authored-research persistence as core v7 architecture

The current decision separates:

- **Track B:** deterministic engines compute/expose technical data;
- **Track A:** analyst-authored/parser-imported trade ideas remain the controlled persistence path where applicable.

Track B does not recreate the old workflow by authoring trade ideas, macro narrative, `trade_ideas`, or `research_cycles` merely to restore deprecated behavior.

### Corrected: Evidence Packet assumption

The September 1 application inspection found that `buildEvidencePacket()` was not simply an upstream AI gate. In the actual application it was being used as parallel/non-blocking verification around parsing.

Inspect actual code before modifying this boundary.

### Corrected: native Yahoo 4H canonicalization

Do not revert to native Yahoo 4H bars for the required canonical 4H buckets when derived 4H from validated 1H data is the chosen architecture.

---

## 14. PROVENANCE / HISTORY RULE

Do not silently delete or rewrite historical decisions merely because they were superseded.

Preserve history.

When a newer decision supersedes an older one:

- retain the old document;
- identify the newer controlling decision;
- do not treat the old instruction as current.

`DEVELOPMENT_DIRECTIVE_AUTOMATED_MACRO_EVIDENCE_V7.md` should currently be treated as a historical/proposal artifact unless a newer explicit decision reactivates it.

---

## 15. IMPLEMENTATION DISCIPLINE

For every new session and every new task:

1. Identify the actual application repository.
2. Confirm branch and commit.
3. Inspect current implementation.
4. Locate existing files/functions before creating replacements.
5. Reconcile documentation against code.
6. Classify the task: CLOSED / OPEN / PARTIALLY IMPLEMENTED / SUPERSEDED / UNKNOWN.
7. Identify dependencies.
8. Make the smallest architecture-consistent change.
9. Run relevant typechecks/tests.
10. Verify failure/rejection behavior where applicable.
11. Record exact files changed.
12. Commit only after verification.
13. Push only after verification.
14. Update the appropriate handover/state documentation when project state materially changes.

Never claim implementation merely because a specification exists.

Never claim a feature is absent merely because an old document says it was absent.

---

## 16. DEFINITION OF DONE

A feature is NOT DONE because:

- Markdown exists;
- code compiles;
- a function exists;
- a route exists;
- an AI answer looks plausible;
- a parser accepts a field;
- a mock dataset passes;
- a commit exists;
- a developer says it works.

For factual/market-data features, appropriate evidence must demonstrate the relevant chain:

`real source → adapter → validation → deterministic state → constrained interpretation → parser validation → persistence/publication`

including rejection behavior for fabricated, invalid, stale, or missing evidence.

---

## 17. REQUIRED FIRST DELIVERABLE OF EVERY NEW SESSION

Before modifying code, produce a concise:

# CURRENT REALITY vs REQUIRED v7 MATRIX

At minimum include:

| Area | Documented Status | Actual Code Status | Runtime Verified? | Tests | Gap | Next Action |
|---|---|---|---|---|---|---|
| Market data | — | — | — | — | — | — |
| Yahoo FX | — | — | — | — | — | — |
| TwelveData fallback | — | — | — | — | — | — |
| XAU/XAG | — | — | — | — | — | — |
| COT | — | — | — | — | — | — |
| Evidence ingestion | — | — | — | — | — | — |
| Evidence validation | — | — | — | — | — | — |
| Evidence Packet | — | — | — | — | — | — |
| Daily OHLC | — | — | — | — | — | — |
| 4H OHLC | — | — | — | — | — | — |
| 1H OHLC | — | — | — | — | — | — |
| HTF structure | — | — | — | — | — | — |
| Trend alignment | — | — | — | — | — | — |
| Protected swing | — | — | — | — | — | — |
| Structural break | — | — | — | — | — | — |
| Liquidity | — | — | — | — | — | — |
| Flow | — | — | — | — | — | — |
| QMR | — | — | — | — | — | — |
| QM/QML | — | — | — | — | — | — |
| AI boundary | — | — | — | — | — | — |
| Parser | — | — | — | — | — | — |
| Persistence | — | — | — | — | — | — |
| Publication | — | — | — | — | — | — |
| Macro evidence | — | — | — | — | — | — |
| Position Ledger | — | — | — | — | — | — |
| Legacy defects | — | — | — | — | — | — |

Then identify the **highest-priority verified blocker**.

Only after that should implementation begin.

---

## 18. FINAL CONTINUITY COMMAND

Every new developer/AI session should begin from this exact principle:

> **“I am continuing HipMarvinFX, not restarting it. I will read this canonical directive, inspect the actual application repository and current runtime state, reconcile CURRENT REALITY vs REQUIRED, preserve all verified decisions, ignore superseded architecture, and only then implement the highest-priority verified open item.”**

The objective is not to make the repository appear complete.

The objective is to make HipMarvinFX:

**deterministic, evidence-backed, anti-fabrication, testable, auditable, and publication-safe.**

---

**END — CANONICAL NEW-SESSION STARTING DIRECTIVE**
