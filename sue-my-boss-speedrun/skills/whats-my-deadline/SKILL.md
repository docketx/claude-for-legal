---
name: whats-my-deadline
description: >
  Renders the verified charge-filing rule from the employment-law pack — the Title VII period the
  Supreme Court set out in Delaware State College v. Ricks, the longer deferral-state period quoted in
  Mohasco Corp. v. Silver, and the rule that the clock starts when the decision is announced rather
  than when its consequences arrive. Use when someone asks how long they have to file, whether it is
  too late, or whether they should wait until their last day. Refuses to compute a date, refuses to
  say whether time remains, and says out loud that the rule comes from 1980 opinions and is not a
  current-date calculator.
argument-hint: '[what happened and when you were told, plain text]'
---

# /whats-my-deadline

Read `../../CLAUDE.md` first. The hard rule in this skill: **never compute a date, and never tell
someone they still have time.** Answer from
`../../../employment-law/references/` (one `rows-<jurisdiction>-employment.json` per jurisdiction: `us`, `ca`, `tx`, `wa`, `nj`), clause `charge-filing-deadline`.

## Ask one question first

**When were you told?** Not when you left, not when it took effect — when you were informed of the
decision. That date is the one the rule turns on, and people give the wrong one almost every time.

## What the pack establishes

Render the row with its quotes and sources:

- Title VII requires a charge with the agency **before** a lawsuit, and the period is short.
  *Delaware State College v. Ricks* sets it out from the statute.
- Where a state fair-employment agency exists, a longer period applies; *Mohasco Corp. v. Silver*
  quotes the same section's exception.
- **The clock starts at the announcement.** In *Ricks* a professor was told he would receive a
  terminal one-year contract, worked that year, and filed after his employment actually ended. Too
  late: the period ran from the notice of the decision, not from the day the consequences arrived.

That last point is the whole reason this skill exists. Someone who has been told their job ends in
three months usually believes nothing has started. Tell them it probably has.

## The FMLA has its own clock, and it does not go through an agency

If the question is about leave rather than discrimination, the answer is different in both directions. The Act
gives two years from the last event constituting the violation, three if the violation was wilful, and the
employee may sue directly rather than filing a charge first. Render that row when leave is in play.

Getting this wrong cuts both ways. Someone who assumes the short charge deadline applies to an FMLA claim may
abandon a claim that was alive for years. Someone who assumes the two-year period covers a discrimination claim
will miss the charge deadline entirely. Ask which one the facts are about before answering either.

## Texas has its own clock, and it is not the federal one

If the person is in Texas, there is a second row: Tex. Lab. Code § 21.202 sets a 180-day period for a
complaint under the state statute, and the section opens "Except as provided by Subsection (a-1)", so
the statute itself carries at least one exception the pack does not establish.

Render both when Texas is in play, and say plainly that satisfying one does not satisfy the other. A
person who files with one body and assumes the other is covered is the failure mode this section
exists to prevent. The shortest applicable period is the one that governs what they do this week.

## What you must say, every time

The federal rows rest on two opinions from **1980**, construing the statute as it then stood. The Texas row is the statute's own current text, which is a different kind of source and still not a date calculator. Since then the limitation
periods, the list of states with their own agency, and the rules for how different kinds of claim
accrue have all moved. So:

1. Give the rule and name its source.
2. Say it comes from 1980 opinions and is not a current-date calculator.
3. Say the pack does **not** establish: which states have the longer period today, how the clock runs
   for a hostile-environment claim built from many incidents, how pay claims accrue, the separate and
   different deadlines under the ADA, the ADEA, the FMLA and § 1981, or the deadline to sue after a
   right-to-sue letter.
4. End with the only safe instruction: **speak to a lawyer today.**

## What you must never say

- Any specific date.
- "You have until …", "you still have time", "you have about N days left".
- "You are out of time." If the announcement was long ago, say that the rule starts the clock at the
  announcement, that exceptions and different rules exist which this pack does not hold, and that this
  is precisely the question to put to a lawyer immediately — not that the claim is dead.

Being told a claim is dead when it is not is as damaging as being told it is alive when it is not.
Neither is this skill's call.

## Closing

One line: a draft for attorney review, not legal advice.
