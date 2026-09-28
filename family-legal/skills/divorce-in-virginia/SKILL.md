---
name: divorce-in-virginia
description: >
  Routes a Virginia divorce question to the right row in the verified reference and renders the
  answer with its verbatim statutory quote and source tag. Covers residency and venue, fault and
  no-fault grounds, the separation period as the filing gate, marital versus separate property
  (including hybrid/tracing rules), the equitable-distribution factors, spousal support factors
  and the adultery bar with its manifest-injustice exception, the child-support guideline
  presumption and deviation factors, custody and visitation with Virginia's no-presumption rule,
  pendente lite (temporary) orders including the presumptive percentage formula for temporary
  spousal support, settlement and marital agreements, name restoration, mediation, and what the
  Code of Virginia does and does not require by way of disclosure. Use when the user asks "can I
  file for divorce in Virginia", "how is property split in Virginia", "do I qualify for spousal
  support in Virginia", "how much child support in Virginia", "who gets custody in Virginia", "how
  long does a Virginia divorce take", "does adultery affect spousal support in Virginia", or any
  other Virginia divorce question. Does not cover any state other than Virginia, and does not
  answer protective-order or safety-planning questions.
argument-hint: '[a Virginia divorce question, plain text]'
---

# /divorce-in-virginia

Answers a Virginia divorce question from the verified reference at
`../../references/rows-va-family.json`, never from memory alone. Read
`../../CLAUDE.md` before running this skill — it has the source-tag vocabulary, the jurisdiction
gate, the family-violence caveat, and the honest-negative rule this skill depends on.

## Instructions

1. **Family-violence check, first, before anything else.** If the question is actually about a
   protective order, safety planning, or "am I safe" — not a divorce mechanics question that
   happens to mention violence or abuse — stop and follow the family-violence caveat in
   `../../CLAUDE.md` verbatim. Do not proceed to step 2. This matters more than usual for Virginia:
   several rows in this reference (the custody best-interest factors, the pendente lite
   family/household-member exclusion order, the mediation referral's family-abuse screen) mention
   family abuse or violence in passing because the statute does, but none of them is a substitute
   for the caveat's routing, and none should be used to build safety guidance.

2. **Jurisdiction check.** If the facts don't establish Virginia (no Virginia county or circuit
   court, no explicit statement), ask. If another state is named — including Texas, California,
   Washington, or Illinois, which this plugin also covers — follow the jurisdiction gate in
   `../../CLAUDE.md` and stop. Never answer a Virginia question from another state's rows, or a
   question about another state from the Virginia rows; Virginia disagrees with at least one other
   covered state on nearly every clause here (Virginia keeps fault grounds where California and
   Washington have none; Virginia's equitable-distribution factors are open-ended where California
   commands an equal split; Virginia has an adultery bar on permanent spousal support with no
   analog quoted in this plugin's other three states; Virginia's custody statute says "no
   presumption" in the same breath California's does, but Texas presumes joint managing
   conservatorship) — do not assume any state's rule carries over by analogy, even where the topic
   looks the same.

3. **Load the rows.** Read `../../references/rows-va-family.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority` (an
   array of statute quotes with `verified.corpus` blocks), `status`, and `notes`.

4. **Route the question to the matching row(s) by `clause`.** The clauses currently in the
   reference:

   | User is asking about | `clause` value |
   |---|---|
   | How long you must live in Virginia before the suit can be filed | `residency-and-venue` |
   | Whether fault has to be proven; what the fault and no-fault grounds actually are | `grounds` |
   | How long spouses must be separated before the case can move to decree | `waiting-period` |
   | Whose property is whose; marital versus separate property; property that is part each | `marital-vs-non-marital-property` |
   | How the court splits the marital estate; is it 50/50 | `division-standard` |
   | What factors govern spousal support; whether adultery bars support | `spousal-support` |
   | How child support is calculated; when a court can order something other than the guideline amount | `child-support-guidelines` |
   | Custody, visitation, is there a presumption toward one parent or one arrangement | `custody-and-visitation` |
   | What a court can order while the case is pending; temporary spousal support | `temporary-orders` |
   | Can spouses agree to their own terms; separation and marital agreements | `settlement-agreements` |
   | Changing a name back after the divorce | `name-restoration` |
   | Is mediation available or required for custody/visitation | `mediation` |
   | What has to be disclosed during the case | `required-disclosures` |

   A single question can touch more than one clause (e.g., "how long will my Virginia divorce take
   and what will I get" touches `waiting-period`, `division-standard`, and possibly
   `spousal-support`). Route to every clause the question actually asks about; don't force a
   multi-part question into one row.

   If the question doesn't match any `clause` in the table, say so directly: "That's not one of the
   topics this reference currently covers for Virginia family law — [state what was searched and
   what wasn't found, per the honest-negative rule in CLAUDE.md]," rather than answering from
   general knowledge under this skill.

5. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question` in one or two sentences — the `rule` field
     is already written this way ("At least six months of actual bona fide residence and domicile
     in the Commonwealth by ONE of the parties...") — lead with that, not with throat-clearing.
   - Follow with the supporting statutory text: quote directly from
     `authority[].verified.corpus.quote`, cite `authority[].cite`, and tag it
     `[DocketRouter — verified]`.
   - If the row has more than one authority, use as many as are relevant to the specific question
     asked — don't dump every authority on the row if the user asked a narrower question, but don't
     omit an authority that changes the answer (e.g., the adultery bar's manifest-injustice
     exception in `spousal-support`, or the $10,000 combined-income ceiling on the presumptive
     pendente lite formula in `temporary-orders`).
   - Read `notes` and carry forward any scope limit it records (an honest negative, an excluded
     cross-reference, an unresolved question) as part of the answer, not as a buried caveat. The
     `spousal-support` row's own `trap` flags that its reading of "in such spouse's favor" is not
     confirmed against case law — carry that qualifier forward rather than stating the adultery bar
     as a flatly settled mechanical rule.
   - After the rule, surface the row's `trap` field as a distinct "what people get wrong" note —
     several Virginia traps are specifically about not assuming another covered state's rule
     carries over (fault grounds surviving alongside no-fault, an adultery bar on support that
     California and Washington do not have, a presumptive percentage formula for temporary spousal
     support that none of this plugin's other three states have) — this is the load-bearing
     misconception-correction content of the row and should not be dropped for brevity.

6. **If the corpus doesn't answer part of the question**, say exactly that, using the pattern from
   the honest-negative rule in `../../CLAUDE.md` — what was searched, what wasn't found — instead of
   filling the gap with `[model knowledge — verify]` prose that reads as equally authoritative as
   the verified part of the answer. Several rows in this reference already record honest negatives
   this way (no dollar/percentage cap found on ordinary spousal support; no Virginia analog to
   Texas's mediated-settlement-agreement revocability rule was searched for; the general,
   non-collaborative discovery regime lives in the Rules of Supreme Court of Virginia, which this
   corpus does not carry) — follow the same pattern for a new gap rather than guessing.

7. **Close every substantive answer** with the draft-for-attorney-review line from
   `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim
statutory quote and `[DocketRouter — verified]` tag, the trap/misconception note, any honest
negative the corpus requires, and the draft-for-attorney-review line. Not a memo, not a multi-page
review — sized to the question asked.
