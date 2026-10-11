# HipMarvinFX — Weekly, Daily, and Week-End Workflow Implementation Directive

**Status:** Operational directive  
**Scope:** Research preparation, evidence retrieval, daily updates, week-end review, and research-cycle import  
**Authority:** Supplements Standing Protocol v7 and existing templates. It does not override them. If requirements conflict, report the exact conflict rather than silently changing either.

## 1. Objective

Deliver the weekly research outlook, its daily updates, and the week-end review using evidence and automation already available in HipMarvinFX. Do not turn routine verification into a request for the owner to manually collect data.

**Default rule: inspect first, retrieve through existing authorized capabilities, validate, draft, and request owner input only for a demonstrated gap or a decision/approval only the owner can provide.**

## 2. Before every run: establish task state

1. Identify the requested instrument set, reporting period, timezone, template version, publication target, and whether this is a new cycle, daily update, or week-end close.
2. Read the current canonical protocol, applicable template, previous cycle/update, and handover/open-item notes.
3. Inspect relevant repository code, configured adapters, pipeline call sites, schemas, evidence records, generated artifacts, and tests.
4. Check whether this task or an earlier attempt already wrote records. Do not retry a multi-write workflow until its current state is known.
5. State the intended read/write boundary. Use read-only inspection and dry runs first. Production ingestion, imports, cleanup, publication, and deployment require applicable authorization.

Do not restart from a generic checklist when prior work has established facts or closed questions.

## 3. Weekly research workflow

For each new weekly cycle:

1. **Resolve the cycle window.** Establish exact start/end dates and timezone from the project convention and requested week. Do not guess or silently change the period.
2. **Audit and retrieve evidence through existing paths first.**
   - Price: inspect the configured Yahoo FX adapter and any configured fallback; verify pair/timeframe coverage, timestamps, closed candles, freshness, OHLC validity, and the specified historical range anchor.
   - COT: inspect the CFTC adapter and fallback; verify report date, instrument coverage, parsing, provenance, persistence, status, and consumer compatibility.
   - Calendar: inspect the configured calendar adapter and pipeline output; verify requested-window coverage, event timestamps, currencies, impact fields, provenance, duplicates, and source suitability.
   - Macro, central-bank, and other evidence: use approved adapters and evidence stores, applying provenance, effective-time, freshness, and status checks.
3. **Do not equate implementation with success.** An adapter existing or being called does not prove retrieval succeeded, records were persisted, or evidence passed validation.
4. **Build an evidence inventory** for the requested period and instruments. Use the existing evidence statuses VERIFIED, PENDING, STALE, INVALID, and NOT_AVAILABLE; preserve FACT, DERIVED, and INTERPRETATION distinctions.
5. **Draft from validated evidence.** Complete supportable sections. Mark unsupported fields using the approved template convention. Never fabricate prices, closes, ranges, COT figures, actuals, forecasts, structure, liquidity, catalysts, entries, stops, targets, risk multiples, confirmations, or outcomes.
6. **Apply methodology gates.** The weekly thesis, HTF structure, 20-day location, liquidity, QMR/QM execution, trade eligibility, risk, and correlation rules in Standing Protocol v7 remain binding. Missing evidence is not permission to relax a gate.
7. **Validate the draft** against the current template, parser rules, required fields, source/provenance requirements, and relevant tests before import or publication.
8. **Keep drafting separate from importing.** A draft can progress while an evidence gap is investigated. Do not call it complete or import-ready until applicable validation gates pass.

### Source-specific input requests

A request for a screenshot, chart, report, or manual value is permitted only after the agent records:
- the exact template or specification requirement;
- which existing adapter, source, evidence record, and alternative path were checked;
- the actual result/error and the time window/instruments checked;
- why automated evidence cannot satisfy that requirement; and
- the smallest owner input that resolves the gap.

Do not assume an adapter is equivalent to a named source when the template explicitly requires that source. First determine whether the source requirement is mandatory or an approved alternative is allowed. For example, FinanceCalendar must not be silently represented as ForexFactory. Equally, do not request a ForexFactory screenshot by default if the governing requirement can be satisfied by an approved, validated feed.

## 4. Daily update workflow

A daily update modifies the current weekly research cycle; it is not a fresh weekly outlook.

1. Load the active cycle, latest daily update, current relevant evidence, open scenarios/ideas, and previous recorded verdicts.
2. Retrieve new event and market evidence from approved adapters and stores. Verify event time, actual release values, candle closure, capture/effective time, and provenance.
3. Compare new evidence with the weekly thesis and scenario conditions. State whether it confirms, challenges, invalidates, or leaves the thesis unchanged, with actual evidence supporting the conclusion.
4. Update only fields affected by newly verified evidence. Preserve historical records and prior verdicts; do not silently rewrite history.
5. Re-evaluate HTF structure, liquidity/flow regime, QMR/QM gates, risk, and trade eligibility only where relevant evidence warrants it. A sweep alone is not a confirmed reversal; 20-day location alone is not direction.
6. If an event has not occurred or its actual is unavailable, state that plainly. Do not invent an actual or infer confirmation from narrative.
7. Append/update the daily record under the existing schema and idempotency rules. Before writing, inspect whether the same date/event has already been recorded.
8. Validate the update and report what changed, what did not change, and what remains pending.

## 5. Week-end workflow

Close the reporting period using recorded evidence—not memory or retrospective assumptions.

1. Establish the exact week being closed and load its weekly thesis, daily updates, scenarios, trade ideas, event records, COT/macro evidence, and available outcome evidence.
2. Reconcile scheduled events with sourced actuals and actual market response. Missing actuals or price-response evidence remain pending/unavailable; do not manufacture resolution.
3. Assess each scenario and idea against predefined conditions using recorded evidence. Preserve distinctions between confirmed, invalidated, mixed, pending, and unavailable.
4. Produce the Week Close Review using the currently approved parser/template. Do not revive retired extraction logic if a newer parser is canonical.
5. Carry forward only supported lessons and unresolved conditions into the next cycle. If the previous week's review is absent, explicitly mark it unavailable; do not fabricate a carryover.
6. Validate date boundaries, references, required fields, and links between the closed week and next cycle.
7. Only after review and validation, perform any authorized persistence/publication step. Inspect current records first and avoid blind retries.

## 6. Required blocker classification

Every task report must distinguish:
- **UNVERIFIED:** not yet checked; continue investigation.
- **VERIFICATION_FAILED:** a check was attempted and failed; report the exact error and assess safe alternatives.
- **CAPABILITY_GAP:** no available implementation/source can meet a documented requirement.
- **EVIDENCE_GAP:** the capability exists, but required data is missing, stale, invalid, incomplete, or insufficiently sourced.
- **OWNER_DECISION_OR_APPROVAL:** a genuine choice, permission, credential, or approval only the owner can provide.

Use VERIFIED, PENDING, STALE, INVALID, and NOT_AVAILABLE for evidence state. Never describe unverified evidence as unavailable.

## 7. Persistence and recovery safeguards

The research-cycle importer may perform multiple writes and may not roll them back as a single transaction.

- Inspect the result of every prior attempt before retrying.
- Identify inserts, updates, upserts, uniqueness constraints, and idempotency behavior in the actual routes.
- Do not assume duplicate protection without verifying it.
- Do not blindly retry, manually delete records, or claim rollback without evidence.
- If partial writes are found, produce a record-by-record recovery plan before cleanup.
- No production write, cleanup, ingestion, import, or publication without applicable authorization.

## 8. Standard completion report

Every weekly, daily, or week-end task must end with:
- **Deliverable and period**
- **Sources/adapters and code paths inspected**
- **Evidence retrieved and validation results**
- **Changes made and artifacts produced**
- **Tests/checks run and results**
- **Unverified items and exact blockers**
- **Persistence/write state and approval status**
- **Owner input required:** NONE unless a specific, demonstrated need exists
- **Next safe action**

Do not claim readiness based only on code existence, adapter invocation, a successful isolated test, or a successful partial write.

## 9. Stop the recurring failure pattern

Before asking the owner for any file, screenshot, chart, report, value, or clarification, answer:
1. What exact requirement remains unmet?
2. What existing sources, adapters, records, artifacts, and authorized tools were checked?
3. What did those checks actually return?
4. Why can the existing system not meet the requirement?
5. What is the smallest owner input needed, and what work can continue without it?

If these questions cannot be answered, the request is premature. Continue the investigation or report the precise technical limitation. Do not transfer routine system verification to the owner.

This directive is an operational instruction, not proof that the process is technically enforced. Any proposed code-level gates, tests, or automation must be implemented and validated separately before being described as enforced.
