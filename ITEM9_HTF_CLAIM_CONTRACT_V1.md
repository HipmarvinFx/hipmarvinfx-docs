# ITEM 9 — HTF Claim Validation Contract v1

**Status:** APPROVED (decisions D-1 to D-4), not yet implemented
**Date:** 2026-10-08
**Branch:** item9-htf-claim-contract (from origin/main)
**Scope boundary:** contract only. No parser, route, importer or template change is authorised by this document.

## 1. Principle

The parser validates claims against deterministic packet evidence. It never manufactures missing engine evidence from template vocabulary.

## 2. Scope

Slice 1 covers three fields on Trade Priority List ideas: `HTF Trend`, `Trend Alignment`, `QMR Phase`.

Deferred, not changed by this contract: `Structural Break`, `QM/QML Refinement`, the Higher-Timeframe Trend Map (parserless, presentation-only), and any new engine state not already in the deterministic packet.

## 3. Canonical sources and mappings

| # | Claim | Packet field | Normalization | Comparison |
|---|---|---|---|---|
| 3.1 | `HTF Trend` (market-level) | `dailyTrend` | uppercase | exact equality (see §5) |
| 3.2 | `Trend Alignment` + idea direction D | `trendAlignment`, `preferredDirection` | uppercase, spaces to hyphens | see §4 |
| 3.3 | `QMR Phase` | `qmrPhase` | uppercase, spaces to hyphens | exact equality |

- **3.1 (D-1):** `HTF Trend` remains the market-level state BULLISH / BEARISH / TRANSITION / CONFLICT (preserves Rule 34). `Trend Alignment` is the idea-level directional interpretation. The two must not duplicate each other. Daily is the direction authority in the engine (`trend-alignment.ts`); a 4H-only claim is out of slice.
- **Idea direction D** is derived from the idea itself: Buy = LONG, Sell = SHORT.
- A label that cannot be normalized to an engine enum yields NOT_AVAILABLE, never FAIL.
- The label "Choppy" maps to TRANSITION (not CONFLICT). It is dormant in slice 1 because the Trend Map is deferred.
- Engine vocabulary is authoritative and hyphenated (e.g. `WITH-TREND`). No second vocabulary is introduced.

## 4. Direction-specific semantics

For an idea with direction D:

- `WITH-TREND`: PASS only if packet `trendAlignment` is `WITH-TREND` and `preferredDirection` equals D.
- `COUNTERTREND-AFTER-STRUCTURAL-BREAK`: PASS only if packet has that alignment and `preferredDirection` equals D.
- `NOT-ELIGIBLE`: PASS only if packet `trendAlignment` is `NOT-ELIGIBLE` (`preferredDirection` null).
- Any other combination: FAIL.

## 5. CONFLICT and TRANSITION

For structured HTF claims, CONFLICT and TRANSITION are evaluated explicitly: a directional Bullish/Bearish claim against either state is FAIL, while an explicit Transition/Conflict claim matching the packet state is PASS. This changes Item 9 claim-validation behaviour only; no unrelated existing parser behaviour is modified.

## 6. Check result (HTF rules only, D-4)

| Result | Meaning |
|---|---|
| PASS | Claim matches the packet. |
| FAIL | Claim contradicts the packet. |
| NOT_AVAILABLE | No packet for the pair, label cannot be normalized, or the packet field is null. |

Aggregate verdict over HTF checks:

    any FAIL          -> REJECTED
    else any N/A      -> PENDING
    else all PASS     -> VALIDATED
    zero checks       -> PENDING

NOT_AVAILABLE must never be converted into FAIL to force a definitive verdict.

The existing price-rule behaviour (`EVIDENCE_MISSING` -> REJECTED) is **unchanged** by Item 9. The tri-state is scoped to the new HTF rules only; global evidence semantics may be revisited separately.

Response honesty: the route response must state whether HTF rules were applied and how many HTF claims were extracted. `htfPairsValidated` currently counts reconstructed packets, not validated claims; it is to be renamed to say so, with an alias kept until its readers are identified.

## 7. QMR per side (D-3)

The packet carries one `qmrPhase` and cannot manufacture an opposite-side QMR state.

For a conditional idea, a QMR claim is validated only when its side equals the packet's `eligibleDirection`. A claim for the opposite side is NOT_AVAILABLE; it must never be inferred, mirrored, or treated as FAIL. If `eligibleDirection` is null, side-specific QMR validation is NOT_AVAILABLE, because null means the packet establishes no eligible direction, not that both sides lack QMR.

## 8. Conditional-idea schema (D-2)

Canonical form is one dash-field per line with exact labels:

    HTF Trend (long side)
    HTF Trend (short side)
    Trend Alignment (long side)
    Trend Alignment (short side)
    QMR Phase (long side)
    QMR Phase (short side)

- Combined conditional lines (e.g. "HTF Trend / Trend Alignment / Structural Break (per side)") are retired as a canonical format.
- Labels are matched whole and anchored. The importer's current prefix-match behaviour is not a precedent.
- Canonical one-line-per-field form is already delivered by `WEEKLY_RESEARCH_TEMPLATE_v5_1.md` (L195-204), a deliberate versioned change under Rule 20. No template v6 is required for this contract. A Standing Protocol note recording the canonical form remains outstanding.

## 9. Importer and template compatibility

- A stored column must never hold a different semantic than its name states.
- Required sequence: **contract -> importer repair -> real import proof -> parser validation.** (Template step satisfied by v5.1.)
- The importer is repaired against §8 and template v5.1. The `getDashField` regex is not patched before the schema is frozen.
- A real v5/v6 import must pass through the repaired importer and the stored values be inspected before Item 9 validation relies on those columns.

## 10. Exclusions

No change to `Structural Break` or `QM/QML`. No Trend Map parser. No prose enum matching. The `claims-json` channel is not used for HTF. No new engine field is invented to make template vocabulary validate.

## 11. Fixture requirements

Synthetic template-format text plus plain packet objects, no database dependency, for standard and conditional ideas. Reference packet states, taken from the as-of dry run at cutoff 2026-10-05T03:10:00.000Z: EURUSD and GBPUSD BEARISH/BEARISH, WITH-TREND, QUALITY; USDJPY BULLISH/BULLISH, WITH-TREND, QUALITY.

1. EURUSD Short, With-trend, Quality -> PASS.
2. EURUSD Long, With-trend -> FAIL.
3. USDJPY Long, With-trend, Quality -> PASS.
4. GBPUSD Long, Countertrend -> FAIL.
5. USDJPY Long with `HTF Trend: Transition` -> FAIL.
6. Pair absent from the packet set -> NOT_AVAILABLE.
7. Unmapped label -> NOT_AVAILABLE.
8. Conditional idea with differing long/short sides -> each side checked independently (QMR per §7).
9. A correct 4H-style statement must not fail against the Daily field (regression for the old keyword-anchoring false positive).

## 11a. Packet shapes (clarification)

This contract validates claims against `HtfEvidencePacket` (`lib/engine/htf-evidence-packet.ts`), reconstructed from `evidence_store` rows for historical validation. It is distinct from the legacy `EvidencePacket` and from `CanonicalResearchEvidencePacket` described in the (unmerged) `docs/evidence-packet-contract-reconcile` branch. This contract does not amend `EVIDENCE_PACKET_CONTRACT_V1.md`.

## 12. Open items recorded

- **A:** importer conditional-schema mismatch. `getDashField` interpolates labels into a regex unescaped, so `(long side)` / `(short side)` never match; the template's combined line does not match the importer's per-side labels either. Replica test showed per-side lookups returning empty and the plain label returning the first line or the whole combined string. Also documented as a known limitation in `WEEKLY_RESEARCH_TEMPLATE_v5_1.md` (parser note and limitation list): both sides receive the long-side value. Replica-confirmed here; no HTF values have ever been stored (0 rows in `trade_ideas`), so no real import has demonstrated it. Until repaired, stored short-side columns must not be relied on. Tracked in parallel; not patched.
- **B:** the template cannot express engine states UNCONFIRMED (Structural Break) or PRESENT/NOT-PRESENT (QM/QML).
- **D:** `parseAndValidateV7` skips HTF rules whenever `rawText` is present, and `buildHtfClaimRule` scans prose; as wired it cannot emit an HTF check on any path. This contract replaces that design for structured claims.

## 13. Decision record

D-1 market-level `HTF Trend`: approved. D-2 one exact field per side: approved. D-3 eligible-side QMR rule, with §7 wording: approved. D-4 HTF-only tri-state: approved. §5 wording amendment: approved.