# HipMarvinFX v7 — Integration Boundary Lift

**Status:** Approved implementation decision
**Date:** 29 August 2026
**Scope:** First v7 end-to-end vertical slice

## 1. Decision

The Phase 2 boundary rule prohibiting modification of the existing v6.1 daily pipeline is formally lifted for the first v7 integration vertical slice.

Phases 2–5 of the Evidence Layer have been independently committed, tested, and verified. The Evidence Packet is now proven in isolation. The next priority is therefore to prove that the Evidence Layer can control the actual research pipeline rather than remain a collection of disconnected infrastructure modules.

## 2. Authorized scope

The boundary lift authorizes only the minimum integration required to prove the following path:

```text
Validated Evidence
    ↓
Evidence Store
    ↓
Evidence Packet
    ↓
AI Interpretation
    ↓
V7 Parser / Publication Firewall
    ↓
Validated Output
    ↓
Persistence / Publication
```

The initial implementation may use deterministic fixtures for COT and economic-calendar evidence, as permitted by `MACRO_ENGINE_SPEC_V1.md` §19.

## 3. Files authorized for integration

The first vertical slice may create or modify only the files required to establish the evidence-to-AI-to-parser path, including:

- `lib/parser/types.ts`
- `lib/parser/v7-parser.ts`
- `lib/parser/rules/price-rules.ts`
- `lib/pipeline/daily.ts`
- `lib/ai/adapters/index.ts`

Additional files require explicit justification if they fall outside this scope.

## 4. v6.1 preservation

This decision does **not** replace or redesign the v6.1 methodology.

The existing v6.1 production path remains operational during the vertical-slice validation period. The v7 path must initially operate in controlled or shadow execution until its end-to-end safety properties are demonstrated.

No cutover to v7 publication is authorized merely because the new modules compile or unit tests pass.

## 5. Publication firewall requirement

The v7 parser is a publication firewall, not a formatter or repair mechanism.

It receives:

```text
AI output
+
Evidence Packet
```

and must validate factual claims against the packet and applicable deterministic methodology rules.

Unsupported claims must be rejected, marked pending, or otherwise blocked according to the canonical contracts. The parser must never repair an unsupported value by guessing or asking the AI to supply a replacement.

In particular, the vertical slice must demonstrate that a fabricated price cannot reach the persistence/publication layer.

## 6. Macro Engine sequencing

The full Macro Engine remains deferred until the vertical slice proves the architecture end-to-end.

The first slice may use deterministic COT and calendar fixtures. This is explicitly allowed by `MACRO_ENGINE_SPEC_V1.md` and separates proof of the evidence pipeline from the later problem of production macro-source ingestion and deterministic macro scoring.

## 7. Canonical contract authority

Implementation must conform to the canonical v7 contracts in `hipmarvinfx-docs`.

The Evidence Packet contract establishes that AI receives evidence packets rather than raw research inputs, that packet health states are explicit, that facts and derived values retain provenance, and that the parser receives both AI output and the Evidence Packet. See `EVIDENCE_PACKET_CONTRACT_V1.md`.

The Macro Engine contract establishes that only acceptable validated evidence may feed deterministic macro calculations, that missing data must remain explicit, and that the first vertical slice may use deterministic fixtures. See `MACRO_ENGINE_SPEC_V1.md`.

## 8. Required adversarial proof

Before v7 publication cutover, the integration must demonstrate at minimum:

- unsupported price → `REJECT`;
- unsupported COT claim → `REJECT`;
- unsupported macro actual → `REJECT`;
- missing provenance → `REJECT`;
- stale evidence → `STALE` / not accepted as current evidence;
- unavailable evidence → `NOT_AVAILABLE`;
- missing required evidence → `PENDING` / blocked from unsupported publication;
- rejected parser output → zero database/publication write;
- valid evidence-backed output → `VALIDATED` and eligible for persistence.

The decisive test is not merely whether parser unit tests pass. The decisive test is whether a deliberately fabricated AI response can traverse the real integrated pipeline and reach persistence. It must not.

## 9. Rollback

If the vertical slice causes build failure, breaks existing v6.1 behavior, or permits unsupported claims to reach persistence, v7 integration must be disabled/reverted while the known-good v6.1 path remains available.

## 10. Governance note

This boundary lift is a deliberate architecture decision and supersedes the earlier Phase 2 restriction only for the explicitly defined vertical-slice scope. It does not authorize unrestricted refactoring or silent changes to the v6.1 methodology.
