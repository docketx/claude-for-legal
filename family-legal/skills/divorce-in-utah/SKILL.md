---
name: divorce-in-utah
description: >
  Routes a Utah divorce question to the right row in the verified reference and renders the
  answer with its verbatim statutory (or, where no statute exists, case-law) quote and source tag.
  Covers residency and venue, the ten fault-and-no-fault grounds including the insanity ground's
  guardian ad litem safeguards, the 30-day waiting period, marital versus separate property under
  Utah's judge-made (non-community-property) rule, the "fair, just, and equitable" division
  standard, alimony factors and the length-of-marriage cap, the child support guideline's
  rebuttable presumption, custody and parent-time (including the joint-legal-custody presumption
  that does NOT extend to joint physical custody), the mandatory mediation program, temporary
  orders and the standalone temporary-separation track, what Utah does and does not require by way
  of financial disclosure, settlement stipulations, name restoration, and Utah's own mandatory
  divorce orientation course. Use when the user asks "can I file for divorce in Utah", "how is
  property split in Utah", "do I qualify for alimony in Utah", "how much child support in Utah",
  "who gets custody in Utah", "how long does a Utah divorce take", "is Utah a community property
  state", "do I have to take a divorce class in Utah", or any other Utah divorce question. Does not
  cover any state other than Utah, and does not answer protective-order or safety-planning
  questions.
argument-hint: '[a Utah divorce question, plain text]'
---

# /divorce-in-utah

Answers a Utah divorce question from the verified reference at
`../../references/rows-ut-family.json`, never from memory alone. Read
`../../CLAUDE.md` before running this skill — it has the source-tag vocabulary, the jurisdiction
gate, the family-violence caveat, and the honest-negative rule this skill depends on.

## Instructions

1. **Family-violence check, first, before anything else.** If the question is actually about a
   protective order, safety planning, or "am I safe" — not a divorce mechanics question that
   happens to mention violence or abuse — stop and follow the family-violence caveat in
   `../../CLAUDE.md` verbatim. Do not proceed to step 2. This matters for Utah specifically: the
   `custody-and-visitation` row's best-interest factors, the `grounds` row's cruelty ground, and
   the `division-standard`/`marital-vs-non-marital-property` rows' underlying case law all mention
   domestic violence or abuse in passing because the statute or a quoted opinion does, but none of
   them is a substitute for the caveat's routing, and none should be used to build safety guidance.

2. **Jurisdiction check.** If the facts don't establish Utah (no Utah county or district court, no
   explicit statement), ask. If another state is named — including Texas, California, Washington,
   Illinois, Iowa, or Virginia, which this plugin also covers — follow the jurisdiction gate in
   `../../CLAUDE.md` and stop. Never answer a Utah question from another state's rows, or a
   question about another state from the Utah rows; Utah disagrees with at least one other covered
   state on nearly every clause here (Utah keeps nine fault grounds alongside "irreconcilable
   differences" where California and Washington have none; Utah is NOT a community-property state
   and has no statute at all defining marital versus separate property, unlike California's or
   Washington's codified rules; Utah caps alimony at the length of the marriage where California
   keeps jurisdiction open indefinitely past 10 years; Utah presumes joint LEGAL custody but not
   joint PHYSICAL custody, a split neither California's nor Virginia's rows draw the same way; Utah
   requires a mandatory divorce orientation course with no counterpart anywhere else in this
   plugin) — do not assume any state's rule carries over by analogy, even where the topic looks the
   same.

3. **Load the rows.** Read `../../references/rows-ut-family.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority` (an
   array of statute or case quotes with `verified.corpus` blocks), `status`, and `notes`.

4. **Route the question to the matching row(s) by `clause`.** The clauses currently in the
   reference:

   | User is asking about | `clause` value |
   |---|---|
   | How long you must live in the county before filing; is there a separate statewide residency test | `residency-and-venue` |
   | Whether fault has to be proven; what the ten grounds actually are | `grounds` |
   | How soon after filing the decree can enter | `waiting-period` |
   | Whose property is whose; is Utah a community-property state | `marital-vs-non-marital-property` |
   | How the court splits the marital estate; is it a fixed percentage | `division-standard` |
   | What factors govern alimony; how long can it last; the 10-year equalization presumption | `spousal-support` |
   | Is the child support guideline amount mandatory; when can a court deviate | `child-support-guidelines` |
   | Custody, parent-time, is there a presumption toward joint custody; the minimum parent-time schedule | `custody-and-visitation` |
   | What has to be disclosed during the case | `required-disclosures` |
   | What a court can order while the case is pending; getting temporary orders without filing for divorce | `temporary-orders` |
   | Is mediation required | `mediation` |
   | Can spouses agree to their own terms; will a stipulation be enforced | `settlement-agreements` |
   | Changing a name back after the divorce | `name-restoration` |
   | Is there a mandatory class or orientation course before the divorce can proceed | `divorce-orientation-course` (Utah-only; see `../../CLAUDE.md`'s jurisdiction-gate note on single-state topics) |

   A single question can touch more than one clause (e.g., "how long will my Utah divorce take and
   what will I get" touches `waiting-period`, `division-standard`, and possibly `spousal-support`).
   Route to every clause the question actually asks about; don't force a multi-part question into
   one row.

   If the question doesn't match any `clause` in the table, say so directly: "That's not one of the
   topics this reference currently covers for Utah family law — [state what was searched and what
   wasn't found, per the honest-negative rule in CLAUDE.md]," rather than answering from general
   knowledge under this skill.

5. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question` in one or two sentences — the `rule` field
     is already written this way ("Ninety days of county residency by the petitioner OR the
     respondent...") — lead with that, not with throat-clearing.
   - Follow with the supporting text: quote directly from `authority[].verified.corpus.quote`, cite
     `authority[].cite`, and tag it `[DocketRouter — verified]`. Most Utah authorities are
     statutes, but `marital-vs-non-marital-property`, `division-standard`'s judicial-standard
     quote, `required-disclosures`, and `settlement-agreements` are built from **case law**
     (`authority[].kind` = `"case"`, dataset `docketx/us-caselaw-ut`) because no Utah statute
     answers those questions — say so plainly rather than presenting a case holding as if it were a
     statutory rule.
   - If the row has more than one authority, use as many as are relevant to the specific question
     asked — don't dump every authority on the row if the user asked a narrower question, but don't
     omit an authority that changes the answer (e.g., the length-of-marriage cap's own exception in
     `spousal-support`, or the joint-legal-custody presumption's four carve-outs in
     `custody-and-visitation`).
   - Read `notes` and carry forward any scope limit it records (an honest negative, an excluded
     cross-reference, an unresolved question) as part of the answer, not as a buried caveat. The
     `required-disclosures` row's `notes` in particular flag that Utah R. Civ. P. 26.1 — the actual
     financial-disclosure rule — is quoted there from `docketx/court-rules` for transparency but is
     **not** a `verified.corpus` authority and must **not** be rendered with the
     `[DocketRouter — verified]` tag; use `[unverified]` and say it came from the court-rules
     dataset if you mention its specifics.
   - After the rule, surface the row's `trap` field as a distinct "what people get wrong" note —
     several Utah traps are specifically about not assuming another covered state's rule carries
     over (nine surviving fault grounds, no statutory property-classification rule at all, a hard
     alimony ceiling where California has none, a presumption that applies to legal custody but not
     physical custody) — this is the load-bearing misconception-correction content of the row and
     should not be dropped for brevity.

6. **If the corpus doesn't answer part of the question**, say exactly that, using the pattern from
   the honest-negative rule in `../../CLAUDE.md` — what was searched, what wasn't found — instead of
   filling the gap with `[model knowledge — verify]` prose that reads as equally authoritative as
   the verified part of the answer. Several rows in this reference already record honest negatives
   this way (no Title 81 statute defines marital versus separate property or the division standard
   — both are case law; no dedicated settlement-agreement statute was found, only one narrow
   appellate application; no divorce-decree name-restoration statute exists, only the general
   Title 42 change-of-name petition) — follow the same pattern for a new gap rather than guessing.

7. **Close every substantive answer** with the draft-for-attorney-review line from
   `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim quote
and `[DocketRouter — verified]` tag (or `[unverified]` for the one court-rules mention in
`required-disclosures`), the trap/misconception note, any honest negative the corpus requires, and
the draft-for-attorney-review line. Not a memo, not a multi-page review — sized to the question
asked.
