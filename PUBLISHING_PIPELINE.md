# HipMarvinFX — Publishing Pipeline v1.9

> **v1.9 — v6 Liquidity/Flow publication alignment.** For v6 research files, `WEBSITE_PARSER_CONTRACT_v6.md`, `WEBSITE_PAGE_SCHEMA_v6.md`, and `V6_WEBSITE_INTEGRATION.md` govern parser normalization and website presentation. This v6 contract supersedes the older Rule 21 zone-direction interpretation **for v6 inputs only**. Historical v5 research remains governed by the v5 rules and must not be retroactively reclassified.

## v6 publication authority

For v6 research files, the publication chain is:

`Research File → v6 Parser Contract → Validation → Publication Derivation → Website Pages`

The 20-day range is **structural location**, not directional instruction. Website derivation combines:

- `daily_zone` — location
- `liquidity_flow_regime` — `RANGE` / `TRANSITION` / `DIRECTIONAL`
- `liquidity_state` — sweep/acceptance state
- `zone_flow_relationship` — relationship between location and flow

The website/parser must never reject a Premium Buy or Discount Sell solely because of location. It must surface the location/flow relationship and preserve the research conclusion.

### v6 website derivation examples

| Location | Flow | Liquidity state | Public interpretation |
|---|---|---|---|
| Deep Premium | DIRECTIONAL | Swept + accepted | Premium / Bullish Continuation when direction is bullish |
| Deep Premium | RANGE | Swept + rejected | Premium / Reversal Watch |
| Mild Premium | TRANSITION | Confirmation pending | Transition / Wait for Confirmation |
| Deep Discount | DIRECTIONAL | Swept + accepted | Discount / Bearish Continuation when direction is bearish |

These are presentation mappings, not new trading rules. The research file remains the source of analytical conclusions.

## Historical compatibility

v5 files remain parseable. Missing v6 fields must be represented as `NOT_RECORDED` rather than inferred. Historical Premium/Discount flags must not be silently converted into v6 flow conclusions.

---

Updates v1.8: resolves two genuine specification gaps surfaced while
translating the Setup Quality Derivation Rule (Layer 1, v1.5) into code,
not implementation detail — both change what rating a real trade idea
receives, so both are recorded here as spec changes, with code following
afterward.

- **Removed the undefined "dominant/majority category" concept from
  COT-supports.** Leveraged Funds and Asset Managers are now treated as
  two independent evidence sources for both COT states: support or
  opposition requires both categories to agree; disagreement is Mixed COT
  either way. The prior "dominant category" wording never specified how
  to pick one, and since Asset Manager position sizes run consistently
  larger than Leveraged Funds' in the COT Positioning table, a
  size-based dominance rule would have made Asset Managers dominant by
  construction rather than by genuine signal — inconsistent with how the
  Week 32 file already reads COT (treating Lev Funds/Asset Mgr agreement
  or disagreement itself as the meaningful signal, e.g. EUR/GBP
  disagreement vs. AUD/JPY/CAD agreement).
- **Resolved the previously unenumerated Setup Quality branch** where an
  idea's direction runs against the Weekly Thesis and is not supported by
  COT (Mixed COT or unanimous COT opposition). This is now explicitly
  🔴 Weak, not left undefined. Reasoning: an idea that both opposes the
  house thesis and lacks independent institutional backing has no basis
  left to stand on — Contrarian is reserved for ideas that oppose the
  thesis but carry genuine COT support, preserving Contrarian as a status
  with objective evidence behind it rather than a catch-all for
  thesis-opposing ideas generally.
- Added a Setup Quality decision table for unambiguous reference,
  reproducing the same logic as the bulleted rule in table form.

No changes to Entry Quality, Suggested Action, Stage 1/2/3 structure, the
Risk Assessment fields, or the Headline Selection Rule.

Updates v1.7: adds an explicit cross-reference to `EDITORIAL_STANDARD_v1_1.md`,
clarifying the relationship between this document's own External Review
Intake section (below) and that Standard's general Editorial Review
Workflow. External Review Intake governs *whether* an externally-sourced
recommendation should be acted on; the Editorial Standard governs *how*
any accepted edit — to this document or any other canonical file — is
actually produced. The two were developed independently and, until now,
never cited each other despite covering overlapping ground (evaluating
and acting on proposed changes). This update resolves that gap without
changing either document's actual workflow steps, consistent with the
Document Consolidation Principle below (cross-reference, not duplicate).
Also adds one sentence to this document's opening section noting that
revisions to this document itself are governed by the Editorial Standard,
the same way `STANDING_PROTOCOL_v4.md` already governs this document's
research-file-facing content. No changes to Stage 1/2/3 structure, the
Risk Assessment fields, the Headline Selection Rule, or any content
carried over from v1.6.

Updates v1.6: adopts two genuine extensions surfaced by external review —
(1) a **Weekly Outlook → Weekly Research Article** Stage 2 mapping table,
closing the Open Item that Stage 2 only covered Daily Update fields, and
(2) a **Document Consolidation Principle** under External Review Intake,
generalizing the reasoning already applied when the Website Experience
Layers' derivation logic was evaluated for extraction and deferred at
v1.5 — both added below. **Reviewed and explicitly not adopted:** an
alternative "Opportunity / Entry / Recommendation" star-rating scheme
proposed as a replacement for the Setup Quality / Entry Quality model.
It contains a real, demonstrable internal contradiction, not a style
disagreement: its own rule requires no Rule 21B conflict to reach 4–5
stars, while its own Recommendation table defines "Wait for Pullback" as
4–5 stars *combined with* Buying High/Selling Low — a condition that is,
by the same document's own definition, identical to a Rule 21B conflict.
The two requirements can never both be true, making "Wait for Pullback"
unreachable by construction. The Setup Quality / Entry Quality model
avoids this because the two dimensions are genuinely independent by
design (Setup answers "is this a good idea," Entry answers "is this a
good price," and neither gates the other) — that independence is exactly
what let the real Gold and USD/CAD bugs get caught and fixed in v1.5, and
collapsing it back into one gated score would reintroduce the same class
of bug. **Also explicitly not adopted:** an external draft's relabeling of
External Review Intake's origin as "v1.3→v1.4." The real, verified history
places it at v1.3, full stop — no v1.4 changelog entry exists to attribute
anything to. Per this document's own documentation discipline (leave
unexplained gaps unresolved rather than inferring provenance for them),
that gap stays open rather than silently filled a second time.

Updates v1.5: adds a three-layer **Website Experience Layers** architecture
(Dashboard → Trade Page → Research Article). Layer 3 is exactly the
existing Stage 2/3 mapping below, unchanged. Layers 1 and 2 are new,
resolving two Open Items carried since v1.2/v1.3: the Weekly Outlook side
of Stage 2's mapping table, and "derived publication sections... generator
logic not formalized." Setup/Entry/Action derivation logic is specified
directly in Stage 2, below — it is Stage 2's own output (publication
translation of already-written research), never something the Stage 1
analyst narrates, so it was never a candidate for `HipMarvinFX_Generation_
Prompts_v4.md`; that document was checked (against v4.6, the version
current at review time) and confirmed to contain no conflicting Dashboard
logic, resolving the "check for consistency" Open Item rather than merely
deferring it. All Dashboard/Trade Page inputs (Macro Thesis, Overall Bias,
Tier, Correlation Class, COT Positioning, Daily Zone/Rule 21, Reasoning)
already exist in `WEEKLY_RESEARCH_TEMPLATE_v3.md` — no new research-file
field required, no change to `STANDING_PROTOCOL_v4.md`. Two independent
external reviews were run before this logic was finalized; both converged
on separating public-facing labels (Setup / Entry / Action, short, for a
10-second Dashboard read) from the fuller internal terms (Setup Quality /
Entry Quality / Suggested Action, used in derivation logic and on the more
detailed Trade Page) and on renaming Trade Quality → Setup Quality, Status
→ Suggested Action, Neutral → Not Rated. Convergence was treated as reason
to verify, not as validation — one recommendation both reviews made
(deleting the derivation logic from this document entirely, leaving only
output labels) was **not** adopted, since it would break the
audit/reproducibility standard this document holds Stage 1 to. A
verification pass ahead of those reviews also surfaced two real derivation
bugs, both fixed in the rules below: (a) a Tier 1 catalyst could push an
idea to "Strong" even when COT actively opposed it on both sides (the
USD/CAD case); (b) Entry Quality was incorrectly gated on Setup Quality
being rated at all, hiding a real, chart-sourced entry read (the Gold
case). **Standing criterion for future extraction:** if this derivation
logic grows substantially, or begins supporting more than one publication
target (website, email digest, mobile app, an API), it should move to its
own dedicated document — working name **Publication Derivation Logic**,
not "Generator Logic," since "generator" risks being confused with the
Stage 1 Generation Prompts — and this Pipeline would reference it rather
than reproduce it. Until that trigger is met, it stays here, in one place,
at Stage 2, where it's produced and consumed.

Updates v1.3: adds a new "External Review Intake" section (with a
"Reviewer Independence" subsection), placed after Division of
Responsibility and before Analytical Depth Rule. This is a governance
rule for how the methodology itself evolves in response to outside
review — trace every claim to source text, evaluate a recommendation's
conclusion separately from its justification, check consistency across
the four-document architecture, distinguish genuinely new issues from
restatements, and log every accepted change in a version history /
migration note. Placed deliberately in the Publishing Pipeline rather
than `STANDING_PROTOCOL_v4.md`, since it governs how the architecture
evolves, not how research files are generated — Standing Protocol stays
scoped to research-generation rules only. No changes to Stage 1/2/3,
Risk Assessment, or the Headline Selection Rule.

Updates v1.2: adds a fourth Open Item ("Derived publication sections")
documenting that the "What we're watching next" and Risk Assessment
synthesis logic is now spec'd (inputs are defined) but not yet formalized
into actual generator logic. See `STANDING_PROTOCOL_v4.md` Rule 20's new
scope-clarification paragraph for the governing principle behind why "What
we're watching next" was resolved via synthesis rather than a new field.

Updates v1.1: (1) "What we're watching next" no longer sources from "Still
genuinely open" — that was never a defined field in
`DAILY_UPDATE_TEMPLATE.md`, only an informal section some real files used.
The generator now synthesizes this heading from Verdict, Thesis update,
Anything unscheduled, and upcoming scheduled events instead — all fields
that actually exist in the template. No new field added to the research
schema. (2) Risk Assessment sourcing is now split explicitly by
publication type (Daily Update vs. Weekly Outlook) and states each
publication's Risk Assessment draws only from that publication's own
evidence. "Correlation check" and "Zone alignment check" are Weekly
Outlook-only and are no longer listed as a generic/Daily Update source.

Updates v1.0: fixes two stale references — `STANDING_PROTOCOL_v3.md` →
`STANDING_PROTOCOL_v4.md`, `WEEKLY_RESEARCH_TEMPLATE_v2.md` →
`WEEKLY_RESEARCH_TEMPLATE_v3.md` — to match the current canonical set.
Adds an explicit citation linking the Publication Fidelity Rule to
`STANDING_PROTOCOL_v4.md` Rule 20 (they were conceptually aligned before
but never actually cross-referenced by rule number). No changes to Stage
1/2/3 structure, the Risk Assessment fields, or the Headline Selection
Rule.

This document governs how reader-facing publications (Daily Updates, Weekly
Outlooks, post-event analyses) are derived from HipMarvinFX's internal
research files. It does not change `STANDING_PROTOCOL_v4.md`,
`DAILY_UPDATE_TEMPLATE.md`, `WEEKLY_RESEARCH_TEMPLATE_v3.md`, or any other
research-file structure — those remain the source of truth, exactly as
defined elsewhere. This document governs only the translation layer that
sits downstream of them. Revisions to this document itself — as opposed to
the translation rules it defines — are governed by `EDITORIAL_STANDARD_v1_1.md`,
the same way any other canonical HipMarvin FX document's revisions are.

If a rule here ever conflicts with `STANDING_PROTOCOL_v4.md`, the protocol
document wins for anything touching the research file itself. This document
only has authority over the derived, reader-facing output.

---

## Three-Stage Architecture

### Stage 1 — Research File (Internal)

**Purpose:** capture every fact, correction, reconciliation, chart
observation, and audit trail, in full, exactly as it happened.

**Includes:**
- Event schedule
- Actual releases
- Chart observations (including app/source/timestamp metadata)
- Trade reconciliation
- Ledger notes
- Sanity checks (Rule 13)
- Corrections to earlier drafts or entries
- Parser metadata and field structure

**Audience:** the analyst, future audits, and AI reasoning over the file.
Never trimmed, never rewritten for tone. This is the permanent record.

---

### Stage 2 — Website Generator (Automatic)

The generator extracts only what a reader needs, mapped from specific
research-file fields to specific website headings. Nothing in this mapping
invents new content — every website field must trace back to something
already written in the research file.

**Daily Update → Daily article:**

| Website heading | Research-file source |
|---|---|
| Headline | Day Verdict + dominant catalyst of the day, selected via the Headline Selection Rule below when more than one candidate qualifies |
| Market takeaway | Verdict + Thesis update |
| What changed today | Event(s) today + Actual + Price note |
| Impact on our outlook | Trade Impact + Thesis Update (combined) |
| Risk Assessment | Editorial synthesis; inputs depend on publication type — see "Risk Assessment — Field Definitions" below for the Daily Update vs. Weekly Outlook source lists. |
| What we're watching next | Derived from unresolved questions surfaced within Verdict and Thesis update, plus Anything unscheduled that happened today and any upcoming scheduled events for the week. Not a separate research-file field — the generator synthesizes this from existing fields rather than reading a dedicated "still open" section, since no such field exists in `DAILY_UPDATE_TEMPLATE.md`. |
| Sources (optional) | Economic calendar reference + cited news + chart source, stripped of capture-time metadata |

**Weekly Outlook → Weekly Research Article** (new in v1.6 — closes the
Open Item that this table previously only covered Daily Update fields):

| Website heading | Research-file source |
|---|---|
| Headline | Event Label + Thematic Focus, or the single highest-impact Macro Driver if one clearly dominates the week |
| This week's view | Overall Bias + Macro Thesis, condensed |
| What's driving it | Macro Drivers (all, summarized — Tag/Headline/Analysis), in calendar order |
| Trade ideas | Trade Priority List, translated per the Website Experience Layers below (Dashboard + Trade Page) — never reproduced in raw Priority-N/dash-field form |
| Correlation and zone check | Correlation check (Rule 5) + Zone alignment check (Rule 21), translated to plain language |
| Institutional positioning | COT Positioning table, summarized in plain language |
| Risk Assessment | Editorial synthesis — see the Weekly Outlook source list under "Risk Assessment — Field Definitions" below |
| What we're watching this week | Daily Game Plan, condensed to forward-looking guidance only — never resolves anything, per Rule 6 |
| Sources (optional) | FF calendar reference + chart timestamps + COT report date, stripped of capture-time metadata |

---

### Stage 3 — Reader Output

The generator applies these transformations automatically. None of these
change what is concluded — only how it's presented.

**Remove:**
- Internal reconciliation notes (e.g. "corrects Wednesday's read")
- Parser/rule references (e.g. "per Rule 13," "STANDING_PROTOCOL")
- Chart-app names and capture timestamps, unless materially relevant to the reader
- Ledger-maintenance comments (e.g. notes aimed at a future admin import)
- Internal position labels ("Priority N") — replaced with plain pair/direction language

**Preserve exactly:**
- Every market conclusion
- Every trade conclusion
- Every risk warning
- Every forecast
- Every hedge word ("likely," "confirmed," "unconfirmed," "plausible") — carried over verbatim in meaning. The generator must never resolve a hedge into a certainty, and must never state as fact something the research file only estimated or inferred.

---

## Website Experience Layers (new in v1.5)

The site is not one translation target ("the article") — it's three, at
increasing depth, all sourced from the same Stage 1 Research File:

| Layer | Question it answers | Depth |
|---|---|---|
| **1 — Dashboard** | "Which trades look best this week, at a glance?" | 10-second scan |
| **2 — Trade Page** | "Should I actually take this specific idea?" | One pair, full context |
| **3 — Research Article** | "What's the full weekly/daily analysis?" | Full narrative — exactly the Stage 2/3 mapping above (Headline, Market Takeaway, What Changed Today, Impact on Our Outlook, Risk Assessment, What We're Watching Next); unchanged by this addition |

**Governing constraint, same as the rest of Stage 2:** nothing on any layer
invents content. Every Dashboard or Trade Page field must trace back to a
research-file field, exactly like the existing mapping table above.

### Layer 1 — Dashboard: "This Week's Best Setups"

Public-facing headers stay short, for a 10-second read — the fuller
internal terms (Setup Quality / Entry Quality / Suggested Action) are used
in the derivation logic below and on the Trade Page, not here:

| Pair | Setup | Entry | Action |
|---|---|---|---|
| NZD/USD | 🟢 Strong | 🔴 Buying High | 🟡 Wait for Pullback |
| GBP/USD | 🟢 Strong | 🔴 Buying High | 🟡 Wait for Pullback |
| USD/CAD | 🟡 Mixed | 🔴 Selling Low | 🔴 High Risk |
| EUR/USD | 🟡 Mixed | 🟡 Fair Value | 👀 Watch |
| USD/JPY | 🔴 Weak | 🔴 Selling Low | 🔴 Avoid |
| Gold (XAU/USD) | ⚪ Not Rated | 🟢 Good Buying Area* | ⚪ Insufficient Data |

*\*Entry is shown even though Setup is Not Rated — these are deliberately
independent fields; see Entry Quality Derivation below.*

**Short public disclaimer** (shown directly under the table):

> *Ratings reflect HipMarvinFX's assessment of current market conditions and
> are not guarantees of future performance. Full methodology available on
> request.*

This is a Dashboard-specific supplement — it doesn't replace the full Risk
Assessment Field Definitions disclaimer that still governs at the Research
Article layer; it exists at the point of least reader patience, where the
longer disclaimer won't be read anyway.

#### Setup, Entry, and Action — derivation logic

This is deterministic derivation logic, not editorial judgment applied
fresh each week. It lives here, in Stage 2, because these three outputs
are publication translation — never something the analyst narrates in
Stage 1. All inputs already exist in `WEEKLY_RESEARCH_TEMPLATE_v3.md`.

**Two defined COT states**, used below. Both are evaluated the same
way — Leveraged Funds and Asset Managers are treated as two independent
evidence sources, and support/opposition requires both to agree. This
replaces an earlier "dominant/majority category" formulation for COT
support, which never defined how to pick a dominant category and, given
how consistently larger Asset Manager position sizes run in the COT
Positioning table, would have made Asset Managers dominant by construction
rather than by genuine signal. See v1.8 changelog.
- **COT supports** the idea — both Leveraged Funds *and* Asset Managers
  are net positioned with the idea's direction, *and* both categories'
  week-on-week flow moved further in that direction.
- **COT actively opposes** the idea — a strict, deliberately narrow bar:
  both Leveraged Funds *and* Asset Managers are net positioned against the
  idea's direction, *and* both categories' week-on-week flow moved further
  against it. Cases where the two categories disagree with each other do
  **not** meet either bar — they fall through to Mixed COT (neither
  supports nor opposes) rather than being force-classified either way.

**Setup Quality Derivation Rule**
- 🟢 **Strong** — direction matches the Weekly Thesis, **and** (Tier 1
  **or** COT supports), **and** COT does **not** actively oppose, **and**
  no Correlation-check-flagged contradiction. *(The COT-actively-opposes
  guard is the fix for the USD/CAD bug: a Tier 1 catalyst alone can no
  longer outvote COT that is genuinely, two-sidedly against the trade.)*
- 🟡 **Mixed** — direction matches the Weekly Thesis, but doesn't clear the
  Strong bar (no Tier 1 and no clear COT support, or COT actively opposes
  without also matching the strict Weak conditions below).
- 🟡 **Contrarian** — direction runs against the Weekly Thesis, but COT
  supports it. Contrarian is reserved for ideas that oppose the thesis
  *and* have independent institutional evidence backing that override —
  not for every thesis-opposing idea.
- 🔴 **Weak** — one of two cases:
  - direction matches the Weekly Thesis, Tier 2, **and** COT actively
    opposes (the strict bar above); or
  - direction runs against the Weekly Thesis **and** is not supported by
    COT (this covers both Mixed COT and unanimous COT opposition against
    a thesis-opposing idea) — i.e. the idea has neither the Weekly
    Thesis nor independent institutional positioning behind it. *(Added
    v1.8 — previously unenumerated; see changelog.)*
- ⚪ **Not Rated** — Correlation Class is Proxy-standalone, or a required
  input (e.g. COT) is Pending/not sourced this cycle.

**Setup Quality decision table** (added v1.8, for unambiguous reference):

| Weekly Thesis | COT | Result |
|---|---|---|
| Matches | Supports, or Tier 1 with COT not opposing | Strong |
| Matches | Mixed, or opposes without Tier 1 | Mixed |
| Matches | Tier 2, and actively opposes | Weak |
| Against | Supports | Contrarian |
| Against | Mixed, or actively opposes | Weak |

**Entry Quality Derivation Rule**

Direct translation of Rule 21's Daily Zone label into reader language — no
new calculation, same number, same source, same Rule 21B logic. **Computed
independently of Setup Quality** — the fix for the Gold bug: a pair can be
Not Rated on Setup Quality while still carrying a real, chart-sourced Entry
Quality read, since Daily Zone doesn't depend on COT or thesis alignment
at all.

| Daily Zone (Rule 21) | Buy idea | Sell idea |
|---|---|---|
| Discount / Deep Discount (0–20%) | 🟢 Good Buying Area | 🔴 Selling Low |
| Lower Equilibrium / Equilibrium / Mild Premium (20–80%) | 🟡 Fair Value | 🟡 Fair Value |
| Premium / Deep Premium (80–100%) | 🔴 Buying High | 🟢 Good Selling Area |

For conditional (dual-trigger) ideas, Entry Quality is stated once per
triggered side, matching how Rule 21B already treats them individually. If
Daily Zone itself is unavailable, Entry Quality is "Not Rated," independent
of whatever Setup Quality shows.

**Suggested Action Derivation Rule**

Mechanical combination of the two fields above — not independent editorial
judgment, so it stays inside the Publication Fidelity Rule's
translation-only boundary:

| Suggested Action | Meaning | Triggered when |
|---|---|---|
| 🟢 Favorable | Strong idea, acceptable entry | Setup = Strong, Entry = Fair Value or Good Buying/Selling Area |
| 🟡 Wait for Pullback | Strong idea, expensive entry | Setup = Strong, Entry = Buying High / Selling Low |
| 👀 Watch | Worth monitoring, not ready | Setup = Mixed or Contrarian, Entry = Fair Value or Good Buying/Selling Area |
| 🔴 High Risk | Several conflicting factors | Setup = Mixed or Contrarian, Entry = Buying High / Selling Low |
| 🔴 Avoid | Weak overall setup | Setup = Weak (regardless of Entry) |
| ⚪ Insufficient Data | Evidence incomplete | Setup = Not Rated, or Entry unavailable |

**Status of these three outputs:** editorial synthesis, not market data —
same category as Risk Assessment, under the Field Definitions below.

**Optional future enhancement, not adopted:** a star-rating overlay
(★★★★★–★) as a faster-to-scan alternative to color/label badges — pure
presentation choice, no new logic. Left open; the two external reviews
disagreed on whether it improves on labeled badges.

### Layer 2 — Trade Page (one page per pair)

Badges at top for a fast read, detail below for anyone who wants it. Here,
the fuller internal terms are used since there's room for them:

```
## GBP/USD Long
🟢 Setup Quality: Strong    🔴 Entry Quality: Buying High    🟡 Suggested Action: Wait for Pullback

### Why we like it
[bulleted, from Reasoning]

### Main risks
[bulleted, from Reasoning — Rule 21B / Rule 5 content, never omitted]

### Trading Plan
Entry / Stop / Target 1 / Target 2

### Next Event
[tagged Macro Driver or Scenario Matrix]
```

| Website field | Research-file source |
|---|---|
| Setup Quality / Entry Quality / Suggested Action badges | Same derived values as the Dashboard row for this pair — computed once per the derivation logic above, reused, never re-derived differently per page |
| Why we like it | The idea's own Reasoning field (Trade Priority List), translated to reader language and reformatted as bullets, "Priority N" language stripped per the existing Stage 3 rules above |
| Main risks | Same Reasoning field — specifically any Rule 21B conflict and/or Rule 5 correlation-check content that applies to that idea, plus the Tier-2 "no catalyst" caveat where relevant. **Never omitted for brevity — same hard requirement as the "Preserve exactly" list above.** |
| Trading Plan (Entry / Stop / TP1 / TP2) | Trade Priority List — carried verbatim; these are market-data fields, not editorial, and are preserved exactly per the Publication Fidelity Rule |
| Next Event | Whichever Macro Driver is tagged to that pair, or the relevant Scenario Matrix if the idea is directly tied to a red-folder event |
| Latest update | That pair/direction's most recent Daily Update "Trade impact" entry, once the week is underway |

---

## Risk Assessment — Field Definitions

The Risk Assessment section is an **analyst interpretation, not a market
data field.** It has no chart or calendar source the way a price level or
an actual release does — it is the analyst's synthesis of the evidence
available at the time that specific publication is generated, and must be
labeled as such every time it appears.

**Risk Assessment is generated independently for each publication, using
only the evidence available up to that publication** — a Daily Update's
Risk Assessment does not reach into that week's Weekly Outlook for fields
that aren't part of a daily entry, and vice versa. Sourcing differs by
publication type:

**For Daily Updates**, derive Risk Assessment from:
- Verdict
- Thesis update
- Trade impact
- Anything unscheduled that happened today
- Unresolved risks (per the "What we're watching next" synthesis above)

**For Weekly Outlooks**, derive Risk Assessment from:
- Weekly Thesis
- Correlation check (Rule 5)
- Zone alignment check (Rule 21)
- Scenario Matrix
- Trade Priority List
- Unresolved risks

`Correlation check` and `Zone alignment check` only exist in the Weekly
Outlook's Trade Priority List — never list them as a Daily Update source,
since no Daily Update field carries that content.

| Field | Definition |
|---|---|
| Bias | Current directional view based on the balance of evidence. |
| Confidence | Analyst's confidence in that bias (Low / Moderate / High). Qualitative, not statistical. |
| Risk Level | Expected market volatility or uncertainty (Low / Moderate / High). |
| Primary Driver | The single factor currently influencing the market most. |
| Key Risk | The event or development most likely to invalidate the current view. |

**Required disclaimer** (one line, every time the section appears in a
published article):

> *This section reflects HipMarvinFX's editorial assessment based on
> available market evidence. It is not a statistical probability or
> consensus forecast.*

This table itself belongs in this methodology document, not repeated
inside individual articles — articles carry only the one-line disclaimer
above.

Additional qualitative fields may be added in future revisions provided
they remain explicitly identified as editorial assessment rather than
market data.

---

## Division of Responsibility

- **Research File = the source of truth.** Nothing is ever cut from it for
  readability; corrections and reconciliations stay visible permanently.
- **Website Article = the reader-facing narrative.** Same conclusions, same
  analysis, no loss of accuracy — different audience, different voice
  (MarvinX voice, third-person, publication register), different level of
  operational detail.
- **The generator/parser = a translation layer, not an editor.** It maps
  and rephrases; it does not introduce new judgment, resolve hedges into
  certainties, or drop a risk warning for length.

---

## External Review Intake

**Relationship to `EDITORIAL_STANDARD_v1_1.md` (added v1.7):** this section
governs *whether* an externally-sourced recommendation should be acted on.
Once a recommendation is accepted, the resulting edit — to this document
or any other canonical file — is produced by following the Editorial
Standard's Editorial Review Workflow (Steps 1–6) and Editorial Decision
Hierarchy. The two are complementary, not overlapping: this section is
scoped specifically to externally-sourced recommendations; the Editorial
Standard governs all revisions, regardless of source.

External recommendations are treated as review inputs, not as authoritative
changes to the HipMarvinFX methodology. Their value is determined by
verification against the canonical document set, not by repetition or
presentation quality.

For every substantive recommendation:

1. **Trace the claim to the source.**
   - Identify the document(s), section(s), or rule(s) that support or
     contradict the recommendation.
   - Where practical, verify against the exact text rather than a
     paraphrase.
2. **Separate conclusion from justification.**
   - A recommendation may reach the correct conclusion for the wrong
     reason.
   - Evaluate the proposed change independently from the explanation
     offered for it.
3. **Check architectural consistency.**
   - Confirm that the recommendation is consistent with the overall
     separation between:
     - Standing Protocol
     - Research Templates
     - Generation Prompts
     - Publishing Pipeline
   - A local improvement must not introduce a contradiction elsewhere.
4. **Determine whether the issue is genuinely new.**
   - Distinguish between:
     - a newly discovered design issue,
     - clarification of an existing principle,
     - or repetition of a previously accepted recommendation.
5. **Record accepted changes.**
   - Every accepted structural change should be reflected in the
     appropriate document(s), version history, and migration notes where
     applicable.
   - Undocumented schema or methodology changes are not permitted.

### Reviewer Independence

Agreement between multiple reviews may increase confidence that a design
issue exists, but it does not by itself validate the reasoning used to
reach that conclusion.

Convergence should therefore be treated as evidence that a recommendation
deserves verification, not as evidence that it is correct.

Every recommendation should be re-derived from the canonical documents
before acceptance, regardless of its source or how many times it has been
repeated.

### Document Consolidation Principle

New documents should not be introduced solely for organizational
preference. A section stays inside its parent document unless one or more
objective triggers occur:

- it needs to serve multiple independent systems (e.g. the same
  derivation logic consumed by a website, a mobile app, an email digest,
  and an API, not just one target),
- it grows substantially larger than the surrounding sections in its
  parent document,
- it develops its own independent version lifecycle, separate from the
  document it currently lives in, or
- keeping it in the parent document is causing demonstrable
  synchronization problems — not hypothetical ones.

Until one of these is actually met, consolidation is preferred over
decomposition. This isn't a bias against ever splitting a document — it's
a default that requires a concrete trigger before adding a new file,
rather than re-opening the question from scratch every time a review
suggests it would be "cleaner." The Setup/Entry/Action derivation logic's
standing extraction criterion (see Layer 1, above) is the worked example
this principle generalizes from: a plausible single-responsibility
argument exists for splitting it into its own document, but no trigger
has actually fired, so the split stays deferred and documented rather
than executed.

---

## Analytical Depth Rule (governs Stage 1 — Research File generation)

Within each existing research-file field, the model should maximize
explanatory value without increasing evidentiary certainty.

Specifically:

- Explain why the observed event matters to the market.
- Explain how today's developments affect the existing thesis.
- Explain what changes for the next trading session.
- Strengthen causal reasoning where supported by the evidence.
- Preserve all confidence labels and hedge language exactly as supported by
  the research file.
- Never introduce stronger conclusions than the underlying evidence
  justifies.

This rule operates entirely inside existing fields — it does not add,
remove, rename, or reorder anything. Depth is a writing-quality target, not
a license to assert more than the sourced facts and prior hedges support.
An analysis that reads more thoroughly reasoned than yesterday's but
concludes something the price/data doesn't actually support has failed
this rule, not satisfied it. Greater depth should come from connecting
evidence already present in the research file, not from introducing
additional assumptions — that distinction is what separates reasoning from
speculation.

---

## Publication Fidelity Rule (governs Stage 2/3 — Website Generator; the
downstream counterpart to `STANDING_PROTOCOL_v4.md` Rule 20 — Rule 20
governs research-file integrity, this rule governs translation integrity)

The publication layer may improve clarity, narrative flow, and analytical
explanation, but it must not:

- introduce new facts,
- omit material risk warnings,
- alter the confidence level of conclusions, or
- change the substantive meaning of the underlying research.

This is the formal version of the generator's role described earlier in
this document ("a translation layer, not an editor"): it distinguishes
**style improvements** (rewording, restructuring, removing internal
metadata) from **analytical alterations** (anything that would change what
a reader concludes). Style improvements are the generator's job.
Analytical alterations are never the generator's job — if the underlying
research needs to change, that happens in the Research File, at the
source, not in the derived article.

Any output that fails this rule — a hedge quietly firmed up, a risk
warning dropped for brevity, a conclusion sharpened past what the research
file actually supports — is a fidelity failure, not a style choice, and
should be treated as a bug in the generator, not a matter of editorial
taste. If the generator cannot faithfully express a conclusion without
materially changing its meaning, it must preserve the original wording
rather than paraphrase it.

---

Together, the Analytical Depth Rule and Publication Fidelity Rule define
the standard for HipMarvinFX publications: explain as deeply as the
evidence permits, but never with greater certainty than the evidence
supports. Richer reasoning is encouraged; stronger claims are not.

---

## Headline Selection Rule

When a day has more than one candidate for "dominant catalyst," the
headline is chosen by this hierarchy, in order, stopping at the first rule
that resolves it:

1. Systemic macro catalyst (Fed, CPI, NFP, GDP, central bank decisions)
2. Cross-market catalyst affecting multiple assets
3. Pair-specific catalyst
4. Technical catalyst
5. If two candidates are still equal after 1–4, choose the one that most
   changed the weekly thesis.

This removes subjective judgment from headline selection on days with
competing stories (e.g. Day 4's BOE/GDP/PCE story vs. the USD/JPY
intervention story) — rule 2 (cross-market catalyst) would have resolved
that specific case in favor of the broad-dollar BOE/GDP/PCE story, since
it affected six pairs, with the USD/JPY intervention covered prominently
in the body of the article rather than the headline.

---

## Open Items (not yet resolved by this document)

- **Sources section** — listed as optional above; not yet decided whether
  published articles should include it by default or only on request.
- **Where this pipeline itself lives** — this document doesn't yet have a
  parser/code implementation, matching the same "spec written, not yet
  built" status as other items flagged in `HANDOVER_NOTE.md`. Treat this as
  a methodology reference for now, not a working automation.
- ~~Stage 2's mapping table only covers Daily Update fields~~ — **resolved
  for Weekly Outlook by the new mapping table above (v1.6).** Post-event
  analyses still have no defined mapping — this document's opening line
  claims scope over all three publication types, but only two (Daily
  Update, Weekly Outlook) have a table.
- ~~Derived publication sections — generator logic not formalized~~ —
  **resolved for Setup / Entry / Action**, which are now fully
  deterministic. "What we're watching next" (Daily Update) remains an
  Open Item — its synthesis logic is still input-defined but not yet
  reduced to a rule the same way.
- **`HipMarvinFX_Generation_Prompts_v4.md` consistency** — checked against
  v4.6 (current at time of this revision) and confirmed to contain no
  conflicting Dashboard/Setup Quality logic, since that document governs
  the three research-file generation prompts only. Resolved for now; worth
  re-checking on any future Generation Prompts version bump that touches
  Weekly Outlook structure.
- **Alternative star-rating ("Opportunity") scheme — considered and
  rejected, not left open.** An external review proposed collapsing Setup
  Quality and Entry Quality into a single gated Opportunity score. Found
  to contain an internal contradiction (see v1.6 changelog above) and not
  adopted. Documented here so it isn't independently re-proposed and
  re-evaluated from scratch later without this finding attached.

---

END DOCUMENT
