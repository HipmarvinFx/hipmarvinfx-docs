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

## RECONCILIATION — resolved after cross-checking against this session's own evidence

Two items were initially proposed for this note's backlog in a form that
conflicted with what this session had just verified. Both are now resolved:

- **Week-End Review parser** and **`trade_ideas.tier` schema** were
  initially proposed as still-open. Per this session's direct verification
  (parser + regression tests + admin wiring + downstream display fix +
  clean build, for the first; live schema confirmation + real read/write
  usage in derivation logic, for the second), both are confirmed **CLOSED**
  and recorded as such above. A later verified build/test result supersedes
  an earlier backlog listing — the backlog does not get to overrule the
  evidence just by having been written first.

- **GBPUSD COT visual regression** and **USDCHF COT visual regression**
  were proposed as open items, then withdrawn on review. The supporting
  evidence available establishes: EURUSD's COT presentation was verified
  end-to-end; the seven-pair currency decomposition map includes GBPUSD and
  USDCHF as entries; the missing-COT-data status handling was patched and
  type-checked; EURUSD's technical-detail presentation was visually
  confirmed. None of that constitutes visual verification of GBPUSD or
  USDCHF specifically — being present in a decomposition map is not the
  same as having been rendered and checked. Correctly withdrawn rather than
  carried forward as if verified. (Note: this specific evidence trail — the
  EURUSD verification, the decomposition map, the missing-status patch —
  was not independently re-checked within this session's own transcript;
  it's recorded here per the standing project record, on the same basis
  every other unverified claim in this note is treated: named explicitly,
  not silently trusted.)

**Governing principle, worth stating plainly for future sessions:**
Do not resurrect an old handover item merely because it appears in an
earlier backlog. A later, independently verified build/test/result
supersedes it. An item's presence in a prior note is not evidence of its
current status — only a fresh check is.

---

### 4. Buy/Sell badge color bug — CLOSED (not reproducible)

Original claim (`HANDOVER_NOTE_updated.md`, `HipMarvinFX_Roadmap_Rev7_1.md`):
Buy/Sell badges on `/outlook/weekly/[cycle_id]` render the same color for
both directions. Original repro location (Trade Priority Tab) was already
removed by the time this session investigated.

**Every direction-based color implementation found in the current codebase
is correctly differentiated.** Checked exhaustively across:

- `app/outlook/weekly/[cycle_id]/page.tsx` — `TradeIdeasTab` (line ~184) and
  the COT positioning table (line ~373): both use case-insensitive matching
  (`String(r.direction).toLowerCase() === 'buy' || ... === 'long'`) and
  correctly split emerald (buy/long) vs. rose (sell/short).
- `app/trades/page.tsx` (line ~163): correctly splits green
  (`rgba(34,197,94,...)`) vs. red (`rgba(239,68,68,...)`) via
  `['buy','long'].includes(...)`.
- `app/live-trades/page.tsx` (line ~274): same correct pattern, green vs.
  red via `['BUY','LONG'].includes(...)`.
- `app/reports/[slug]/page.tsx` (line ~1116): correctly splits green
  (`#4ADE80`) vs. red (`#F87171`) by `idea.direction === 'Sell'`.

No shared `Badge` component conflates direction with an unrelated field —
`admin/page.tsx`'s `statusBadge`/`badgeFor` key off trade *status*
(Waiting/Triggered/Win/Loss/Confirmed/Invalidated), not direction.
`app/page.tsx`'s `ZoneBadge`/`FillBadge` key off zone label and fill
confidence, not direction. The scenario-status badge on the outlook page
(`stateStyles[r.state]`) keys off confirmation state
(near_confirmation/watch/ineligible), not direction — a different feature
entirely, not a variant of the reported bug.

**Conclusion:** not currently reproducible anywhere in the app. Most likely
resolved as an incidental side effect of other work on these same files
(e.g. the `/trades` data-integrity fix), without the fix ever being logged
against this specific item — the same pattern already seen this session
with the `tier` column and the Week-End Review parser: a backlog note
described a real state accurately at the time it was written, and has
since gone stale as the underlying code moved on. If the symptom
resurfaces, it needs a fresh screenshot and URL to reopen against — there
is currently no reproducible case to fix.

---

## BACKLOG STATE (corrected, evidence-based)

**Closed:**
1. `trade_ideas.tier` schema — verified this session
2. Week-End Review parser (including the newly-found + fixed display bug)
   — verified this session
3. ABC / B-zone validator investigation — verified this session
4. Buy/Sell badge color bug — verified this session, not reproducible

**Active backlog, carried forward:**
1. **Pipeline timing verification** — current implementation needs
   measurement before any architectural decision is made on it.
2. **Stage 2 generator** — still a declared implementation item, not
   superseded by any work done this session.
3. **Macro engine + ABC B-zone validator/invalidator implementation work**
   — current architectural work identified after the older, now-superseded
   QMR/HTF Steps 3–14 backlog.
4. **Rule-21 zone-label landmine** — investigation closed (see above);
   retained here only as a future implementation warning, not an active bug.
   Do not reopen as implementation work until an actual reader-facing Daily
   Zone requirement exists to wire it into.

**Removed from the verified backlog (insufficient evidence to carry forward):**
- GBPUSD COT visual regression
- USDCHF COT visual regression

These may be reintroduced later as **deliberate, explicitly-scoped future
regression coverage** for the other pairs in the seven-pair decomposition
map — but not as if they were already-known outstanding failures, since no
verification of that kind has actually been performed on those two pairs.

---

*End of session note.*