---
name: contract-trap
description: >
  For a state and a clause family, states the common assumption versus what that state's law
  actually says — the `trap` every verified row carries: the thing that makes a clause that reads
  correctly anywhere else fail in this state specifically. Use when the user asks "what's the
  catch in <state> for <clause>", "what do people get wrong about <state> non-competes / indemnity
  / liquidated damages", "is there a gotcha", or wants a checklist of state-specific traps across
  the families the store holds for that state. Traps only; the full rule and authorities are
  /contract-rule.
argument-hint: '[state] [clause family, or "all" for every family the state holds]'
---

# /contract-trap

Read `../../CLAUDE.md` first: the jurisdiction gate and source tags apply.

## Instructions

1. **State.** From the argument; check `../../references/index.json`. No entry → the gate's "no
   file" step, stop.
2. **Family.** One of the families in `CLAUDE.md` (or "all"). For a family the state's file lacks, say so and list the
   families it has.
3. **For each matching row** in `../../references/rows-<state>.json` (filter for `"clause": "<family>"`) render, tagged
   `[DocketRouter — verified]` with `rule_id`:
   - **The assumption** — one sentence naming what a drafter used to other states would expect.
     This sentence is yours; tag it `[framing]` and keep it to what the trap itself implies.
   - **The trap** — the row's `trap`, verbatim.
   - **The words that carry it** — the single authority quote (`verified.corpus.quote`) that most
     directly supports the trap, with its cite.
   - **As of** — the row's `as_of`.
   A row with no `trap` field is listed as "no trap recorded" — not invented.
4. **For "all"**, order by family name and keep each entry to those four lines; this is a
   checklist, not a memo. End with the families the state's store does not hold.
5. **Never** generalise a trap to another state, and never add a trap the store does not record.

## Example

`/contract-trap ne non-competition` → for the Nebraska non-competition row, tagged
`[DocketRouter — verified]` with its `rule_id`:

- **The assumption** `[framing]` — a court will trim an overbroad covenant down to something
  enforceable.
- **The trap** — the row's `trap`, verbatim (Nebraska does not blue-pencil; the covenant stands or
  falls as written).
- **The words that carry it** — CAE Vanguard, Inc. v. Newman: "The provision of the agreement which
  states that a court may reform the covenant is of no effect."
- **As of** — the row's `as_of`.

Then, for `all`, one such entry per family the state holds, ending with the families it does not.
