# HipMarvinFX — Clarification: `profiles.status = 'approved'` is intentional comp-access

**Status:** Confirmed intent, recorded — not a bug, no code change required

---

## What this is

`profiles.status = 'approved'` grants a member **full premium access,
regardless of `tier` or `premium_status`**. This is checked as
`isApprovedOverride` in every access gate in the codebase:

- `app/lib/requireSubscriber.ts`
- `app/lib/requireSubscriberPage.ts`
- `app/trades/page.tsx`
- (and equivalent inline logic in `app/live-trades/[id]/page.tsx`,
  `app/components/layout/Sidebar.tsx`)

It is set via the **"Approve" button** in the admin member-management UI
(`app/admin/page.tsx`, around line 3597), which an admin clicks manually
on a per-member basis.

## Confirmed intent (14 September 2026)

This is **intentional comp-access functionality** — an admin deliberately
grants a specific member free premium access by clicking "Approve." It is
not a signup default, not payment-related, not automatically triggered.

This was investigated as a possible bug on 14 September 2026 after
discovering (via direct Supabase query) that 2 real accounts had
`status: 'approved', tier: 'free', premium_status: 'inactive'` and were
receiving full paid content. At the time this looked like an unintended
paywall bypass. It was confirmed with the project owner to be working
exactly as designed: those 2 accounts are legitimate, intentional comps.

**Do not re-investigate this as a security or billing bug without new
evidence.** If a future audit finds `approved/free/inactive` accounts
again, that is the expected shape of a comped account, not a leak.

## Known naming ambiguity (not fixed, flagged for future consideration)

The button is labeled **"Approve"**, which reads more like "this member's
signup/identity is legitimate" than "this member gets free premium
access." This ambiguity is exactly why the mechanism needed re-confirming
tonight instead of being immediately obvious from the UI. A future,
low-risk improvement worth considering:

- Rename the button/label to something that states the effect directly
  (e.g. "Grant Comp Access"), or
- Add a confirmation step or tooltip stating "This grants full premium
  access for free" before the action fires.

Not urgent — behavior is correct as-is. This is a clarity improvement,
not a correctness fix.

---

Logged: 14 September 2026
