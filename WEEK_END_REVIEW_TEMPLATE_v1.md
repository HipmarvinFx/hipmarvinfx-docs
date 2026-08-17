# HipMarvin FX — Week-End Review Template

**Changelog:**
- **v1 (2026-08-17):** Initial template. Formalises the Week-End Review structure that was previously written ad-hoc (see WEEK30_WEEK_END_REVIEW.md as the real-world reference). Adds `## MACRO REGIME REVIEW` section (Macro Regime snapshot Monday → Friday, regime changes with catalysts, Macro Thesis Scorecard, Macro → Trade Attribution, Next-Week Macro Carryover) inserted after `## WEEK N RESULTS` and before `## HONEST WEEK SUMMARY`. Existing sections (WEEK N RESULTS, CARRYOVER RESOLVED THIS WEEK, HONEST WEEK SUMMARY, LESSONS) are unchanged in structure — this template documents them explicitly for the first time. Week-Close Review parser does not yet exist in working form; this template is the definitive document structure it will eventually target.

---

## WEEK [N] RESULTS

*(One row per trade idea that reached a resolution this week — triggered and stopped, triggered and hit target, or expired unfilled. Carried positions that saw no change this week are noted in CARRYOVER RESOLVED THIS WEEK, not repeated here.)*

| Pair | Dir | Entry | Stop | TP1 | Result | Pips | Notes |
|------|-----|-------|------|-----|--------|------|-------|
| | | | | | WIN / LOSS / UNFILLED | | |

**Week net:** [+/– pips] across [N] resolved ideas ([N] wins, [N] losses, [N] unfilled)

*(If most entries stopped out around –1R, say that plainly rather than computing a flattering average — Rule 15.)*

---

## CARRYOVER RESOLVED THIS WEEK

*(Positions carried from a prior week that reached a resolution during this week. If none, write "None — no prior-week carryover resolved this week.")*

| Pair | Dir | Entry (week opened) | Exit | Result | Pips | Notes |
|------|-----|---------------------|------|--------|------|-------|
| | | Week [N] | | WIN / LOSS / EARLY EXIT | | |

*(Update the Position Ledger immediately after completing this section — this is the trigger for Ledger changes, per Rule 14.)*

---

## MACRO REGIME REVIEW

*(Presentational only — not machine-parsed. Complete this section before writing the Honest Week Summary. It is the structured record of whether the weekly macro framework was correct, independent of whether entries worked.)*

### Monday → Friday Macro Regime

| Currency | Monday Regime | Friday Regime | Change | Thesis Correct? |
|----------|---------------|---------------|--------|-----------------|
| USD | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| EUR | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| GBP | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| JPY | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| CHF | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| CAD | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| AUD | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| NZD | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| NOK | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |
| SEK | [Strong / Constructive / Neutral / Weakening / Weak] | [Strong / Constructive / Neutral / Weakening / Weak] | [↑ / → / ↓] | [Yes / No / Partial] |

### Macro Regime Changes

*(One entry per currency whose regime changed during the week. If no regime changed, write "No regime changes this week.")*

- **[Currency]:** [Previous] → [Current]
  - Catalyst: [release / development]
  - Why it changed: [brief explanation]
  - FX consequence: [pairs / direction]

### Macro Thesis Scorecard

**What the weekly macro thesis got right:**
[What the inflation/labour/central-bank framework correctly anticipated this week.]

**What the weekly macro thesis got wrong:**
[Where the fundamental interpretation failed, or where the data changed unexpectedly.]

**What was correct macro analysis but poor trading execution:**
[Separate macro correctness from entry/trigger/fill/position-management errors. A thesis can be right and a trade can still lose due to execution — record these separately.]

**What was a genuine macro regime error:**
[Only use this if the underlying economic/policy interpretation itself was wrong — not just the timing or entry. If none, write "None identified — losses this week were execution or catalyst-override failures, not macro regime errors."]

### Macro → Trade Attribution

*(One row per resolved trade idea this week, including carryover resolutions.)*

| Trade | Macro Regime at Entry | Macro Regime at Week End | Trade Result | Attribution |
|-------|-----------------------|--------------------------|--------------|-------------|
| [Pair] | [Regime] | [Regime] | [Win / Loss / Unfilled] | [Macro Correct / Macro Wrong / Entry Error / Execution Error / Catalyst Override] |

*(Attribution definitions: **Macro Correct** = the regime read was right, trade worked or failed for non-macro reasons. **Macro Wrong** = the underlying regime interpretation was incorrect. **Entry Error** = the macro read was right but the entry level, timing, or trigger discipline was wrong. **Execution Error** = the macro and entry were right but position-management (sizing, stop placement, early exit) caused the loss. **Catalyst Override** = an unforeseeable or unscheduled event invalidated an otherwise-correct setup.)*

### Next-Week Macro Carryover

**Currencies whose macro regime should carry forward into next week:**
- [Currency] — [reason the current read is still valid]
- [Currency] — [reason]

**Currencies requiring reassessment before next week's file is written:**
- [Currency] — [what specific data or event needs to confirm or change the read]
- [Currency] — [what needs confirmation]

**Open macro questions entering next week:**
1. [Question — e.g. "Does the Fed's hawkish hold translate into sustained dollar strength or was Wednesday's sell-off the real signal?"]
2. [Question]
3. [Question]

---

## HONEST WEEK SUMMARY

*(Free-form, 3–6 sentences. What actually happened this week vs. what the plan anticipated? Name the week's single biggest analytical or process error plainly, in one sentence — don't bury it in a longer paragraph (Rule 16). If most entries stopped out around –1R, say that plainly rather than computing a flattering average (Rule 15).)*

---

## LESSONS

*(Bullet list — one lesson per bullet, specific and actionable. Each lesson should be something that will change how next week's file is written or how next week's trades are managed. Avoid vague lessons like "be more patient" — name the specific situation that caused the problem and the specific rule or habit that would prevent it recurring.)*

- [Lesson 1]
- [Lesson 2]
- [Lesson 3]

---

**Sources:** [Daily Update file(s) referenced · Position Ledger version · FF screenshot date/time for any unresolved events · chart timestamps for any exit prices sourced this session]

*(Re-read before writing: before writing anything in this file, re-read the week's full Daily Update file and the Position Ledger top to bottom. Confirm every day marked "Closed" was actually resolved, and every position's status matches its latest verdict — the newest call wins. Rule 14.)*
