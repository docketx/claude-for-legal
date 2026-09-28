---
name: audit-a-pack
description: >
  Adversarially audits an installed reference pack: samples its rules deterministically, reads every
  authority in full and in context from the corpus, and asks of each sentence whether the authority
  actually says it — returning a verdict per rule (CORRECT / OVERSTATED / UNSUPPORTED SENTENCE / WRONG)
  with a severity. Use when the user asks "is this pack any good", "audit these rules", "how do we know
  this is right", before relying on a pack for real work, or after building one. Verification proves a
  quote is real; only this proves the rule is a fair reading of it.
argument-hint: '[pack or jurisdiction] [how many rules to sample]'
---

# /audit-a-pack

Read `../../references/VERIFICATION.md` first. Verification and audit answer different questions: the
verifier proves the authority **says the words**; the audit asks whether the **rule is a fair reading** of
them. A pack can be 100% verified and still wrong.

## Instructions

1. **Sample deterministically, and say how.** Sort the pack's served rules by `rule_id` and take fixed
   positions (1, 6, 11, 16 …) per jurisdiction. A sample you chose by eye is a sample you chose to pass.
2. **Read the authority in full, in context** — the whole section, its neighbours where scope depends on
   them, the opinion's own reasoning rather than the quoted fragment. Check whether the quote is a holding
   or a recitation of someone else's argument, and whether a dissent is being quoted as law.
3. **Check later treatment.** For each cited case, scan the jurisdiction's later decisions for its cite or
   name near overrul / disapprov / abrogat / supersed / recede / declin\w+ to follow / called into
   question / limited to its facts. Report the windows, do not adjudicate them.
4. **Verdict per rule, with severity:**
   - **CORRECT** — every sentence is supported by the authority as read.
   - **UNSUPPORTED SENTENCE** (L) — a sentence is an inference, not text. Name the sentence.
   - **OVERSTATED** (L/M/H) — the authority says "may" and the rule says "will", or the rule's scope is
     wider than the section's own. Quote both sides.
   - **WRONG** (H) — a reader relying on it would get the law wrong in a common case.
   Severity is about the reader's exposure, not about how embarrassing the error is.
5. **Change nothing during the audit.** Produce the table first. Then, separately: pull every WRONG rule
   from serving immediately (`status: "partial"` with a note naming the audit), correct it against the
   text, and restore it only with a note recording what changed and why.
6. **Write the audit down with its date and method**, including the sample positions and the exact
   queries run. An audit nobody can re-run is an opinion.

## What a real audit looks like

The one this scaffolding came from sampled 24 rules across six jurisdictions and found 2 correct, 15
overstated, 4 unsupported sentences and 3 wrong. The three wrong rules were pulled the same day,
corrected against the sections' own text, and restored with notes. **Expect to find something.** An audit
that returns all-correct on first contact is evidence about the audit, not about the pack.
