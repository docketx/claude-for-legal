---
name: what-the-authority-says
description: >
  Answers a legal question from the installed reference packs and nothing else: finds the rows for the
  jurisdiction the facts establish, renders the question, rule, trap and every authority with its
  verbatim quote and as_of date, and says plainly when no pack covers the jurisdiction or no row covers
  the topic. Use for "what does the law say about X in Y", "is this enforceable in Y", "what are the
  requirements for X" — any question where the answer must be traceable to a published source rather
  than to the model. Never paraphrases a rule, never borrows another jurisdiction's, never fills a gap.
argument-hint: '[jurisdiction] [the question or the topic]'
---

# /what-the-authority-says

Read `../../CLAUDE.md` first: the two categories, the source tags, the jurisdiction gate and the
attorney-review line all apply.

## Instructions

1. **Establish the jurisdiction** from the facts, or ask. Check `references/` for a pack. No pack → the
   gate's step 2: name what is missing and how it gets added, do not answer from general knowledge.
2. **Classify the question into a topic** from that pack's vocabulary. Name the topic you chose in one
   line. If it fits none, say so — do not force the nearest.
3. **Load every row** for (jurisdiction, topic). There may be more than one; there may be none, in which
   case say so and list the topics the pack does hold.
4. **Render each row in this order**, tagged `[verified]` with its `rule_id`:
   - **Question** — the row's own `question`, verbatim.
   - **Rule** — the row's `rule`, verbatim. **Do not paraphrase**: the wording was verified, a paraphrase
     was not.
   - **Trap** — the row's `trap`, verbatim, if it has one.
   - **Authorities** — each `cite` (with `name`, `court`, `decided` when present) and its verbatim
     `quote`. An authority with no quote is cited without one, and you say so.
   - **As of** — the row's `as_of`, and its `notes` when they record something verification could not
     establish. The `notes` are part of the answer, not an appendix.
5. **Stop there.** No "most jurisdictions", no comparison to another pack unless asked — and then only
   from that pack's own rows.

## The shape of a good "no"

> No verified pack is installed for Ohio. I will not answer this from general knowledge: on this exact
> point the installed jurisdictions already split [one concrete example]. Adding Ohio needs a published
> corpus of its statutes and decisions — that is `/add-a-jurisdiction`. If you want a starting point now,
> a research connector can answer, tagged `[unverified]`.

That is a useful answer. "I don't have that" is not.
