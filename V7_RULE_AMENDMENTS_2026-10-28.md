# Rule Review Amendments (approved 2026-10-28)

Owner-approved. Commit to hipmarvinfx-docs. Supersedes ad-hoc gating added during Week 44.

## Severity model (applies to every rule)
- **BLOCK** stops publication. Reserved for fabrication risk and data integrity.
- **DISCLOSE** publishes with a visible label (for example FALLBACK_FRAMEWORK, or a WARN flag line).
- **INFO** is logged only.
- A rule with no machine check and no named owner is removed. "Ask the owner" is never a rule's output.

## Amendments
1. **Publish gate.** Only BLOCK flags, RED status, publishable=false, or a missing, invalid or stale attestation block. WARN flags publish with disclosures. Replaces the AMBER allowlist of 2026-10-27.
2. **Suggested Action.** Derived only before a trigger. Triggered, Active and TP1 Hit show "Active: Manage Risk". TP2 Hit, Stopped Out and Closed show "Resolved".
3. **Zones.** Name them: the 20-day zone is context only and never alone makes an idea against its own bias. Entry Quality uses it together with the idea's structure: with-trend ideas in an extended location read "Extended, With Trend". Premium is not an automatic sell, and discount is not an automatic buy. Supersedes the Entry Quality table in PUBLISHING_PIPELINE v1.7 where they conflict.
4. **Trigger.** "Triggered" means an entry touch on recorded price. Confirmation (ABC) is a separate fact, shown only where recorded.
5. **Staleness.** Every outlook shows an evidence as-of time and its age. After a day the banner states that levels reflect that time plus daily updates.
6. **Empty sections** say why they are empty.
7. **One canonical Standing Protocol version.** Archive v2 to v5 once the current version is named.

## Decisions recorded alongside
- **FALLBACK framework.** Used where ABC is absent. A touched entry executes without ABC confirmation when trade construction is READY, levels are valid, R:R is at least 1, risk is at most 2%, and structure does not contradict. Labelled FALLBACK_FRAMEWORK. Held ARMED only when macro alignment is explicitly false.
- **Known gap.** macroAligned is not yet populated, so the macro veto is inactive.
