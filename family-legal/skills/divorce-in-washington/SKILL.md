---
name: divorce-in-washington
description: >
  Routes a Washington dissolution (divorce) question to the right row in the verified reference
  and renders the answer with its verbatim RCW quote and source tag. Covers residency and venue,
  grounds, the waiting period, community versus separate property, the division standard, spousal
  maintenance, child support guidelines, the parenting plan (decision-making and residential
  time), required disclosures, temporary orders, mediation/ADR, separation contracts, name
  restoration, and legal separation and its conversion to dissolution. Use when the user asks "can
  I file for divorce in Washington", "how is property split in a Washington divorce", "do I
  qualify for spousal maintenance", "how much child support in Washington", "who gets the kids",
  "how long does a Washington dissolution take", "what is a parenting plan", or any other
  Washington dissolution question. Does not cover any state other than Washington, and does not
  answer protective-order or safety-planning questions.
argument-hint: '[a Washington dissolution question, plain text]'
---

# /divorce-in-washington

Answers a Washington dissolution question from the verified reference at
`../../references/rows-wa-family.json`, never from memory alone. Read
`../../CLAUDE.md` before running this skill — it has the source-tag vocabulary, the jurisdiction
gate, the family-violence caveat, and the honest-negative rule this skill depends on.

## Instructions

1. **Family-violence check, first, before anything else.** If the question is actually about a
   protective order, safety planning, or "am I safe" — not a dissolution mechanics question that
   happens to mention violence — stop and follow the family-violence caveat in `../../CLAUDE.md`
   verbatim. Do not proceed to step 2. This includes a question that sounds like it is about the
   `parenting-plan` row's mandatory-limitation trigger (RCW 26.09.191(4)(a)) — that row states the
   trigger exists as part of answering how the ordinary allocation regime works; it is not a
   safety plan, and a question actually about safety routes to the caveat instead.

2. **Jurisdiction check.** If the facts don't establish Washington (no Washington county, court, or
   explicit statement), ask. If another state is named, follow the jurisdiction gate in
   `../../CLAUDE.md` and stop — do not answer a non-Washington question from this skill's rows.
   Washington's own terminology is "dissolution of marriage," not "divorce"; treat the two as the
   same question, but use "dissolution" when quoting or citing the statute.

3. **Load the rows.** Read `../../references/rows-wa-family.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority`
   (an array of statute quotes with `verified.corpus` blocks), `status`, and `notes`.

4. **Route the question to the matching row(s) by `clause`.** The clauses currently in the
   reference:

   | User is asking about | `clause` value |
   |---|---|
   | How long you must live in Washington, or which county | `residency-and-venue` |
   | Whether fault has to be proven; what the only ground is | `grounds` |
   | How soon after filing the dissolution can be granted | `waiting-period` |
   | Whose property is whose; what counts as community property | `community-vs-separate-property` |
   | How the court splits property; is it 50/50; does it reach separate property | `division-standard` |
   | Who qualifies for spousal maintenance, how much, and for how long | `spousal-maintenance` |
   | How child support is calculated | `child-support-guidelines` |
   | Decision-making authority, residential time/schedule, custody | `parenting-plan` |
   | What has to be disclosed; worksheets; financial affidavits | `required-disclosures` |
   | What a court can order while the case is pending | `temporary-orders` |
   | Is mediation required | `mediation-and-adr` |
   | Separation contracts, can I back out of one | `separation-contracts` |
   | Changing a name back in the dissolution decree | `name-restoration` |
   | Legal separation, and converting it to a dissolution | `legal-separation-and-conversion` |

   A single question can touch more than one clause (e.g., "how long will my dissolution take and
   what will I get" touches `waiting-period`, `division-standard`, and possibly
   `spousal-maintenance`). Route to every clause the question actually asks about; don't force a
   multi-part question into one row.

   If the question doesn't match any `clause` in the table, say so directly: "That's not one of
   the topics this reference currently covers for Washington family law — [state what was searched
   and what wasn't found, per the honest-negative rule in CLAUDE.md]," rather than answering from
   general knowledge under this skill.

5. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question` in one or two sentences — the `rule`
     field is already written this way — lead with that, not with throat-clearing.
   - Follow with the supporting statutory text: quote directly from
     `authority[].verified.corpus.quote`, cite `authority[].cite`, and tag it
     `[DocketRouter — verified]`.
   - If the row has more than one authority, use as many as are relevant to the specific question
     asked — don't dump every authority on the row if the user asked a narrower question, but
     don't omit an authority that changes the answer (e.g., the mandatory residential-time
     limitation trigger in the `parenting-plan` row).
   - Read `notes` and carry forward any scope limit it records (an honest negative, an excluded
     cross-reference, an unresolved question) as part of the answer, not as a buried caveat.
   - After the rule, surface the row's `trap` field as a distinct "what people get wrong" note —
     this is the load-bearing misconception-correction content of the row and should not be
     dropped for brevity. Several `trap` fields in this reference contrast Washington against
     Texas (no residency-duration floor, no fault grounds, no maintenance cap or duration limit,
     no joint-decision-making presumption, a statutory worksheet/financial-affidavit disclosure
     duty Texas's Family Code lacks) — keep that contrast in the answer when it is what the trap
     is built around, since it is often the actual misconception being corrected.

6. **If the corpus doesn't answer part of the question**, say exactly that, using the pattern from
   the honest-negative rule in `../../CLAUDE.md` — what was searched, what wasn't found — instead
   of filling the gap with `[model knowledge — verify]` prose that reads as equally authoritative
   as the verified part of the answer. This reference has more open questions than the Texas one
   in a few places (for example, whether the `required-disclosures` worksheet/affidavit duty
   reaches a childless dissolution, and what "on motion" requires procedurally for a
   legal-separation conversion) — read the relevant row's `notes` before answering rather than
   assuming the statute resolves everything it is quoted for.

7. **Close every substantive answer** with the draft-for-attorney-review line from
   `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim
statutory quote and `[DocketRouter — verified]` tag, the trap/misconception note, any honest
negative the corpus requires, and the draft-for-attorney-review line. Not a memo, not a multi-page
review — sized to the question asked.
