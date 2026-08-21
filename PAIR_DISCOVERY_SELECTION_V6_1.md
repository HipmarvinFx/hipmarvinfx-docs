# HipMarvin FX — Pair Discovery & Best Expression v6.1

**v6.1 — Pair-selection architecture · 2026-08-21**

This document adds the missing selection layer to the v6 methodology. It does **not** weaken or replace Rule 23. It determines which currency relationship and which pair deserve evaluation before the existing liquidity/flow and execution rules are applied.

## Canonical v6.1 architecture

**Macro Regime → Currency Relative Strength → Pair Discovery (Majors + Liquid Crosses) → Best Expression → 20D Location → Liquidity → Flow Regime → Acceptance/Rejection → Premium/Discount Context → Catalyst → Execution.**

The architecture is deliberately ordered this way because pair selection is an opportunity-discovery problem, while Rule 23 is a validation/execution problem.

---

## Rule 24 — Currency-relative pair discovery

### 24A — No major-pair whitelist

The research universe must include:

- Major USD pairs
- Liquid G10 crosses
- Other liquid pairs only where liquidity, spread, data quality, and execution conditions are adequate

A pair must **never be excluded solely because it is a cross**.

The absence of a USD leg is not a negative score by itself.

### 24B — Start with currencies, not symbols

First rank currencies by the current macro regime and observable relative strength/weakness. Then construct candidate pair expressions from the strongest-vs-weakest relationships.

Example logic:

- Strong GBP vs weak JPY → GBP/JPY becomes a valid candidate even if GBP/USD and USD/JPY are less attractive individually.
- Strong CAD vs weak JPY → CAD/JPY becomes a valid candidate.
- Strong AUD vs weak NZD → AUD/NZD becomes a valid candidate.

The pair is selected because it expresses the underlying currency relationship, not because it belongs to a preferred symbol list.

### 24C — Compression creates a discovery opportunity

When major pairs are compressed, range-bound, or offering poor directional expression, actively test liquid crosses for a cleaner expression of the same currency-relative regime.

Do not treat major-pair compression as evidence that the broader currency thesis is invalid.

### 24D — Crosses are candidates, not automatic trades

A cross only survives discovery if it offers a sufficiently strong combination of:

- Currency-relative strength differential
- Macro/regime alignment
- Liquidity quality
- Clear structural location
- Identifiable liquidity destination
- Flow-regime evidence or a credible transition path
- Catalyst relevance
- Acceptable execution/risk characteristics

Discovery ranks opportunities. It does not create an entry.

---

## Rule 25 — Best-expression selection

For each material currency thesis, compare the available expressions rather than defaulting to the major pair.

The **Best Expression** is the pair that most cleanly converts the macro/relative-strength thesis into an executable opportunity after considering:

1. Relative-strength differential
2. Macro alignment
3. Price structure and 20D location
4. Liquidity map and target availability
5. Flow regime
6. Acceptance/rejection evidence
7. Catalyst timing
8. Correlation and concentration
9. Spread/liquidity/execution quality
10. Risk/reward

The best expression may be a major or a cross.

### 25A — Expression comparison

When two or more pairs express substantially the same thesis, state why one is preferred.

Use:

- **Preferred expression:** [pair]
- **Alternative expression:** [pair]
- **Why preferred:** [relative-strength / structure / liquidity / flow / catalyst / execution reason]
- **Why alternative is weaker:** [specific reason]

Do not select a pair merely because it is more familiar or because it has a USD leg.

### 25B — Correlated expression control

Multiple pairs can represent the same underlying currency bet. The discovery layer must identify that concentration before the Priority List is finalized.

A cross is not an independent idea simply because its symbol differs from a major.

---

## Rule 26 — Premium/discount is contextual

Premium and Discount remain useful location descriptors under Rule 21.

They are **not automatic directional filters**.

Therefore:

- Never exclude a bullish expression solely because it is in Premium.
- Never exclude a bearish expression solely because it is in Discount.
- Never fade a directional move merely because price is stretched within the 20D range.

A Premium Buy or Discount Sell requires explicit continuation justification under Rule 23, especially liquidity acceptance, displacement/follow-through, and a credible next liquidity objective.

### 26A — Continuation test

**Sweep → acceptance → shallow retracement → next liquidity target** supports continuation.

### 26B — Reversal test

**Sweep → rejection → return into range** supports mean reversion.

A wick alone does not establish either conclusion.

---

## Rule 27 — Discovery output

Before the Trade Priority List, the weekly research must produce a compact **Pair Discovery Matrix**.

| Currency Thesis | Candidate Pair | Major/Cross | Relative Strength | 20D Location | Liquidity | Flow | Catalyst | Expression Rank | Decision |
|---|---|---|---|---|---|---|---|---:|---|
| [strong vs weak] | [pair] | [Major/Cross] | [High/Med/Low] | [zone] | [quality/target] | [regime] | [event] | [1..n] | [Select / Watch / Reject] |

### Required decisions

- **Select:** best expression and eligible for Rule 23 evaluation.
- **Watch:** interesting relationship, but missing confirmation, catalyst, or execution quality.
- **Reject:** insufficient liquidity, weak differential, conflicting evidence, poor risk, or excessive concentration.

A cross should only be rejected for an actual analytical/execution reason—not because it is a cross.

---

## Rule 28 — Handoff to Rule 23

Once the best expression is selected, Rule 23 remains intact.

The handoff is:

**Pair discovered → best expression selected → 20D location established → liquidity mapped → flow regime classified → acceptance/rejection assessed → catalyst checked → Rule 23 execution gate applied.**

Rule 23 therefore remains the final analytical gate for trade expression; v6.1 simply ensures the gate is applied to the **best available pair**, rather than to a prematurely restricted universe.

---

## v6.1 core principles

> **Never exclude a cross because it is a cross.**
>
> **Never fade a trend merely because it is in Premium.**
>
> **Find the strongest currency relationship first; then choose the cleanest pair expression.**
>
> **Pair discovery expands the opportunity set. Rule 23 still decides whether the opportunity is tradable.**
