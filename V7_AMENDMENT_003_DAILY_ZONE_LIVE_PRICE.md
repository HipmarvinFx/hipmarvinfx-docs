# V7 Amendment 003 - Daily Zone recorded at live price (Rule 21 exception)

**Status:** IN FORCE by owner decision (Elijah Agom, 2026-11-02).
**Effective:** Week 45 (2026-11-01 to 2026-11-07) and every later weekly file, until a numbered amendment or protocol revision supersedes it.
**Nature:** a documented exception. The Rule 21 formula and bands are unchanged (Rule 20: deliberate, versioned change).

## Why this exists

Conflict found by reading code (app commit 91b29a6):
- Rule 21 computes the Daily Zone from the ENTRY price.
- The write gate (lib/integrity/idea-check.ts) compares the idea's zone_position_pct to the LIVE zone and blocks when the gap exceeds ZONE_TOLERANCE_POINTS (5). REQUIRE_IDEA_ZONE is true, so a missing zone also blocks.
- Week 44 EUR/USD was published with Daily Zone 31.4% (live price) while its entry zone 1.1212 sat at 62.8%. It passed the gate.

Under the Workflow Directive the conflict was reported and only this decision was paused. The owner chose Option A (keep the gate, record the zone at live price).

## The exception

1. **Daily Zone field** on a Trade Priority List idea = the zone at the live price at the evidence cutoff (the engine live zone), in the existing format "X% up the 20D range - Label".
2. **Entry zone** must be stated in Reasoning: "Entry zone: Y% up the same 20D range (Label)", computed with the Rule 21 formula on the same 20D High and Low. If those values are not sourced, write NOT SOURCED. Never estimate.
3. **Rule 21B disclosure (Buy in Premium, Sell in Discount):** judge the idea on both the live zone and the entry zone. Disclose a conflict if either one conflicts. This remains a soft flag, not a blocker.
4. **Position Ledger:** copy Daily Zone exactly as published (the live-price value), per Ledger v3. Do not re-derive it. The entry zone lives in Reasoning/Notes only.
5. **Do not** change the gate or its tolerance to make an idea pass. **Do not** replace the live-price value with the entry-zone value to "match the protocol": the gate will reject it when the gap exceeds 5 points.

## Not verified

- That the engine liveZonePct is computed from the latest close (inferred from the gate selftest values only).
- Which routes call the idea gate on the admin import path (taken from the audit, not read).

## Planned resolution

Phase 5 decision note: either amend the gate to compare against the zone at entry (Option B), or amend Rule 21 to define the Daily Zone at live price. Until then this exception stands.

## Documents that should carry a pointer (not edited by this amendment)

- WEEKLY_RESEARCH_TEMPLATE_v5_1.md (Daily Zone field and parser note)
- POSITION_LEDGER_TEMPLATE_v3.md (Daily Zone field rule)
- HipMarvinFX_Generation_Prompts_v7_x

## Numbering note

V7_AMENDMENT_001 and 002 are unmerged DRAFTs on branch docs/generation-prompts-v7-1 and are not on main. This amendment is numbered 003 to avoid a collision. Nothing in 001 or 002 is relied on here.