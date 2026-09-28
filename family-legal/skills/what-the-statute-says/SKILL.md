---
name: what-the-statute-says
description: >
  For a given Texas family-law topic, states what the statute actually says versus what people
  commonly assume it says, rendered from each row's `trap` field with its verbatim quote. This is
  the corrective layer, not a shortcut or a workaround — it exists to close the gap between a
  common misreading and the statute's own words, never to find an angle the statute doesn't
  support. Use when the user asks "what's the catch", "what do people get wrong about this", "is
  it really true that...", "what does the statute actually say", or when another skill's answer
  needs the misconception explicitly named and corrected. Does not cover any state other than
  Texas, and does not answer protective-order or safety-planning questions.
argument-hint: '[a Texas family-law topic or assumption to check, plain text]'
---

# /what-the-statute-says

Names the common misreading of a Texas family-law rule and corrects it against the statute's own
words, using the `trap` field of the matching row(s) in `../../references/rows-tx-family.json`.
Read `../../CLAUDE.md` before running this skill.

**What this skill is not:** it is not a way to find a loophole, a shortcut, or an angle to work
around a rule. Every `trap` field in the reference exists because the statute's actual text is
narrower, or differently shaped, than a common assumption — the correction always points BACK to
what the statute requires, never to a way around it. If a question is phrased as "how do I get
around X," reframe it as "what does the statute actually require here" and answer that.

## Instructions

1. **Family-violence check, first.** If the question is actually about safety or how to avoid a
   protective order's effect, follow the family-violence caveat in `../../CLAUDE.md` and stop —
   this skill does not provide a way around a protective order, and does not treat "what's the
   catch with a protective order" as a topic it answers.

2. **Jurisdiction check.** Texas only; otherwise follow the jurisdiction gate in `../../CLAUDE.md`
   and stop.

3. **Load the rows** from `../../references/rows-tx-family.json` and find the matching `clause`(s)
   using the same routing table as `/divorce-in-texas`.

4. **For each matched row, state the pairing explicitly:**
   - **The assumption** (what people commonly think the rule is — draw this from the row's `trap`
     field, which already names the misconception).
   - **What the statute actually says** — the verbatim quote from `authority[].verified.corpus.quote`,
     cited and tagged `[DocketRouter — verified]`.
   - **The gap between them**, stated plainly — this is the substance of the `trap` field; don't
     just restate the rule a second time, explain specifically where the common assumption goes
     wrong and why it matters in practice.

5. **Don't manufacture a trap that isn't in the row.** If a user asks about a topic where the
   matching row's `trap` field doesn't address their specific angle, say so — answer with the
   row's actual rule and trap, and note plainly that the specific angle they asked about isn't one
   this reference has researched, rather than inventing a plausible-sounding "gotcha."

6. **If the question names a topic with no matching row**, apply the honest-negative rule from
   `../../CLAUDE.md`: say what was searched and not found, rather than speculating about what the
   statute "probably" says.

7. **Close with the draft-for-attorney-review line** from `../../CLAUDE.md`.

## Output

For each matched topic: the assumption, the verbatim statutory text with
`[DocketRouter — verified]`, and the specific gap between them — followed by the
draft-for-attorney-review line. Skip topics the user didn't ask about even if their row is
adjacent; this skill answers the specific trap asked about, not every trap in the reference.
