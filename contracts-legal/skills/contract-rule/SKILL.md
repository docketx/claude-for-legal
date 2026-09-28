---
name: contract-rule
description: >
  States what a given state's law actually requires for one contract clause family — indemnity,
  limitation of liability, liquidated damages, non-competition, warranty disclaimer, anti-assignment,
  arbitration, choice of law, venue, attorney fees, limitations period, usury, statute of frauds,
  force majeure, entire agreement, electronic signature, prompt payment, lien waiver, trade-secret
  confidentiality, unconscionability, consumer-protection waiver — from the verified store: the
  question the rule answers, the rule, the trap that makes a facially normal clause fail in that
  state, and every authority with its verbatim quote. Use when the user asks "is this enforceable in
  <state>", "what does <state> require for a <clause>", "will a <state> court enforce this cap /
  non-compete / fee clause". Covers 28 states; reports any other as not covered.
argument-hint: '[state] [clause family or a plain description of the clause]'
---

# /contract-rule

Read `../../CLAUDE.md` first: the jurisdiction gate, the clause vocabulary, the source tags and
the attorney-review line all apply.

## Instructions

1. **Governing law.** Take the state from the argument or the contract's choice-of-law clause. If
   neither, ask. Check `../../references/index.json`: if the state has no entry, follow the gate's
   "no file" step and stop.
2. **Classify the clause** into one of the families listed in `CLAUDE.md`. If the user gave a family
   name, use it; if they described a clause, name the family you chose and why in one line. If it
   fits none, say so.
3. **Look it up** in `../../references/rows-<state>.json`: every row whose `clause` equals the
   family (filter the file for `"clause": "<family>"`; do not read it whole). There may be more than one (Texas holds several indemnity rules). If there is none,
   follow the gate's "no rule for this family" step and list the families the state does hold
   (from `index.json`).
4. **Render each row, in this order**, tagged `[DocketRouter — verified]` with its `rule_id`:
   - **Question** — the row's `question`, verbatim.
   - **Rule** — the row's `rule`, verbatim. Do not paraphrase a rule; the wording was verified, a
     paraphrase was not.
   - **Trap** — the row's `trap`, verbatim, if present.
   - **Authorities** — each authority's `cite` (and `name`, `court`, `decided` when present) with
     its verbatim quote, which is at `verified.corpus.quote` (not a top-level field). The same cite
     may appear several times with different quotes; render each. If an authority has no
     `verified.corpus` block, cite it without a quote and say so.
   - **As of** — the row's `as_of` date, and the `notes` if they record something the verification
     could not establish.
   - If the row carries `applies_to`, say which contract kinds it is limited to and, if the user's
     contract is not one of them, say the rule was held back and why.
5. **Stop there.** Do not add what "most states" do, do not fill a gap with general knowledge, and
   do not compare to another state unless asked — and then only from that state's own file.

## Example

`/contract-rule mt liquidated-damages` → the Montana row
`mt-void-unless-actual-damages-impracticable-to-fix`: the question, the rule ("Only if actual
damages would have been impracticable or extremely difficult to fix when the contract was made —
otherwise it is void…"), the trap, the § 28-2-721 quote and the Montana Supreme Court quotes, the
`as_of` date. Nothing else.
