---
name: sue-my-boss-speedrun
description: >
  Front door for "something happened to me at work, what do I do now" — orders the next steps into
  preserve, understand the clock, file the thing that comes before a lawsuit, and get a lawyer onto
  it, using only the verified rules in the employment-law plugin. Use when someone says they were
  fired, demoted, denied leave, retaliated against after reporting something, or handed a non-compete,
  and wants to know what to do rather than only what the law says. Routes FMLA questions to
  did-they-break-fmla and deadline questions to whats-my-deadline. Covers the federal tier, Washington
  and New Jersey only. Never says whether the person has a claim, never computes a deadline date,
  never names or characterizes a real employer.
argument-hint: '[what happened at work, plain text]'
---

# /sue-my-boss-speedrun

Read `../../CLAUDE.md` first. It carries the three hard limits: no verdict on whether they have a
claim, no computed deadlines, no naming a real employer.

The legal statements come from `../../../employment-law/references/` (one `rows-<jurisdiction>-employment.json` per jurisdiction: `us`, `ca`, `tx`, `wa`, `nj`). This skill
adds the ordering and nothing else.

## Step 0 — find out where they are

Two questions, asked plainly:

- **Which state, and is the employer a government body?** The state decides which rows apply. Whether
  the employer is a state agency changes the FMLA answer completely, and it is the single fact most
  people do not think to mention.
- **Has anything been announced to them yet?** A told-you-in-advance termination starts a clock the
  person usually thinks has not started. Ask before anything else.

If the state is not one this pack covers (federal, California, Texas, Washington, New Jersey), say so, give the
preserving steps below anyway because those are not jurisdiction-specific, and stop there on the law.

## Step 1 — preserve, tonight

This is the part that is genuinely urgent and genuinely free. Give it first, before any legal talk.

- Write your own account, in your own words, dated today. What happened, when, who was present, what
  was said. Memory degrades and this is the thing people most regret not having.
- Keep a copy of what you **already lawfully have**: your offer letter and contract, pay records,
  your own performance reviews, messages you were a party to, any written policy you were given.
- Note names and dates of anyone who saw or heard it.

Say the limit out loud in the same breath: do **not** take confidential material you are not entitled
to, break a system rule to obtain evidence, or record a conversation where that is unlawful. Doing so
can be a fireable act in itself and can damage the claim being preserved.

## Step 2 — the clock

Do not calculate anything. Say three things:

1. A short clock probably applies.
2. It may have started **when you were told**, not when the job actually ended — the pack's
   charge-filing row carries the case on exactly that point.
3. Therefore: speak to a lawyer **today**, not after the last day of work.

If they want the underlying rule, hand off to `whats-my-deadline`, which renders it with its source
and its limits.

## Step 3 — what gets filed before a lawsuit

For discrimination claims there is normally an agency charge before any court case, and the deadline
in step 2 is the deadline for **that charge**, not for a lawsuit. This is the step most people have
never heard of, and missing it can end a claim that was otherwise fine.

State what the pack establishes and stop there. The pack does not hold current agency procedure, so do
not describe how to file, which office, or what form. Say that a lawyer or the agency itself will tell
them, and that step 2 means doing it now.

## Step 4 — route to the specific question

- Leave, denied leave, "they never told me it counted" → `did-they-break-fmla`
- "How long do I have" → `whats-my-deadline`
- Final pay, a non-compete, or retaliation after reporting something, in any covered state → the
  `employment-law` skill, which renders those rows directly. Final pay is often the fastest concrete thing a
  person can act on: Texas sets a six-day deadline after a discharge, and California makes the wages due
  immediately with a penalty measured in days of pay.

## Closing

One line: this is a draft for attorney review, not legal advice. Do not add reassurance, do not
predict an outcome, and do not tell them the employer behaved unlawfully.
