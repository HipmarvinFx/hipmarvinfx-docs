# HipMarvinFX — Proposed Work Item: Trade Copier / EA Signal Distribution

**Status:** PROPOSED — not a decision record, not started, no build authorized

This document logs a feature idea discussed and researched on 14 September
2026. It is explicitly **not** an implementation directive. Per this repo's
own Order of Authority (`ONBOARDING_START_HERE.md`, item 10), a proposal
sits below any actual decision record and must not be treated as approved
scope until someone produces a decision record that says otherwise.

---

## What this is

Allow subscribers to auto-execute HipMarvinFX trade ideas in their own
MT4/MT5 broker account via a local Expert Advisor (EA), without HipMarvinFX
ever holding execution authority, broker credentials, or client funds.

This is explicitly the **signal-distribution model** (client's own EA acts
on the client's own broker account), not a managed-execution model where
HipMarvinFX would place trades on a client's behalf via broker API. The
managed-execution version was raised and explicitly declined during
discussion — it likely crosses into regulated investment-adviser/money-
manager territory depending on jurisdiction and would require legal/
compliance review before any architecture work, not after. It is not part
of this proposal.

---

## Architecture direction (research, not yet verified against this repo's code)

Webhook/polling-based, client-side EA — not MT4-to-MT4 server bridging.
The client's own local EA (running on their machine or VPS) polls a signed
HipMarvinFX signal endpoint and executes trades using the client's own
broker session inside their own MetaTrader terminal. HipMarvinFX never
touches broker credentials or places trades directly.

This direction was chosen over direct MT5-to-MT5 API bridging because
MetaQuotes has been actively restricting third-party MT5 API access;
webhook/local-EA architectures are the more future-proof pattern
industry-wide as of the research done alongside this proposal.

**Per `ONBOARDING_START_HERE.md` §4-5: none of the following has been
verified against the actual repository or database.** This section
describes what a build would plausibly need, based on general familiarity
with `trade_ideas`/`trade_results` discussed elsewhere in this project's
documentation — not a confirmed audit of current schema or code.

Plausible requirements, pending verification:
- A new `api_keys` table (EAs cannot use browser session cookies)
- A new `profiles.copier_enabled` boolean column
- A new `/api/copier/signals` endpoint
- A separate MQL4/MQL5 EA codebase (distinct skillset/repo from the Next.js app)

---

## Cost-control / access-gating mechanism (the actual design constraint)

The reason this is worth logging even while unstarted: it must ship in a
**structurally zero-cost-until-activated** state, not "built and hoped
nobody uses it yet."

**Gate 1 — separate flag, not tier-inherited.** Being a paid subscriber
must not automatically grant copier access. A distinct `copier_enabled`
flag, default false, is required as a second gate on top of whatever the
subscription-tier check currently is.

**Gate 2 — manual key issuance only, at launch.** API keys are issued by
an admin action, not self-served or auto-generated on signup. No request
can reach the endpoint successfully until an admin has both enabled the
flag and issued a key for that specific account.

**Gate 3 — per-key rate limiting, independent of Gates 1/2.** Once
enabled, each key should be rate-limited server-side, keyed to the API
key itself rather than IP.

**Explicit non-goal for v1: no automatic activation rules.** Auto-enabling
`copier_enabled` based on tier duration, spend, or any derived condition
is deliberately out of scope. This project has already found a real,
live instance of an access condition silently meaning something different
than intended (a `profiles.status` value unintentionally granting broader
access than a tier field alone would suggest — found and fixed 14
September 2026, separate work). A feature with real infrastructure-cost
implications should not inherit that risk class. Manual-only activation
until the mechanism is proven correct in practice.

---

## Open questions, not yet decided

- Does the EA only need to see newly-opened trade ideas, or does it also
  need to receive stop-loss/target modifications and closes after entry?
  The latter is materially larger scope and needs its own explicit
  go/no-go, not an assumed yes.
- Where does this fit relative to the actual current critical path listed
  in the live project handover? This proposal does not claim priority
  over anything already in flight.

---

## Next step, if this moves forward

Per `ONBOARDING_START_HERE.md`'s own required process: before any code is
written, verify the actual current schema (`trade_ideas`, `profiles`,
existing rate-limit helpers) against the real repository, not against
this document's assumptions. If verified and still wanted, this should be
converted into a real decision record — following the same format as
`HipMarvinFX_Implementation_Directive.md` — at that point, not before.

---

**No secrets or API key values belong in this public documentation
repository.**

Logged: 14 September 2026
