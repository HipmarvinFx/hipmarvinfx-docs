# HipMarvinFX Parser Test Plan v6

## Goal

Prove that the v6 parser is deterministic, preserves historical data, and never turns 20D location into an automatic directional signal.

## Test groups

### A. Extraction

- Weekly metadata is extracted.
- Trade Priority List entries are separated correctly.
- Daily Liquidity/Flow Regime Check is extracted.
- Ledger records remain separate from trade ideas.

### B. Normalization

- Pair names normalize consistently.
- Direction values normalize to LONG/SHORT/CONDITIONAL.
- Percentages parse correctly.
- Flow labels normalize to canonical vocabulary.
- Source text is retained where interpretation could be affected.

### C. Rule 23

- RANGE accepted.
- TRANSITION accepted.
- DIRECTIONAL accepted only with supporting evidence.
- Unknown flow value fails validation.
- Premium does not imply SHORT.
- Discount does not imply LONG.

### D. Backward compatibility

- v5 files parse successfully.
- Missing v6 fields become `NOT_RECORDED`.
- No historical flow regime is inferred.
- Historical zone labels remain unchanged.

### E. Safety

- Missing executable stop blocks publication.
- Contradictory direction/trigger data blocks publication.
- Unconfirmed transition cannot become a directional public card.
- Parser never modifies Ledger status.

### F. Website contract

For each normalized fixture, assert the expected public presentation defined in `WEBSITE_PARSER_CONTRACT_v6.md`.

## Required regression suite

Run all cases in `PARSER_FIXTURE_V6.md` on every parser change.

A future CI job should fail the build if any fixture changes expected output without an explicit fixture update and review.

## Definition of done

The parser is ready for website integration only when all fixture tests pass and the normalized output matches the v6 contract exactly for required fields.
