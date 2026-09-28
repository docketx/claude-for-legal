---
name: was-my-defense-ineffective
description: >
  For a criminal client asking about their own defense lawyer — keeps the Peeler doctrine (Texas's
  bar on a civil malpractice suit against trial counsel until the client is exonerated) and
  Strickland v. Washington (the federal constitutional ineffective-assistance standard) in clearly
  separate sections, never blended, and routes toward a criminal-appeals or post-conviction
  attorney rather than answering the criminal-law question itself. Use when the user asks "was my
  lawyer ineffective", "can I sue my criminal defense lawyer", "I was convicted and my lawyer
  messed up", "what is ineffective assistance of counsel", or similar. Does not cover any state
  other than Texas for the Peeler/civil-suit half, and does not provide criminal-appeals or habeas
  strategy.
argument-hint: '[a question about your criminal defense lawyer, plain text]'
---

# /was-my-defense-ineffective

Answers a criminal client's question about their own defense lawyer using the
`peeler-doctrine-criminal-malpractice` and `strickland-ineffective-assistance` rows in
`../../references/rows-tx-sue-my-lawyer.json`. Read `../../CLAUDE.md` before running this skill —
especially the "Criminal cases — Peeler and Strickland are different standards; never blend them"
section, which this skill exists to enforce.

## Instructions

1. **Named-lawyer check.** If the user names a real lawyer or firm, follow "What this plugin is
   never for" in `../../CLAUDE.md`.

2. **Route first, hotline-style, before any legal analysis.** This plugin does not provide
   criminal-appeals or post-conviction strategy, and a criminal client's timeline is often
   urgent (appellate and habeas deadlines are frequently shorter and less forgiving than the
   civil two-year period elsewhere in this plugin). Open with:

   > If you believe your criminal defense was constitutionally inadequate, the deadlines to raise
   > that on direct appeal or through post-conviction relief can be short and are separate from
   > anything else in this plugin. Please contact a criminal-appeals or post-conviction attorney
   > as soon as possible — a public defender's office, a regional innocence project, or the State
   > Bar's lawyer referral service can help you find one if you don't have one.

3. **Then present BOTH doctrines, in two clearly labeled, separate sections — never merged into
   one test or one paragraph:**

   - **Section A — Strickland v. Washington (the constitutional standard).** This is what a court
     uses to decide whether to set aside the conviction itself. Render both quoted prongs from the
     `strickland-ineffective-assistance` row: deficient performance judged by "an objective
     standard of reasonableness," and prejudice requiring "a reasonable probability that, but for
     counsel's unprofessional errors, the result of the proceeding would have been different."
     State plainly that this row does not cover Texas's own procedural mechanism for raising a
     Strickland claim (direct appeal versus habeas corpus under Tex. Code Crim. Proc. art. 11.07)
     — that is not established here, and is exactly the kind of question a criminal-appeals
     attorney handles.
   - **Section B — the Peeler doctrine (the Texas civil-suit bar).** This is a completely
     different question: whether the client may bring a CIVIL lawsuit for MONEY against the
     defense lawyer. Render the `peeler-doctrine-criminal-malpractice` row's quotes: the
     categorical bar unless "exonerated on direct appeal, through post-conviction relief, or
     otherwise," and the footnote that exoneration is only the threshold — the client still has to
     prove every element of malpractice (duty, breach, causation, damages — cross-reference the
     `elements-of-legal-malpractice` and `case-within-a-case-causation` rows) after that.

4. **State the relationship between the two sections carefully, using only what the rows
   establish.** Winning Strickland relief (a reversed conviction, a new trial, habeas relief) is
   ONE possible way to become "exonerated" for Peeler purposes — but do not state that a Strickland
   victory automatically satisfies Peeler's exoneration requirement; that connection is not
   established in this plugin's verified rows, per `../../CLAUDE.md`.

5. **If the user is still in the criminal case (not yet convicted, or convicted and still within
   an active appeal or habeas proceeding)**, make especially clear that this skill is not a
   substitute for, and should not delay, engaging appellate/habeas counsel — repeat the routing
   line from step 2 at the end of the answer, not just the beginning.

6. **Close with the draft-for-attorney-review line** from `../../CLAUDE.md`, in addition to (not
   instead of) the criminal-appeals routing line.

## Output

The routing line first, then Strickland and Peeler in two clearly separate, labeled sections, each
with its verbatim quotes tagged `[DocketRouter — verified]`, an explicit statement of how (and how
not) the two doctrines relate, and the draft-for-attorney-review line.
