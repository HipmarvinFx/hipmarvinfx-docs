# HipMarvinFX — Session Handover: 14 September 2026

**Status:** Session record — security fixes verified and deployed; several
items flagged for next session, not yet resolved.

**Important process note, stated up front:** this session did not begin by
following `ONBOARDING_START_HERE.md`'s required sequence — it started from
a security audit request and only discovered the onboarding process, the
Order of Authority, and `HipMarvinFX_Implementation_Directive.md` partway
through. Every fix below was individually build-tested and verified live
in production, but nobody has yet checked this session's app-repo changes
against the current `STANDING_PROTOCOL_*` or the Implementation Directive.
**Recommended first action for whoever picks this up: do that check before
treating anything below as fully settled**, per this repo's own stated
discipline (`ONBOARDING_START_HERE.md` §11 — "the objective is
continuation, not reconstruction by assumption").

---

## 1. Shipped and verified — production, real accounts, end-to-end

All of the following were fixed in the actual application repository
(`hipmarvinfx`, not this docs repo), built locally, diffed, committed,
pushed, and re-tested live on `hipmarvinfx.com` after deploy — including
with a genuinely locked (non-subscriber) test account, not just assumed
correct from code inspection.

### API authorization fixes

Several `GET` routes were found gated behind `requireAdmin` (or, in one
case, no auth check at all) when they should have been gated behind
`requireSubscriber` or a lighter logged-in-only check. This was a
repeating pattern across the API, not one isolated bug — six separate
routes had the same mistake:

| Route | Was | Now |
|---|---|---|
| `GET /api/trade-results` | fully unauthenticated | `requireSubscriber` |
| `GET /api/trade-ideas` | fully unauthenticated | `requireSubscriber` |
| `GET /api/trade-ideas/[id]` | fully unauthenticated, then briefly `requireSubscriber` | `requireUser` (any logged-in user — needed so the detail page can render its blurred preview rather than 403 the whole record) |
| `GET /api/research-cycles` | `requireAdmin` (wrong) | `requireSubscriber`, plus non-admins only see `status = 'published'` rows |
| `GET /api/research-cycles/[id]` | `requireAdmin` (wrong) | Same pattern as above |
| `GET /api/live-prices` | `requireAdmin` (wrong) | `requireSubscriber` |

A new helper, `app/lib/requireUser.ts`, was added — checks only that a
user is logged in, no tier check. Used where a page needs to render for
any authenticated user and apply its own visual gating (blur), rather
than the API blocking the whole record.

**`app/api/trade-results/[id]/route.ts` has no `GET` handler at all** —
only `PATCH`/`DELETE`, both correctly admin-gated. Confirmed intentional
(nothing in the app calls a singular `trade-results` GET).

**Checked and confirmed correctly admin-only, left unchanged:**
`calendar-events/[id]`, `scenario-matrix/[id]`, `derived-publications/[id]`,
`weekly-lessons/[id]` — all called exclusively from `app/admin/page.tsx`.

**Checked and confirmed unused/dead, left unchanged:** `reports/[id]`,
`weekly-trade-log/[id]` — no call sites found in `.ts` or `.tsx` files.

### Client-side crash fixes

Two pages crashed with `TypeError` when the API correctly returned a
401/403 to a non-subscriber — because the fetch handlers parsed the
response body without checking `response.ok` first, so an error object
got treated as an array and `.map()`/`.filter()` threw.

- `app/live-trades/[id]/page.tsx` — guarded `trade-results` and
  `research-cycles` fetches with `.ok` checks before `.json()`.
- `app/trades/page.tsx` — same class of bug on `trade-results`; guarded
  and also fixed to distinguish "access denied" from "genuinely zero
  trades" (previously showed the misleading "No trades logged yet" to
  locked users instead of an upgrade prompt).

### UI/UX consistency fixes

- `app/live-trades/page.tsx` — trade cards were non-clickable `<div>`s;
  wrapped in `<Link>` so they navigate to the detail page. Also added
  `requireSubscriberPage()` gating with blur + upgrade banner for
  non-subscribers, matching the detail page's pattern.
- `app/components/layout/Sidebar.tsx` — added subscription-aware
  gating (`GATED_HREFS`) with 🔒 icons and `/upgrade` redirect for
  `/live-trades`, `/reports`, and (added tonight) `/trades`.
- `app/complete-profile/page.tsx` — changed `.update()` to `.upsert()`
  so onboarding doesn't silently no-op when a `profiles` row doesn't
  yet exist for a new user.

All of the above were reviewed line-by-line before being trusted — one
in particular (a detail-page rewrite from an untracked, previously-
unverified file found sitting on disk) turned out to contain a real bug
(the same unguarded-`.json()` crash pattern) that was caught and fixed
*before* being applied, not after.

---

## 2. Documented, explicitly not built

- `QMR_ABC_STATUS_NOTE.md` — logged and pushed to this repo.
- `PROPOSED_trade_copier_signal_distribution.md` — logged and pushed,
  explicitly labeled PROPOSED (not a decision record) per this repo's own
  Order of Authority. Describes a client-side EA / signal-distribution
  model (not managed execution — that was explicitly declined pending
  legal/compliance review) with a structural cost-control gating design
  (separate `copier_enabled` flag, manual key issuance, per-key rate
  limiting, no automatic activation rules for v1).

---

## 3. Flagged, not fixed — real open items for next session

1. **`profiles.status = 'approved'` silently grants full premium access
   regardless of `tier`.** Confirmed via direct query: 2 real accounts
   currently have `status: 'approved', tier: 'free'` and are receiving
   full paid content. Needs a product decision — was `approved` ever
   meant to mean "paid," or is this an unintended full-access bypass?
   ```sql
   SELECT status, tier, premium_status, COUNT(*)
   FROM profiles GROUP BY status, tier, premium_status;
   ```
   returned: `pending/premium/active: 1`, `revoked/free/inactive: 4`,
   `approved/free/inactive: 2`. The `pending/premium/active` row is also
   worth checking — a paying customer being blocked would be the
   opposite, revenue-visible problem.

2. **Console errors on `/live-trades` list page** —
   `Uncaught TypeError: Cannot read properties of undefined (reading
   'startTime')` at `et.reportAllChanges`. Function name suggests a
   web-vitals/analytics script, not application logic. Not yet
   diagnosed or confirmed unrelated to app code.

3. **`profiles` table column defaults** (`role`, `tier`, `status`,
   `premium_status`) were never actually checked in Supabase. Relevant
   to whether the `complete-profile` upsert fix (item 1 above, section
   1) could create an inconsistent row for a brand-new signup that
   never had these columns explicitly set.

4. **Items from an earlier, separately-flagged status report**, not
   independently verified this session: age 18+ gate, T&C consent step,
   captcha/bot-check on auth, a country-dropdown visual bug on
   `/complete-profile`, and the root cause of why a test account had no
   `profiles` row in the first place (possibly a missing signup
   trigger).

---

## 4. Process notes worth repeating (things that cost real time tonight)

- **PowerShell + square-bracket folder names (`[id]`, `[slug]`) need
  `-LiteralPath` on every cmdlet that touches them** — `Get-Content`,
  `Select-String`, `Test-Path`, `Copy-Item`, `Set-Location`, all of
  them. This bit us repeatedly even with `-LiteralPath` present, because
  relative paths (`.\page.tsx`) combined with a current directory that
  itself contains `[id]` can still resolve incorrectly — absolute paths
  are safer.
- **`Get-Content` mis-renders files containing em-dashes/curly quotes**
  in this environment; several docs files silently truncated mid-
  sentence at the first such character. `[System.IO.File]::ReadAllText()`
  read the same files correctly. Prefer it for any file that renders
  oddly.
- **A downloaded file is not on disk until it's actually been clicked
  and saved** — this tripped us up three separate times tonight (a
  status note, a page-file patch, and this proposal doc). Always verify
  with `Get-ChildItem` before assuming a `Copy-Item` source exists.
- **A file existing with a plausible name and recent timestamp is not
  evidence it does what a prior summary claims it does.** One session's
  confident status report (claiming several fixes as "done and
  verified") turned out to be wrong about at least one specific,
  checkable claim — a file that was described as already swapped into
  place had, in fact, never been copied. Every other claim in that
  report had to be independently re-verified rather than trusted.
- **Two separate documentation lineages exist for this project** — an
  older `Rev7_1`/Phase 0-8 roadmap (never actually committed to this
  repo, provenance unclear) and this repo's real, git-confirmed
  canonical lineage (`ONBOARDING_START_HERE.md` →
  `HipMarvinFX_Implementation_Directive.md` → `STANDING_PROTOCOL_*` →
  `HANDOVER_2026-09-12_PROJECT_STATE.md`). A new proposal was almost
  appended to the wrong (orphaned) file before this was caught by
  checking `git log` against the file directly.

---

**No secrets or API key values belong in this public documentation
repository.**

Logged: 14 September 2026
