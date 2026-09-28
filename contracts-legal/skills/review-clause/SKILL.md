---
name: review-clause
description: >
  Reviews a pasted contract clause, or a whole agreement, against the verified rules of its
  governing state: classifies each clause into the store's families, pulls every applicable rule
  with its trap and authorities, flags where the clause as written fails that state's requirement,
  and lists — separately and honestly — the families the state's store holds no rule for. Use when
  the user pastes a clause or agreement and asks "review this", "what's wrong with this under <state>
  law", "will this hold up", "redline this for <state>". Never rewrites a clause from memory;
  every flag cites a rule_id.
argument-hint: '[governing state, then the clause or agreement text — or run /prepare-your-file first]'
---

# /review-clause

Read `../../CLAUDE.md` first. If the agreement names real parties, run legal-scrambler on it
before pasting (the `family-legal` plugin's `/prepare-your-file` or `legal-scrambler`'s
`bin/scramble.ts`); party names are not needed for a clause review and should not leave the
user's machine.

## Instructions

1. **Governing law.** From the agreement's choice-of-law clause if it has one (say which section),
   else from the argument, else ask. Check `../../references/index.json`; no entry → the gate's
   "no file" step, stop.
2. **Contract kind.** Pick the first that fits from CLAUDE.md's `applies_to` list (construction,
   oilfield-services, employment, franchise, consumer, sale-of-goods, services), or "general". Say
   which and why in one line; this decides which tagged rules are served.
3. **Classify every clause** you were given into the families listed in `CLAUDE.md`. Quote the clause's own heading or
   first line next to the family. A clause that fits no family is listed as "not classified" —
   not silently dropped.
4. **For each classified clause**, load the state's rows for that family from
   `../../references/rows-<state>.json` (filter for `"clause": "<family>"`; do not read the file
   whole) and, for each rule:
   - state whether the clause **as written** meets the rule's requirement, quoting the words of the
     clause that do or do not — this is the one judgment this skill makes, and it must point at
     text on both sides;
   - render the rule's `trap` verbatim, and the authority quote that carries it (the quote is at
     `verified.corpus.quote`);
   - tag the rule `[DocketRouter — verified]` with `rule_id` and `as_of`; tag your own judgment
     `[review — a language-model reading of the clause against a verified rule]`.
   - Held-back rules (`applies_to` mismatch) are named, with the reason.
5. **The honest list, always, at the end:** the families present in the agreement for which the
   state's file holds **no rule** ("not covered by the verified store for <state>"), and the
   families the state holds that the agreement does not address — the latter is information, not a
   recommendation to add clauses.
6. **Never rewrite from memory.** A redline may only restate the rule's own requirement in the
   clause's words ("the indemnity must state in the clause itself that it covers the indemnitee's
   own negligence, in conspicuous type — per `tx-indemnity-fair-notice`"). Anything more is an
   attorney's job; say so.

## Output shape

Attorney-review line → governing law and contract kind (one line each) → one block per clause
(heading · family · finding · rule(s) with trap and quote · tags) → not-classified clauses →
not-covered families → families held but absent → one line of what was and was not checked.

## Example

Governing law `tx` (the agreement's § 14.2), contract kind `construction`. The pasted indemnity reads
"Subcontractor shall indemnify Contractor against all claims arising out of the Work." One block:

- **Clause** — "12. Indemnity" · family `indemnity`
- **Finding** `[review — a language-model reading of the clause against a verified rule]` — the clause
  does not say it covers Contractor's own negligence, so it fails the express-negligence requirement;
  the words "all claims arising out of the Work" are the only scope language, and no part of the clause
  names the indemnitee's negligence or is set in conspicuous type.
- **Rule** `[DocketRouter — verified]` `tx-lol-fair-notice`, as of 2026-09-14 — the row's `trap`
  verbatim, then Ethyl Corp. v. Daniel Construction Co.: "Under the doctrine of express negligence, the
  intent of the parties must be specifically stated within the four corners of the contract."
- **Also served for construction** — `tx-anti-indemnity-construction-contracts` (Tex. Ins. Code
  § 151.102 voids the clause to the extent it covers the indemnitee's own fault, including the duty to
  defend).

Then: not-classified clauses (none), families in the agreement the Texas file holds no rule for, families
Texas holds that the agreement does not address, and one line saying what was and was not checked.
