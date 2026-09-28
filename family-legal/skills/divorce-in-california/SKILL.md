---
name: divorce-in-california
description: >
  Routes a California divorce question to the right row in the verified reference and renders the
  answer with its verbatim statutory quote and source tag. Covers residency and venue, grounds, the
  finality waiting period, community versus separate property, the division standard, spousal
  support factors and duration for a long-duration marriage, the child-support guideline formula,
  custody and visitation, required financial disclosures, temporary and automatic restraining
  orders, mandatory custody mediation, settlement agreements, name restoration, and summary
  dissolution. Use when the user asks "can I file for divorce in California", "how is property
  split", "do I qualify for spousal support", "how much child support", "who gets custody", "how
  long does a California divorce take", "what is summary dissolution", or any other California
  divorce question. Does not cover any state other than California, and does not answer
  protective-order or safety-planning questions.
argument-hint: '[a California divorce question, plain text]'
---

# /divorce-in-california

Answers a California divorce question from the verified reference at
`../../references/rows-ca-family.json`, never from memory alone. Read
`../../CLAUDE.md` before running this skill — it has the source-tag vocabulary, the jurisdiction
gate, the family-violence caveat, and the honest-negative rule this skill depends on.

## Instructions

1. **Family-violence check, first, before anything else.** If the question is actually about a
   protective order, safety planning, or "am I safe" — not a divorce mechanics question that
   happens to mention violence — stop and follow the family-violence caveat in `../../CLAUDE.md`
   verbatim. Do not proceed to step 2.

2. **Jurisdiction check.** If the facts don't establish California (no California county, court, or
   explicit statement), ask. If another state is named — including Texas, which this plugin also
   covers — follow the jurisdiction gate in `../../CLAUDE.md` and stop. Never answer a California
   question from the Texas rows or a Texas question from the California rows; the two states differ
   on nearly every clause this plugin covers (fault grounds, the division standard, the disclosure
   duty, automatic restraining orders, custody presumptions, and more), so do not assume either
   state's rule by analogy to the other even where the topic looks the same.

3. **Load the rows.** Read `../../references/rows-ca-family.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority`
   (an array of statute quotes with `verified.corpus` blocks), `status`, and `notes`.

4. **Route the question to the matching row(s) by `clause`.** The clauses currently in the
   reference:

   | User is asking about | `clause` value |
   |---|---|
   | How long you must live in California / the county before the judgment can enter | `residency-and-venue` |
   | Whether fault has to be proven; what the actual grounds are | `grounds` |
   | How soon after service/appearance the divorce becomes final | `waiting-period` |
   | Whose property is whose; what counts as community property | `community-vs-separate-property` |
   | How the court splits the estate; is it always 50/50 | `division-standard` |
   | What factors govern spousal support | `spousal-support-factors` |
   | How long spousal support lasts, especially after a long marriage | `spousal-support-duration` |
   | How child support is calculated | `child-support-guideline` |
   | Custody, visitation, is there a joint-custody presumption | `custody-and-visitation` |
   | What has to be disclosed during the case | `required-disclosures` |
   | What a court can order while the case is pending; automatic restraining orders | `temporary-orders` |
   | Is mediation required for a custody/visitation dispute | `mediation` |
   | Can spouses agree to divide property unequally; separation agreements | `settlement-agreements` |
   | Changing a name back in the divorce | `name-restoration` |
   | Is there a simplified/fast-track divorce procedure | `summary-dissolution` |

   A single question can touch more than one clause (e.g., "how long will my divorce take and what
   will I get" touches `waiting-period`, `division-standard`, and possibly `spousal-support-factors`
   or `spousal-support-duration`). Route to every clause the question actually asks about; don't
   force a multi-part question into one row.

   If the question doesn't match any `clause` in the table, say so directly: "That's not one of the
   topics this reference currently covers for California family law — [state what was searched and
   what wasn't found, per the honest-negative rule in CLAUDE.md]," rather than answering from
   general knowledge under this skill.

5. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question` in one or two sentences — the `rule` field
     is already written this way ("Six months in California and three months in the county...") —
     lead with that, not with throat-clearing.
   - Follow with the supporting statutory text: quote directly from
     `authority[].verified.corpus.quote`, cite `authority[].cite`, and tag it
     `[DocketRouter — verified]`.
   - If the row has more than one authority, use as many as are relevant to the specific question
     asked — don't dump every authority on the row if the user asked a narrower question, but don't
     omit an authority that changes the answer (e.g., the same-sex-marriage residency exception in
     `residency-and-venue`, or the ex parte property order in `temporary-orders`).
   - Read `notes` and carry forward any scope limit it records (an honest negative, an excluded
     cross-reference, a currency flag for a sunsetting or inflation-adjusted figure, an unresolved
     question) as part of the answer, not as a buried caveat.
   - After the rule, surface the row's `trap` field as a distinct "what people get wrong" note —
     several California traps are specifically about NOT assuming the Texas rule carries over (no
     joint-custody presumption, a general disclosure duty that Texas lacks, an automatic restraining
     order that Texas lacks, no fixed maintenance-duration cap that Texas has) — this is the
     load-bearing misconception-correction content of the row and should not be dropped for brevity.

6. **If the corpus doesn't answer part of the question**, say exactly that, using the pattern from
   the honest-negative rule in `../../CLAUDE.md` — what was searched, what wasn't found — instead of
   filling the gap with `[model knowledge — verify]` prose that reads as equally authoritative as
   the verified part of the answer. Several rows in this reference already record honest negatives
   this way (no dollar/percentage cap found on spousal support; no California analog to Texas's
   mediated-settlement-agreement revocability rule was searched for; no fault ground found in the
   dissolution-grounds chapter) — follow the same pattern for a new gap rather than guessing.

7. **Close every substantive answer** with the draft-for-attorney-review line from
   `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim
statutory quote and `[DocketRouter — verified]` tag, the trap/misconception note, any honest
negative the corpus requires, and the draft-for-attorney-review line. Not a memo, not a multi-page
review — sized to the question asked.
