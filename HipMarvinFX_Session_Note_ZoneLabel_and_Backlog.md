# HipMarvinFX — Session Note: Zone-Label Investigation + Backlog State

**Session focus:** verifying open items from `HANDOVER_NOTE_updated.md` against
real code, one grep/build at a time, rather than trusting prior claims.

---

## CLOSED THIS SESSION (verified end-to-end, build-confirmed)

### 1. `trade_ideas.tier` schema — CLOSED
Confirmed live in the production schema (`integer`, nullable) via direct
schema-dump inspection. Actively read by `lib/derivation/tradeRatingsAdapter.ts`
(`.select('id, pair, direction, tier, correlation_class, zone_position_pct,
thesis_aligned')`) and used as a real decision input in
`lib/derivation/tradeRatings.ts`'s Strong/Weak/Not-Rated derivation logic
(`tier === 1` / `tier === 2` gates real output). Not a dead or orphaned column.
The original handover's uncertainty ("no matching migration SQL shown for
`tier`") is resolved — the column exists and is load-bearing, whether or not
the exact `ALTER TABLE` statement that added it was ever pasted into a note.

### 2. Week-End Review parser — CLOSED
- `app/lib/parseWeekCloseReview.ts` targets the real document structure
  (`## WEEK N RESULTS`, `## CARRYOVER RESOLVED THIS WEEK`,
  `## HONEST WEEK SUMMARY`, `## LESSONS`), confirmed via its own test suite,
  including a test explicitly titled "does not treat legacy B1-B6 headings
  as the current format."
- `app/admin/page.tsx` has the old `parseWeekCloseReviewBlock()` explicitly
  marked RETIRED, with a comment pointing to the real function. The live
  import path (`importWeekCloseReviewBlock`) calls the new parser correctly.
- POST `/api/week-close-review` is a schema-agnostic pass-through (`insert(body)`
  / `update(body)`) — confirmed to hardcode no field names, so it correctly
  writes whatever `WeekCloseReviewPayload` shape the parser produces
  (`cycle_id`, `honest_week_summary`, `lessons[]`, `biggest_error`).
- GET `/api/week-close-review` is the same schema-agnostic pass-through
  (`select('*')`) — also confirmed clean.

**New finding, not previously flagged anywhere:** the *display* page,
`app/review/weekly/[cycle_id]/page.tsx`'s `CloseReviewTab`, was still reading
the old, retired field names (`net_pips`, `fill_rate`, `filled`, `no_fill`,
`wins`, `setups_flagged`, `best_trade_pips`, `avg_rr_planned`,
`week_in_review`, `weekly_note`, `best_trade_analysis`, `trade_to_learn`,
`personal_commentary`, `rr_commentary`) — none of which exist on the real
`WeekCloseReview` type, and never rendering the three fields that actually do
(`honest_week_summary`, `lessons`, `biggest_error`). No crash occurred because
every stale field read was optional-chained or nullish-coalesced — it simply
rendered nothing for the fields that matter, silently.

**Fixed this session:**
- `CloseReviewTab` rewritten to render `honest_week_summary`, `lessons`
  (as a bulleted list), and `biggest_error` — the three real fields.
- Top-of-file schema-note comment (previously describing the old fields)
  corrected to match.
- Verified via `npm run build` — clean compile, zero regressions, all
  routes (including `/review/weekly/[cycle_id]`) generated successfully.
- `.bak` and `.bak2` backups of the pre-edit file retained locally.

---

### 3. ABC / B-zone validator investigation — CLOSED

**No current reader-facing bug found.**

Two non-interoperable zone-label implementations exist in the codebase:

- **`lib/engine/twenty-day-location.ts` / `lib/engine/dealing-range.ts`**
  — a 7-band internal vocabulary (`Deep Discount`, `Discount`, `Lower
  Equilibrium`, `Equilibrium`, `Mild Premium`, `Premium`, `Deep Premium`,
  with extra threshold cuts at 33%/67%). `dealing-range.ts`'s own header
  comment explicitly acknowledges this diverges from Rule 21 by design
  ("a design decision, not a directive requirement"). This scheme is
  **live** — `calculate20DLocation()` is imported directly by
  `lib/pipeline/weekly.ts`, and `calculateDealingRange()`'s output feeds
  `classifyAbcFormationV2()` in both `daily.ts` and `weekly.ts` via
  `composite.dealingRange`. However, only `.drh` / `.drl` are extracted
  from that object into the pipeline's saved output — the `.zone` field
  itself (the 7-band label) is never forwarded to any research file, AI
  prompt, or published output. It's real, computed, and consumed
  internally by ABC formation logic, but not currently reader-facing.

- **`app/lib/types_v6.ts`'s `zoneLabel()`** — a 5-band function
  (`Discount` 0–20, `Lower Equilibrium` 20–40, `Equilibrium` 40–60,
  `Mild Premium` 60–80, `Deep Premium` 80–100) that matches
  `STANDING_PROTOCOL_v5.md` Rule 21 Part A exactly, threshold-for-threshold
  and label-for-label. Confirmed via full-codebase search: **zero callers**.
  Correctly written, entirely unused.

- **`app/page.tsx`**'s zone labels (`"Mild Premium"`, `"Near Discount"`,
  `"Equilibrium"`, `"Deep Premium"`) are hardcoded string literals inside
  static landing-page mock arrays (`PAIRS`, `PAIR_DETAILS`) — confirmed via
  the file's own imports (only `/api/live-prices` is fetched; zone data is
  never fetched). Not derived from either engine. The presence of
  `"Near Discount"` — a label matching neither scheme — further confirms
  this is placeholder marketing copy, not real derived output.

**The Rule 21B soft-flag conflict check itself is correct and unaffected**
by any of the above — `app/admin/page.tsx`'s `rule_21b_flag` logic
independently thresholds at 60%/40% (Buy flagged above 60%, Sell flagged
below 40%), matching Rule 21 Part B regardless of which label scheme is
in play elsewhere.

**Action for the future (not now):** When Rule 21's Daily Zone field is
eventually wired into any reader-facing output — a Weekly Outlook display,
the Trade Priority List, or the Dashboard/Trade Page derivation logic
described in `PUBLISHING_PIPELINE.md` — **do not expose the 7-band internal
`.zone` value from `twenty-day-location.ts` / `dealing-range.ts` as the
Rule-21 label.** Either wire up the existing `zoneLabel()` function in
`types_v6.ts`, or write an explicitly reconciled replacement. This
discrepancy should not be treated as implementation work to pick up now —
there is no current reader-facing requirement it's blocking — but it is a
real landmine if someone reaches for the nearest-looking function
(the engine's) without knowing a second, correct one already exists.

---

## DISPUTED / UNCONFIRMED — flagged before writing into the record

The following were proposed for this note's backlog but conflict with, or
fall outside, what this session actually verified. Left out of the
closed/open lists above pending confirmation:

- **Week-End Review parser** and **`trade_ideas.tier` schema** were
  proposed as still-open items. Per this session's own verification above,
  both are closed. If a different, narrower gap under either name is still
  actually open, it needs to be named specifically so it can be checked —
  it isn't the same thing this session tested.
- **GBPUSD COT visual regression** and **USDCHF COT visual regression** —
  not discussed or investigated in this session at all. No basis to
  confirm or deny status; needs its own investigation thread before being
  carried forward as a known item.

---

## BACKLOG STATE (as verified through this session only)

**Closed, verified this session:**
1. `trade_ideas.tier` schema
2. Week-End Review parser (including the newly-found + fixed display bug)
3. ABC / B-zone validator investigation

**Open, not yet touched this session:**
- Buy/Sell badge color bug (stalled — original repro location removed from
  the Trade Priority Tab; needs a fresh sighting/repro before it can be
  worked)
- Stage 2 generator status (unchecked this session)
- Pipeline timing / Vercel 60s (unverified since last noted check)
- Macro engine implementation work (per v7 handover docs — Phase 6, not
  yet built)
- Any items from prior sessions not re-verified here (COT pair
  decomposition, COT missing-data handling, trader-facing language audit,
  `/trades` dead-table fix, v7 QMR/HTF Steps 3–14 — carried forward from
  earlier claims, not re-checked in this thread)

**Explicitly deferred, not a bug:**
- Zone-label scheme reconciliation (`twenty-day-location.ts`/`dealing-range.ts`
  vs. `types_v6.ts`'s `zoneLabel()`) — do not reopen as implementation work
  until an actual reader-facing Daily Zone requirement exists to wire it into.

---

*End of session note.*
