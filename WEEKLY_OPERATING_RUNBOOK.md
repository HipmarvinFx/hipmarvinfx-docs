# HipMarvinFX - Weekly Operating Runbook (owner view)

Status: operational template. Supplements WEEKLY_DAILY_WEEK_END_WORKFLOW_DIRECTIVE.md and STANDING_PROTOCOL_v7.md; overrides neither. These are process rules, not code gates.

## Principles
1. Two lanes. The publish lane produces and imports this week's research. The engineering lane is anything that changes code, schema, adapters, parsers or tests. Engineering never blocks a publish; it happens on a branch after the publish lane closes.
2. Pending is a valid answer. Missing evidence is written PENDING or NOT_AVAILABLE (Rule 1) and the week ships. Investigate the gap after publishing, not before.
3. Live data is its own as-of. Draft on Sunday from Friday's close, so no as-of replay and no fixtures. As-of tooling is only for backfilling a past week and is never a weekly deliverable.
4. One scratch folder per week (weeks\W<NN>\, gitignored). Nothing from a weekly run is left in the repo root.
5. Check state before any import retry (Directive s7). One snapshot per import, not per attempt.

## Time boxes (defaults, adjust deliberately)
| Phase | Budget |
|---|---|
| Sunday outlook | 90 min |
| Each event day | 15 min |
| Week-end review | 45 min |
| Any single verification | 30 min, then mark PENDING and move on |

## Sunday - Weekly Outlook
Owner provides (max 2):
1. Screenshot of the coming week's Forex Factory calendar (forecast and prior), only if the preflight shows no calendar evidence for the week.
2. Optional standing instruction (pairs to skip, "no trades this week").
Assistant: preflight, evidence inventory (VERIFIED/PENDING/STALE/INVALID/NOT_AVAILABLE), draft against Protocol v7 and the current template, validate, hand over the file.
Owner: read the draft, approve, import in /admin, confirm counts (ideas, events, scenarios, COT rows, game plan).
Done when: cycle active, counts match, /outlook renders.

## Event day - Daily Update
Owner provides: one calendar screenshot after the release (actuals), only on days with red-folder events. No chart screenshots; price comes from H1-derived closes.
No-event days: no mid-week entry. Backfill them in one batch at week-end (the calendar sources "none scheduled").
Assistant: Rule 13 sanity check for unscheduled moves; update only affected fields (Rule 9).
Done when: the day row is imported, Closed (Rule 8) or Partial with what is outstanding.

## Saturday - Week-End Review
Owner provides: confirmation of any manual position closes. Nothing else.
Assistant: re-read the week and the ledger (Rule 14), results, carryover, lessons; update the ledger.
Done when: review imported to the selected cycle, ledger updated, cycle closed.

## Stop rules - send to the engineering lane
- A new adapter, parser, schema change or test harness discovered mid-week.
- A verification that would exceed 30 minutes.
- A second import attempt without a state check.
- Re-deriving a past week's data as-of.

## Never relaxed
Rules 1-4, Rule 8 (no Closed without sourced events), no fabricated figures, no import or publish without owner approval.

## Checklist
- [ ] Preflight run, ledger state known
- [ ] Calendar evidence present or screenshot supplied
- [ ] Draft validated against v7 and template
- [ ] Owner approved; snapshot taken
- [ ] Import done; counts compared
- [ ] Page renders as admin
- [ ] Scratch moved to weeks\W<NN>\