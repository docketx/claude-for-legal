---
name: divorce-in-texas
description: >
  Routes a Texas divorce question to the right row in the verified reference and renders the
  answer with its verbatim statutory quote and source tag. Covers residency and venue, grounds,
  the waiting period, community versus separate property, the division standard, spousal
  maintenance eligibility and amount/duration, child support guidelines, conservatorship and
  possession, temporary orders, mediation and settlement agreements, and name restoration. Use
  when the user asks "can I file for divorce in Texas", "how is property split", "do I qualify
  for alimony", "how much child support", "who gets custody", "how long does a Texas divorce
  take", or any other Texas divorce question. Does not cover any state other than Texas, and does
  not answer protective-order or safety-planning questions.
argument-hint: '[a Texas divorce question, plain text]'
---

# /divorce-in-texas

Answers a Texas divorce question from the verified reference at
`../../references/rows-tx-family.json`, never from memory alone. Read
`../../CLAUDE.md` before running this skill — it has the source-tag vocabulary, the jurisdiction
gate, the family-violence caveat, and the honest-negative rule this skill depends on.

## Instructions

1. **Family-violence check, first, before anything else.** If the question is actually about a
   protective order, safety planning, or "am I safe" — not a divorce mechanics question that
   happens to mention violence — stop and follow the family-violence caveat in `../../CLAUDE.md`
   verbatim. Do not proceed to step 2.

2. **Jurisdiction check.** If the facts don't establish Texas (no Texas county, court, or explicit
   statement), ask. If another state is named, follow the jurisdiction gate in `../../CLAUDE.md`
   and stop — do not answer a non-Texas question from this skill's rows.

3. **Load the rows.** Read `../../references/rows-tx-family.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority`
   (an array of statute or case quotes with `verified.corpus` blocks), `status`, and `notes`.

4. **Route the question to the matching row(s) by `clause`.** The clauses currently in the
   reference:

   | User is asking about | `clause` value |
   |---|---|
   | How long you must live in Texas / the county before filing | `residency-and-venue` |
   | Whether fault has to be proven; no-fault vs. fault grounds | `grounds` |
   | How soon after filing the divorce can be granted | `waiting-period` |
   | Whose property is whose; what counts as community property | `community-vs-separate-property` |
   | How the court splits the estate; is it always 50/50 | `division-standard` |
   | Who qualifies for spousal maintenance | `spousal-maintenance-eligibility` |
   | How much maintenance, and for how long | `spousal-maintenance-amount-and-duration` |
   | How child support is calculated | `child-support-guidelines` |
   | Custody, conservatorship, possession/visitation schedules | `conservatorship-and-possession` |
   | What a court can order while the case is pending | `temporary-orders` |
   | Is a financial inventory automatically required | `inventory-and-appraisement` |
   | What has to be disclosed during the case | `required-disclosures` |
   | Mediation, settlement agreements, can I back out | `mediation-and-agreements` |
   | Changing a name back in the divorce | `name-restoration` |

   A single question can touch more than one clause (e.g., "how long will my divorce take and
   what will I get" touches `waiting-period`, `division-standard`, and possibly
   `spousal-maintenance-eligibility`). Route to every clause the question actually asks about;
   don't force a multi-part question into one row.

   If the question doesn't match any `clause` in the table, say so directly: "That's not one of
   the topics this reference currently covers for Texas family law — [state what was searched and
   what wasn't found, per the honest-negative rule in CLAUDE.md]," rather than answering from
   general knowledge under this skill.

5. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question` in one or two sentences — the `rule`
     field is already written this way ("Six months in Texas and 90 days in the county, by
     either spouse.") — lead with that, not with throat-clearing.
   - Follow with the supporting statutory text: quote directly from `authority[].verified.corpus.quote`,
     cite `authority[].cite`, and tag it `[DocketRouter — verified]`.
   - If the row has more than one authority, use as many as are relevant to the specific question
     asked — don't dump every authority on the row if the user asked a narrower question, but
     don't omit an authority that changes the answer (e.g., the family-violence exception to the
     waiting period).
   - Read `notes` and carry forward any scope limit it records (an honest negative, an excluded
     cross-reference, an unresolved question) as part of the answer, not as a buried caveat.
   - After the rule, surface the row's `trap` field as a distinct "what people get wrong" note —
     this is the load-bearing misconception-correction content of the row and should not be
     dropped for brevity.

6. **If the corpus doesn't answer part of the question**, say exactly that, using the pattern from
   the honest-negative rule in `../../CLAUDE.md` — what was searched, what wasn't found — instead
   of filling the gap with `[model knowledge — verify]` prose that reads as equally authoritative
   as the verified part of the answer.

7. **Close every substantive answer** with the draft-for-attorney-review line from
   `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim
statutory quote and `[DocketRouter — verified]` tag, the trap/misconception note, any honest
negative the corpus requires, and the draft-for-attorney-review line. Not a memo, not a multi-page
review — sized to the question asked.
