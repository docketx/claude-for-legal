---
name: divorce-in-iowa
description: >
  Routes an Iowa divorce question to the right row in the verified reference and renders the
  answer with its verbatim statutory quote and source tag. Covers residency and venue, grounds,
  the ninety-day waiting period, conciliation, the equitable division standard for marital
  property, spousal support factors, the child-support guidelines, custody and the joint-custody
  presumption question, the mandatory parenting course, required financial disclosures, temporary
  orders, mediation, settlement agreements, and name change at dissolution. Use when the user asks
  "can I file for divorce in Iowa", "how is property split", "do I qualify for spousal support",
  "how much child support", "who gets custody", "how long does an Iowa divorce take", "do I have
  to take a parenting class", or any other Iowa divorce question. Does not cover any state other
  than Iowa, and does not answer protective-order or safety-planning questions.
argument-hint: '[an Iowa divorce question, plain text]'
---

# /divorce-in-iowa

Answers an Iowa divorce question from the verified reference at
`../../references/rows-ia-family.json`, never from memory alone. Read
`../../CLAUDE.md` before running this skill — it has the source-tag vocabulary, the jurisdiction
gate, the family-violence caveat, and the honest-negative rule this skill depends on.

## Instructions

1. **Family-violence check, first, before anything else.** If the question is actually about a
   protective order, safety planning, or "am I safe" — not a divorce mechanics question that
   happens to mention abuse — stop and follow the family-violence caveat in `../../CLAUDE.md`
   verbatim. Do not proceed to step 2.

2. **Jurisdiction check.** If the facts don't establish Iowa (no Iowa county, court, or explicit
   statement), ask. If another state is named — including any other state this plugin covers —
   follow the jurisdiction gate in `../../CLAUDE.md` and stop. Never answer an Iowa question from
   another state's rows or another state's question from the Iowa rows; Iowa's own rules differ
   from this plugin's other states on nearly every clause it covers (a purely no-fault ground with
   no fault-based alternative, an "equitably" — not "equally" or "just and right" — division
   standard, a spousal-support statute with no fixed duration cap in its own text, and a custody
   section that states no affirmative joint-custody presumption at all, only a rebuttable
   presumption AGAINST joint custody on a domestic-abuse finding), so do not assume Iowa's rule by
   analogy to a state that looks similar.

3. **Load the rows.** Read `../../references/rows-ia-family.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority`
   (an array of statute quotes with `verified.corpus` blocks), `status`, and `notes`.

4. **Route the question to the matching row(s) by `clause`.** The clauses currently in the
   reference:

   | User is asking about | `clause` value |
   |---|---|
   | How long you must live in Iowa before the court can grant a decree | `residency-and-venue` |
   | Whether fault has to be proven; what the actual ground is | `grounds` |
   | How soon after service the divorce becomes final | `waiting-period` |
   | Whether the court can order the parties into conciliation, and how that affects timing | `conciliation` |
   | How the court splits the estate | `division-standard` |
   | Is inherited or gifted property divided along with everything else | `marital-vs-non-marital-property` |
   | What factors govern spousal support, and for how long it can last | `spousal-support` |
   | How child support is calculated, and whether the court can deviate | `child-support-guidelines` |
   | Custody, is there a joint-custody presumption | `custody-and-visitation` |
   | Is a parenting class required | `parenting-course` |
   | What financial disclosure is required | `required-disclosures` |
   | What a court can order while the case is pending | `temporary-orders` |
   | Is mediation required | `mediation` |
   | Can spouses agree and skip a hearing; settlement agreements | `settlement-agreements` |
   | Changing a name back (or to a birth-certificate name) in the divorce | `name-restoration` |

   A single question can touch more than one clause (e.g., "how long will my divorce take and what
   will I get" touches `waiting-period`, `division-standard`, and possibly `spousal-support`).
   Route to every clause the question actually asks about; don't force a multi-part question into
   one row.

   If the question doesn't match any `clause` in the table, say so directly: "That's not one of the
   topics this reference currently covers for Iowa family law — [state what was searched and what
   wasn't found, per the honest-negative rule in CLAUDE.md]," rather than answering from general
   knowledge under this skill.

5. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question` in one or two sentences — the `rule` field
     is already written this way ("One year, ordinarily...") — lead with that, not with
     throat-clearing.
   - Follow with the supporting statutory text: quote directly from
     `authority[].verified.corpus.quote`, cite `authority[].cite`, and tag it
     `[DocketRouter — verified]`.
   - If the row has more than one authority, use as many as are relevant to the specific question
     asked — don't dump every authority on the row if the user asked a narrower question, but don't
     omit an authority that changes the answer (e.g., the personal-service exception in
     `residency-and-venue`, or the domestic-abuse waiver in `mediation`).
   - Read `notes` and carry forward any scope limit it records (an honest negative, an excluded
     cross-reference, or an unresolved question) as part of the answer, not as a buried caveat.
   - After the rule, surface the row's `trap` field as a distinct "what people get wrong" note —
     several Iowa traps are specifically about NOT assuming a force word the statute doesn't use
     (there is no affirmative joint-custody presumption in the custody row; the residency section a
     stale citation points at, § 598.6, is repealed; mediation is never mandatory anywhere in this
     chapter) — this is the load-bearing misconception-correction content of the row and should not
     be dropped for brevity.

6. **If the corpus doesn't answer part of the question**, say exactly that, using the pattern from
   the honest-negative rule in `../../CLAUDE.md` — what was searched, what wasn't found — instead of
   filling the gap with `[model knowledge — verify]` prose that reads as equally authoritative as
   the verified part of the answer. Several rows in this reference already record honest negatives
   this way (the child-support guideline schedule itself is not in the statute text; no dollar or
   percentage cap on spousal support was searched for; chapter 674's general name-change procedure
   was not independently read) — follow the same pattern for a new gap rather than guessing.

7. **Close every substantive answer** with the draft-for-attorney-review line from
   `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim
statutory quote and `[DocketRouter — verified]` tag, the trap/misconception note, any honest
negative the corpus requires, and the draft-for-attorney-review line. Not a memo, not a multi-page
review — sized to the question asked.
