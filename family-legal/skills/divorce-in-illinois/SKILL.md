---
name: divorce-in-illinois
description: >
  Routes an Illinois divorce question to the right row in the verified reference and renders the
  answer with its verbatim statutory quote and source tag. Covers residency and venue, grounds,
  the waiting period, marital versus non-marital property, the division standard, maintenance
  (eligibility, guideline formula, and duration), child support guidelines, allocation of parental
  responsibilities and parenting time, the financial affidavit and disclosure, temporary relief,
  mediation, marital settlement agreements, name resumption, and joint simplified dissolution. Use
  when the user asks "can I file for divorce in Illinois", "how is property split", "do I qualify
  for maintenance", "how much child support", "who gets decision-making", "how long does an
  Illinois divorce take", "can we do a simplified divorce", or any other Illinois divorce question.
  Does not cover any state other than Illinois, and does not answer protective-order or
  safety-planning questions.
argument-hint: '[an Illinois divorce question, plain text]'
---

# /divorce-in-illinois

Answers an Illinois divorce question from the verified reference at
`../../references/rows-il-family.json`, never from memory alone. Read
`../../CLAUDE.md` before running this skill — it has the source-tag vocabulary, the jurisdiction
gate, the family-violence caveat, and the honest-negative rule this skill depends on.

## Instructions

1. **Family-violence check, first, before anything else.** If the question is actually about a
   protective order, safety planning, or "am I safe" — not a divorce mechanics question that
   happens to mention violence — stop and follow the family-violence caveat in `../../CLAUDE.md`
   verbatim. Do not proceed to step 2.

2. **Jurisdiction check.** If the facts don't establish Illinois (no Illinois county, court, or
   explicit statement), ask. If another state is named, follow the jurisdiction gate in
   `../../CLAUDE.md` and stop — do not answer a non-Illinois question from this skill's rows.

3. **Load the rows.** Read `../../references/rows-il-family.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority`
   (an array of statute or case quotes with `verified.corpus` blocks), `status`, and `notes`.

4. **Route the question to the matching row(s) by `clause`.** The clauses currently in the
   reference:

   | User is asking about | `clause` value |
   |---|---|
   | How long you must be connected to Illinois / which county | `residency-and-venue` |
   | Whether fault has to be proven; irreconcilable differences | `grounds` |
   | How soon after filing the divorce can be granted | `waiting-period` |
   | Whose property is whose; what counts as marital property | `marital-vs-non-marital-property` |
   | How the court splits the estate; is it always 50/50 | `division-standard` |
   | Who qualifies for maintenance, how much, and for how long | `maintenance` |
   | How child support is calculated | `child-support-guidelines` |
   | Decision-making, parenting time, parenting plans | `allocation-of-parental-responsibilities-and-parenting-time` |
   | What has to be disclosed; the financial affidavit | `financial-affidavit-and-disclosure` |
   | What a court can order while the case is pending | `temporary-relief` |
   | Is mediation required | `mediation` |
   | Marital settlement agreements, can I back out | `marital-settlement-agreements` |
   | Resuming a former or maiden name in the divorce | `name-resumption` |
   | A fast-track / simplified divorce | `joint-simplified-dissolution` |

   A single question can touch more than one clause (e.g., "how long will my divorce take and what
   will I get" touches `waiting-period`, `division-standard`, and possibly `maintenance`). Route to
   every clause the question actually asks about; don't force a multi-part question into one row.

   If the question doesn't match any `clause` in the table, say so directly: "That's not one of the
   topics this reference currently covers for Illinois family law — [state what was searched and
   what wasn't found, per the honest-negative rule in CLAUDE.md]," rather than answering from
   general knowledge under this skill.

5. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question` in one or two sentences — the `rule`
     field is already written this way ("Ninety days, not six months, and only one spouse has to
     meet it...") — lead with that, not with throat-clearing.
   - Follow with the supporting statutory text: quote directly from `authority[].verified.corpus.quote`,
     cite `authority[].cite`, and tag it `[DocketRouter — verified]`.
   - If the row has more than one authority, use as many as are relevant to the specific question
     asked — don't dump every authority on the row if the user asked a narrower question, but
     don't omit an authority that changes the answer (e.g., the $500,000 guideline-eligibility
     cutoff on the maintenance row).
   - Read `notes` and carry forward any scope limit it records (an honest negative, an excluded
     cross-reference, an unresolved question) as part of the answer, not as a buried caveat.
   - After the rule, surface the row's `trap` field as a distinct "what people get wrong" note —
     this is the load-bearing misconception-correction content of the row and should not be
     dropped for brevity. Several traps in this reference correct a reflex comparison to Texas
     (e.g., residency, maintenance duration, name resumption) — keep that comparison in the answer
     when it is the thing the user is likely to get backwards, but do not extend it to states this
     reference has not verified (California is not established in this reference).

6. **If the corpus doesn't answer part of the question**, say exactly that, using the pattern from
   the honest-negative rule in `../../CLAUDE.md` — what was searched, what wasn't found — instead
   of filling the gap with `[model knowledge — verify]` prose that reads as equally authoritative
   as the verified part of the answer. This reference has several genuine gaps: no post-filing
   waiting period was found in the Act (`waiting-period`), no Texas-style binding mediated
   settlement agreement statute was found (`mediation`, `marital-settlement-agreements`), and the
   specific Illinois Supreme Court Rule governing the financial affidavit form was not located
   (`financial-affidavit-and-disclosure`).

7. **Close every substantive answer** with the draft-for-attorney-review line from
   `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim
statutory quote and `[DocketRouter — verified]` tag, the trap/misconception note, any honest
negative the corpus requires, and the draft-for-attorney-review line. Not a memo, not a multi-page
review — sized to the question asked.
