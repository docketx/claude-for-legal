---
name: divorce-in-nebraska
description: >
  Routes a Nebraska divorce question to the right row in the verified reference and renders the
  answer with its verbatim statutory (or, for the marital-property classification rule, case-law)
  quote and source tag. Covers residency and venue, grounds, the 60-day pre-hearing wait and the
  decree's finality dates (including the six-month remarriage bar), marital versus nonmarital
  property, the division standard, alimony, the child-support-guidelines delegation, custody and
  parenting time under the Parenting Act, the mandatory parenting education course, mediation,
  required financial disclosures, temporary and ex parte orders, property settlement agreements,
  and name restoration. Use when the user asks "can I file for divorce in Nebraska", "how long do
  I have to live here first", "is Nebraska a no-fault state", "how is property split in a Nebraska
  divorce", "do I qualify for alimony", "how much child support", "who gets custody", "do I have to
  take a parenting class", "how long does a Nebraska divorce take", or any other Nebraska divorce
  question. Does not cover any state other than Nebraska, and does not answer protective-order or
  safety-planning questions.
argument-hint: '[a Nebraska divorce question, plain text]'
---

# /divorce-in-nebraska

Answers a Nebraska divorce question from the verified reference at
`../../references/rows-ne-family.json`, never from memory alone. Read
`../../CLAUDE.md` before running this skill -- it has the source-tag vocabulary, the jurisdiction
gate, the family-violence caveat, and the honest-negative rule this skill depends on.

## Instructions

1. **Family-violence check, first, before anything else.** If the question is actually about a
   protective order, safety planning, or "am I safe" -- not a divorce mechanics question that
   happens to mention violence -- stop and follow the family-violence caveat in `../../CLAUDE.md`
   verbatim. Do not proceed to step 2.

2. **Jurisdiction check.** If the facts don't establish Nebraska (no Nebraska county, court, or
   explicit statement), ask. If another state is named -- including any other state this plugin
   covers -- follow the jurisdiction gate in `../../CLAUDE.md` and stop. Never answer a Nebraska
   question from another state's rows or another state's question from the Nebraska rows; Nebraska
   differs from this plugin's other states on nearly every clause it covers (a marital-property
   regime built entirely from case law rather than a statutory community-property or marital-property
   definition, a "reasonable"/"general rule" division standard rather than a fixed percentage or a
   discretionary multi-factor label, a child-support formula delegated to Nebraska Supreme Court rule
   rather than written into the statute, a mandatory basic-level parenting education course, and more)
   -- so do not assume Nebraska's rule by analogy to another state even where the topic looks the
   same.

3. **Load the rows.** Read `../../references/rows-ne-family.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority`
   (an array of statute or case quotes with `verified.corpus` blocks), `status`, and `notes`.

4. **Route the question to the matching row(s) by `clause`.** The clauses currently in the
   reference:

   | User is asking about | `clause` value |
   |---|---|
   | How long you must live in Nebraska / which county to file in | `residency-and-venue` |
   | Whether fault has to be proven; what the actual ground is | `grounds` |
   | How soon after service the case can be heard, decreed, and become final (including the six-month remarriage bar) | `waiting-period` |
   | Whether a given asset is separate ("nonmarital") or part of the marital estate | `marital-vs-non-marital-property` |
   | How the court splits the marital estate; is there a fixed percentage | `division-standard` |
   | Alimony: what the court weighs, and when it ends | `spousal-support` |
   | How child support is calculated, and whether the guideline table itself is in this reference | `child-support-guidelines` |
   | Custody, parenting time, is there a joint-custody presumption | `custody-and-visitation` |
   | Is a parenting class required | `parenting-course` |
   | Is mediation required for a custody/parenting-time dispute | `mediation` |
   | What has to be disclosed during the case | `required-disclosures` |
   | What a court can order while the case is pending; ex parte orders | `temporary-orders` |
   | Can spouses agree to their own property/support/custody terms; is the court bound by it | `settlement-agreements` |
   | Changing a name back in the divorce | `name-restoration` |

   A single question can touch more than one clause (e.g., "how long will my divorce take and what
   will I get" touches `waiting-period`, `division-standard`, and possibly `spousal-support`). Route
   to every clause the question actually asks about; don't force a multi-part question into one row.

   If the question doesn't match any `clause` in the table, say so directly: "That's not one of the
   topics this reference currently covers for Nebraska family law -- [state what was searched and
   what wasn't found, per the honest-negative rule in CLAUDE.md]," rather than answering from
   general knowledge under this skill.

5. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question` in one or two sentences -- the `rule` field
     is already written this way ("One year of actual Nebraska residence... OR ... a marriage that
     was solemnized in Nebraska...") -- lead with that, not with throat-clearing.
   - Follow with the supporting text: quote directly from `authority[].verified.corpus.quote`, cite
     `authority[].cite`, and tag it `[DocketRouter — verified]`. Most Nebraska rows quote a statute;
     the `marital-vs-non-marital-property` row and part of `division-standard` quote Nebraska
     Supreme Court opinions instead (Brozek v. Brozek and Millatmal v. Millatmal) because Chapter 42's
     text never defines "marital property" or states a fixed division percentage -- say so when the
     authority is a case rather than a statute, since a reader who assumes every Nebraska family-law
     rule is in the code would be wrong on these two rows specifically.
   - If the row has more than one authority, use as many as are relevant to the specific question
     asked -- don't dump every authority on the row if the user asked a narrower question, but don't
     omit an authority that changes the answer (e.g., the six-month remarriage-finality date in
     `waiting-period`, or the custody/support carve-out from the binding-agreement rule in
     `settlement-agreements`).
   - Read `notes` and carry forward any scope limit it records (an honest negative, an excluded
     cross-reference, a task-framing correction, an unresolved question, an OCR/line-wrap artifact in
     a quoted case) as part of the answer, not as a buried caveat.
   - After the rule, surface the row's `trap` field as a distinct "what people get wrong" note --
     several Nebraska traps are specifically about NOT reading a "general rule" or a percentage
     range as a hard ceiling (`division-standard`'s one-third-to-one-half range, affirmed even where
     the actual award was 57/43), about a statutory exception hiding inside the same sentence as the
     main rule (`custody-and-visitation`'s "except as provided in section 43-2933"), or about a duty
     that looks general but is actually narrower than it first appears (`required-disclosures`'s
     support-application-only trigger) -- this is the load-bearing misconception-correction content
     of the row and should not be dropped for brevity.

6. **If the corpus doesn't answer part of the question**, say exactly that, using the pattern from
   the honest-negative rule in `../../CLAUDE.md` -- what was searched, what wasn't found -- instead of
   filling the gap with `[model knowledge — verify]` prose that reads as equally authoritative as
   the verified part of the answer. Several rows in this reference already record honest negatives
   this way (no fault ground found in §§ 42-347/42-361; the actual child-support guideline
   worksheet is set by Nebraska Supreme Court rule, not in this statutes shard; no dollar or
   percentage cap on alimony found in § 42-365; whether § 42-380 reaches a legal-separation
   pleading is not established because § 42-353 was not read in full) -- follow the same pattern
   for a new gap rather than guessing.

7. **Close every substantive answer** with the draft-for-attorney-review line from
   `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim
statutory or case-law quote and `[DocketRouter — verified]` tag, the trap/misconception note, any
honest negative the corpus requires, and the draft-for-attorney-review line. Not a memo, not a
multi-page review -- sized to the question asked.
