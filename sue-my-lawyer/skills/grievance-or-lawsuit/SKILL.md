---
name: grievance-or-lawsuit
description: >
  Explains the two separate routes for a dispute with your own Texas lawyer — a State Bar
  grievance (discipline: reprimand, suspension, disbarment) versus a civil claim or fee-dispute
  procedure (money: a malpractice lawsuit, a fee-forfeiture claim, or the State Bar's model fee
  dispute resolution procedure under Tex. Gov't Code Chapter 81) — and states plainly that filing
  one is not filing the other. Use when the user asks "should I file a grievance or sue", "what
  does a grievance actually get me", "how do I get my fee back", "is a fee dispute the same as
  malpractice", or any question that uses "grievance" and "lawsuit" as if they were the same
  thing. Does not cover any state other than Texas, and does not state grievance-filing mechanics
  (where/how/deadline) this plugin's corpus does not hold.
argument-hint: '[a fee-dispute or grievance question about your own Texas lawyer, plain text]'
---

# /grievance-or-lawsuit

Explains the grievance/lawsuit distinction from the verified reference at
`../../references/rows-tx-sue-my-lawyer.json`, following the standing rule in `../../CLAUDE.md`
("Grievance versus lawsuit — always say which, and that one is not the other"). Read
`../../CLAUDE.md` before running this skill.

## Instructions

1. **Jurisdiction check.** Texas only; otherwise follow the jurisdiction gate in `../../CLAUDE.md`
   and stop.

2. **Named-lawyer check.** If the user names a real lawyer or firm, follow "What this plugin is
   never for" in `../../CLAUDE.md` — explain the process, do not characterize their conduct.

3. **Load the `fee-disputes-and-grievance-process` row** from
   `../../references/rows-tx-sue-my-lawyer.json`. This is the primary row for this skill.

4. **Always present both routes, even if the user only asked about one, because the point of this
   skill is the contrast:**
   - **The grievance/disciplinary route.** Quote Tex. Gov't Code § 81.072(d) and § 81.072(b) from
     the row. State plainly: this is a complaint about the LAWYER's conduct, decided by the State
     Bar's disciplinary system (ultimately administered under standards the Supreme Court of Texas
     sets), and its possible outcomes are things like a private reprimand, suspension, or
     disbarment — **not** money to the complaining client. The row's `rule` and `notes` fields
     spell out exactly what § 81.072's own text does and does not say the outcome is; do not go
     further than that text.
   - **The money routes.** These are: (a) a civil malpractice lawsuit (hand off the elements,
     causation, and limitations questions to `../sue-my-lawyer`); (b) a breach-of-fiduciary-duty /
     fee-forfeiture claim (same row family as (a), in `../sue-my-lawyer`); and (c) the State Bar's
     fee dispute resolution procedure under Tex. Gov't Code § 81.112, quoted in this row. Be exact
     about what § 81.112 actually requires: it only requires the State Bar to maintain a
     **standard MODEL** procedure "that may be used by a bar committee or other organization" —
     its own text does not make a specific local program mandatory, binding, or automatically
     available, and this plugin does not know a given local program's procedure, deadline, or
     whether its outcome binds the lawyer. Say that plainly rather than assuming binding
     arbitration.

5. **State the "always say which, and that one is not the other" rule explicitly in the answer**,
   not just implicitly through separate sections — for example: "Filing a grievance is a
   disciplinary complaint about the lawyer, not a lawsuit, and it will not by itself get you money
   back. If you want money, that's a different route: [civil claim / fee dispute procedure]."

6. **Honest negatives to state, not paper over:** day-to-day grievance-filing mechanics (where to
   file, what form, what deadline) are not established by this row — those live in the Texas Rules
   of Disciplinary Procedure, which this plugin's tooling cannot currently verify (see the
   `right-to-the-file` row's notes for exactly why). Whether a local fee-dispute program's decision
   is binding is also not established. Say both plainly rather than guessing at a process.

7. **Close with the draft-for-attorney-review line** from `../../CLAUDE.md`.

## Output

A side-by-side explanation of the grievance route and the money route(s), each with its verbatim
statutory quote tagged `[DocketRouter — verified]`, an explicit statement that one is not the
other, the honest negatives on filing mechanics and binding effect, and the draft-for-attorney-review
line.
