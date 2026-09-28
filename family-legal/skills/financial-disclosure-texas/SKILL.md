---
name: financial-disclosure-texas
description: >
  Explains what a Texas divorce actually requires a party to disclose or produce, and when —
  required disclosures, the sworn inventory and appraisement, and temporary-order production and
  support obligations — rendered strictly from the verified reference, including where the Family
  Code does NOT impose a duty a user might assume exists. Use when the user asks "what do I have
  to disclose", "is there a required inventory", "what can the other side make me produce", "do I
  have to fill out a financial statement", or similar discovery/disclosure questions in a Texas
  divorce. Does not cover any state other than Texas, and does not answer protective-order or
  safety-planning questions.
argument-hint: '[a Texas divorce disclosure or discovery question, plain text]'
---

# /financial-disclosure-texas

Answers what the Family Code actually requires a party to disclose, produce, or inventory during
a Texas divorce, from the verified rows at `../../references/rows-tx-family.json` — and states
plainly where the Family Code does not reach a question the user assumes it answers. Read
`../../CLAUDE.md` before running this skill.

## Instructions

1. **Family-violence check, first.** If the real question is about safety, not disclosure, follow
   the family-violence caveat in `../../CLAUDE.md` and stop.

2. **Jurisdiction check.** Texas only; otherwise follow the jurisdiction gate in `../../CLAUDE.md`
   and stop.

3. **Load the three rows this skill draws on** from `../../references/rows-tx-family.json`:
   `required-disclosures`, `inventory-and-appraisement`, and `temporary-orders`. These three
   rows share a theme — none of them describes a duty that exists automatically on filing; each
   one exists only once triggered (by opting into the collaborative process, by a court order, or
   by a motion and hearing) — and that theme is itself the answer to most questions this skill
   gets asked.

4. **Sort the question before answering:**
   - **"What do I have to voluntarily disclose, generally?"** → `required-disclosures`. Lead with
     the honest negative: the Family Code's own disclosure duty (§ 15.109) applies only inside the
     opt-in collaborative family law process. If the user is not in that process, say plainly that
     the general discovery obligations in a Texas divorce come from the Texas Rules of Civil
     Procedure, which are outside this corpus, and that this skill cannot state those rules from
     the verified reference — recommend the user ask their attorney about Rule 194 and standard
     written discovery rather than inventing an answer.
   - **"Is there a required financial inventory / do I have to list my property?"** →
     `inventory-and-appraisement`. Lead with the honest negative here too: there's no standing,
     self-executing Family Code inventory requirement — it exists only as temporary-order relief
     a court may order under § 6.502(a)(1), and the specific form used in practice is typically set
     by local county standing order, not the Family Code.
   - **"What can a court make me hand over or do while the case is pending?"** → `temporary-orders`.
     This is the broader power (document production, support payments, a receiver, exclusive use
     of the house, spending restrictions) that the inventory duty is one item of.

5. **Render each matched row** the same way `/divorce-in-texas` does: answer the row's `question`
   first, quote `authority[].verified.corpus.quote` with `[DocketRouter — verified]`, surface
   `trap`, and carry forward `notes` — especially the honest negatives, which are the point of this
   skill more than in most others. A user asking about disclosure is very likely assuming a general
   duty exists; correcting that assumption is the primary job here, not a footnote.

6. **Do not fill the Rules-of-Civil-Procedure gap with model knowledge.** It is tempting to answer
   "what does Rule 194 require" from training knowledge since it's common practice-area
   knowledge — don't. This corpus does not carry the Rules of Civil Procedure, and this skill's
   entire value is in NOT quietly bridging that gap. Tag anything outside the verified rows
   `[model knowledge — verify]` at minimum, and prefer saying "not established in this reference;
   ask your attorney" over supplying an unverified rule number from memory.

7. **Close with the draft-for-attorney-review line** from `../../CLAUDE.md`.

## Output

A direct answer sized to the question, built from the `required-disclosures`,
`inventory-and-appraisement`, and/or `temporary-orders` rows, leading with whichever honest
negative applies before any affirmative rule, with verbatim quotes tagged
`[DocketRouter — verified]` and the draft-for-attorney-review line.
