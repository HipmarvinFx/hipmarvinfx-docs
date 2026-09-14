# QMR/ABC — EXPERIMENTAL, NOT IMPLEMENTED

Status as of: this reconciliation discussion, prior to Test Case #1
Applies to: any material referencing QMR, QMR_STRUCTURE, QMR_REVERSAL_APPLICATION,
QM/QML Continuation, ABC formations, Structural Failure, or related state-machine
concepts developed outside the HipMarvinFX repository.

---

## Current classification

QMR/ABC is **experimental candidate research**, not part of HipMarvinFX.

It is a candidate definition for an already-reserved, currently undefined slot:
**Phase 3 (TAFISE)** and the **`technical_verdicts`** table (design-only, no rows,
no parser, no schema fields approved).

It is explicitly **not**:
- an implemented feature
- a frozen specification
- a replacement for or extension of TAFISE (undecided)
- a second correlation/portfolio-risk mechanism (rejected — see below)

---

## What has been decided

1. **No implementation work proceeds** — no parser code, no schema, no
   `qmr_verdicts`/`abc_verdicts` tables — until QMR/ABC passes a falsification
   test (see below).
2. **Single correlation layer only.** If QMR/ABC is ever adopted, it does not
   introduce its own portfolio/correlation-cap logic. Correlation and sizing
   decisions remain the job of `correlation_class` + Rule 5. QMR's job, if
   adopted, is limited to setup validity — not "should we take this exposure."
3. **`technical_verdicts` is the only destination.** No parallel schema. If
   QMR/ABC ever produces implementable output, it becomes the definition of
   that table's columns — not a new table alongside it.
4. **Rule 2 (sourcing separation) governs every eventual QMR output.** Any
   QMR-derived state (e.g. "structural failure confirmed") must be labeled as
   an **inferred technical read**, never presented as a sourced fact the way
   an FF actual or chart price is. "Confirmed" means confirmed by the defined
   technical rule, not externally verified.
5. **The TAFISE ↔ QMR/ABC relationship is unresolved**, and no outcome is
   preferred. Three hypotheses remain open with no default:
   - **A** — QMR/ABC becomes the definition of TAFISE
   - **B** — TAFISE is defined independently; QMR/ABC remains research
   - **C** — selected QMR/ABC components are adopted into TAFISE, rest rejected

   No dataset review or rule-drafting should proceed as if any of A/B/C is
   the likely answer.

---

## What happens next: falsification test, not a stress test

QMR/ABC will be evaluated via a **pre-registered falsification protocol**
against a fixed historical dataset — not iteratively refined against messy
examples until it fits.

**Ground rules:**
- The dataset must be locked *before* review — captured or sourced in a way
  that guarantees no hindsight knowledge of how the week resolved. A week
  recalled from memory, or a week whose outcome is already known to whoever
  applies the rules, does not qualify as locked data.
- For each test case, output must be one of: `VALID / INVALID /
  NOT_DETERMINABLE / NOT_APPLICABLE` — no silent interpretation.
- If a test case exposes a missing or broken rule: **stop, record the
  failure, do not modify QMR mid-test.** Finish the full predefined test set
  first. Only after the primary evaluation is complete may a rule change be
  proposed — and it must be re-tested on a fresh dataset, not the one that
  exposed the failure.
- Evaluation dimensions: determinacy, completeness, consistency,
  non-retroactivity, stability, invalidation, coverage, exception rate.
- **Known limitation to disclose alongside any result:** if the same person
  both authored QMR's rules and applies them to the test cases, the
  Consistency dimension cannot be meaningfully tested. This should be stated
  explicitly in the test write-up, not assumed away.

**Outcomes:**
- **Hard fail** — systemic non-determinism (same inputs, rules applied
  correctly, two reasonable readings produce different structural verdicts,
  repeatedly). This indicates an architecture problem, not an edge case.
- **Conditional fail** — works, but repeatedly requires undocumented
  discretion, ambiguous A/B/C selection, or retrospective interpretation.
  Stays classified as research.
- **Pass** — deterministic, reproducible, non-retroactive, explicit
  invalidation/staleness handling, no dependence on undocumented discretion.
  Earns a separate implementation-readiness review — not automatic adoption.

Only after a pass does TAFISE/`technical_verdicts` integration design begin.

---

## Next action

Run QMR/ABC Test Case #1 against a locked historical dataset. Before that
can start, two things need to be pinned down:
- which week/dataset qualifies as genuinely locked (not recalled from memory,
  not reviewed with foreknowledge of the outcome)
- who applies the rules, and whether a single-analyst test is accepted as a
  known limitation for this round
