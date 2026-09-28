---
name: employment-law
description: >
  Front door for employment-law questions answered only from a verified reference — the FMLA's
  twelve-workweek entitlement, what an employer's failure to designate leave does and does not buy,
  whether a state employer can be sued for family-care leave versus self-care leave, the Title VII
  charge-filing deadline and when the clock starts, Washington's non-compete disclosure requirement,
  and New Jersey's Conscientious Employee Protection Act. Use when someone asks "how much FMLA leave
  do I get", "they never told me it was FMLA", "can I sue the state for denying my leave", "how long
  do I have to file", "is my non-compete enforceable in Washington", or "I reported something and
  they retaliated" in New Jersey. Covers the federal tier, California, Texas, Washington and New
  Jersey only; every other state is reported as not covered rather than answered from general knowledge. Never says
  whether a person has a claim and never computes a deadline date.
argument-hint: '[an employment-law question, plain text]'
---

# /employment-law

Answers from `../../references/` (one `rows-<jurisdiction>-employment.json` per jurisdiction: `us`, `ca`, `tx`, `wa`, `nj`), never from memory alone. Read `../../CLAUDE.md`
first — it carries the jurisdiction gate, the source tags, the rule that federal statutory text is
that statutory text is quoted from the Code and holdings from the opinions, and the deadline protocol.

## How to run

1. **Find the jurisdiction.** Federal, California, Texas, Washington or New Jersey. If the question is about
   another state, stop and say the pack does not cover it. Do not answer from general knowledge, and do not
   offer the federal row as a substitute for a state one without saying that is what you are doing.

2. **Find the topic.** One of `family-medical-leave`, `leave-interference`, `remedies`,
   `charge-filing-deadline`, `final-pay`, `non-competition`, `retaliation-whistleblower`. If the question does not land in one, say so rather than stretching
   a row to fit.

3. **Read the matching row in full** — `rule`, `trap` and `notes`. The `notes` field is where the
   pack records what it could *not* establish, and for these rows that list is long and load-bearing:
   eligibility thresholds, certification rules, intermittent leave, current deferral-state lists,
   and the separate deadlines under other statutes are all absent.

4. **Render the answer** with the quote and its source, the trap, and every NOT ESTABLISHED item
   that bears on the question actually asked.

5. **Close with the limit.** This is a draft for attorney review. If the question touched a deadline,
   the closing line says to speak to a lawyer today rather than giving the reader a date.

## The traps this pack exists to carry

These are the places where a plausible, confident, wrong answer is easiest to give:

- **Twelve weeks is a ceiling as well as a floor.** An employer that already gave more leave than the
  Act requires has met it. Ragsdale lost having received thirty weeks.
- **A notice failure is not itself the violation.** After Ragsdale the employee must show the failure
  actually cost them something. The missing form alone is not the case.
- **A state employer can be sued for family-care leave and not for self-care leave.** Two employees at
  one agency, both denied leave, can have different remedies. And neither case says anything about a
  private employer.
- **The charge clock starts when you are told.** Ricks was notified, worked out the year, filed after
  the job ended, and lost.
- **FMLA damages double by default.** Liquidated damages equal the lost pay plus interest again, and the
  employer gets back to single damages only by proving good faith AND reasonable grounds, at the court's discretion.
- **The FMLA clock is not the discrimination clock.** Two years, three if wilful, no agency charge first.
- **A Washington non-compete can fail on timing alone**, regardless of how reasonable its terms are.
- **CEPA usually expects the employer to have had a chance to fix it first**, subject to the section's
  own exceptions.

## Handing off

This plugin answers what the law says. It does not sequence a person's next steps or tell them where to
file first — that is `sue-my-boss-speedrun`, which reads these same rows and adds the ordering. If the
question is "what do I do now", hand off rather than improvising a plan here.
