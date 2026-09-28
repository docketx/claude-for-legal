---
name: sue-my-lawyer
description: >
  Front door for "what can I do about my own lawyer" in Texas — routes a question to the right row
  in the verified reference (malpractice elements, case-within-a-case causation, expert-testimony
  requirements, the anti-fracturing rule, breach of fiduciary duty and fee forfeiture, the two-year
  limitations period, the Hughes tolling rule, the discovery rule, assignability of malpractice
  claims) and renders the answer with its verbatim quote and source tag. Use when the user asks
  "can I sue my lawyer", "did my lawyer commit malpractice", "what do I have to prove", "how long
  do I have to sue", "can my lawyer be liable for X", or any other Texas legal-malpractice
  question that isn't specifically about the criminal-case doctrines, the client file, or the
  grievance/fee-dispute process (those route to this plugin's other three skills, and this skill
  should hand off to them rather than answer their questions itself). Does not cover any state
  other than Texas, and never characterizes a specific real lawyer or firm's conduct.
argument-hint: '[a question about your own Texas lawyer, plain text]'
---

# /sue-my-lawyer

Answers a Texas legal-malpractice question from the verified reference at
`../../references/rows-tx-sue-my-lawyer.json`, never from memory alone. Read `../../CLAUDE.md`
before running this skill — it has the source-tag vocabulary, the jurisdiction gate, the
never-characterize-a-real-lawyer rule, the limitations-period protocol, and the honest-negative
rule this skill depends on.

## Instructions

1. **Jurisdiction check, first.** If the facts don't establish Texas (no Texas county, court, or
   explicit statement), ask. If another state is named, follow the jurisdiction gate in
   `../../CLAUDE.md` and stop — do not answer a non-Texas question from this skill's rows.

2. **Named-lawyer check.** If the user names a specific real lawyer or firm and asks whether THAT
   lawyer did something wrong, follow the "What this plugin is never for" section of
   `../../CLAUDE.md`: explain the governing law, do not characterize the named lawyer's conduct,
   and say this is a fact-specific call for the user's own malpractice counsel.

3. **Route to the other three skills where the question is really about their topic:**
   - A criminal case and whether the defense was bad enough to matter → hand off to
     `../was-my-defense-ineffective`.
   - Getting the client file back, or what a lawyer must disclose/turn over → hand off to
     `../get-my-file`.
   - Filing a State Bar complaint, a fee dispute, or "is this a grievance or a lawsuit" →
     hand off to `../grievance-or-lawsuit`.
   This skill covers everything else in the reference: the civil malpractice cause of action
   itself (elements, causation, expert testimony, anti-fracturing), fee forfeiture, limitations
   and tolling, the discovery rule, and assignability.

4. **Load the rows.** Read `../../references/rows-tx-sue-my-lawyer.json` in full. Each row has
   `jurisdiction`, `clause` (the topic), `rule_id`, `question`, `rule`, `trap`, `authority` (an
   array of case or statute quotes with `verified.corpus` blocks), `status`, and `notes`.

5. **Route the question to the matching row(s) by `clause`** (this skill's own clauses):

   | User is asking about | `clause` value |
   |---|---|
   | What has to be proven to win a malpractice case | `elements-of-legal-malpractice` |
   | Whether a clear mistake is enough, or you also have to prove the underlying case would have won | `case-within-a-case-causation` |
   | Whether you need an expert witness | `expert-testimony-requirement` |
   | Suing for fraud/contract/DTPA/fiduciary duty instead of or alongside negligence | `anti-fracturing-rule` |
   | Self-dealing, undisclosed conflicts, getting the fee back without proving damages | `breach-of-fiduciary-duty-and-fee-forfeiture` |
   | How long you have to sue | `limitations-two-year-period` |
   | Whether the clock paused during an appeal in the underlying case | `hughes-tolling-rule` |
   | Finding out about the malpractice late | `discovery-rule-malpractice` |
   | Buying, selling, or assigning a malpractice claim | `assignability-of-malpractice-claims` |
   | Privilege over old communications with the lawyer, in a dispute with that lawyer | `attorney-client-privilege-after-relationship-ends` |

   A single question can touch more than one clause (a "can I sue and how long do I have"
   question touches at least `elements-of-legal-malpractice` and `limitations-two-year-period`).
   Route to every clause the question actually asks about.

   If the question doesn't match any `clause` here or in the other three skills, say so directly
   per the honest-negative rule in `../../CLAUDE.md`, rather than answering from general knowledge.

6. **Render the answer, question-first.** For each matched row:
   - Open by directly answering the row's own `question`.
   - Follow with the supporting text: quote directly from `authority[].verified.corpus.quote`,
     cite `authority[].cite`, and tag it `[DocketRouter — verified]`.
   - Read `notes` and carry forward any scope limit, corrected case name, or honest negative it
     records — do not drop it for brevity, and do not upgrade a court of appeals holding to a
     Texas Supreme Court holding (several rows in this file are court of appeals authority; the
     row's `notes` say so).
   - Surface the row's `trap` field as a distinct "what people get wrong" note.

7. **If the question touches a limitations period or tolling**, follow the limitations-period
   protocol in `../../CLAUDE.md` exactly: verbatim quote only, say the clock may already be
   running, and tell the user to consult a legal malpractice attorney now.

8. **If the corpus doesn't answer part of the question**, say exactly that, using the honest-negative
   pattern in `../../CLAUDE.md` — what was searched, what wasn't found — instead of filling the
   gap with `[model knowledge — verify]` prose that reads as equally authoritative as the verified
   part of the answer.

9. **Close every substantive answer** with the draft-for-attorney-review line from `../../CLAUDE.md`.

## Output

A direct answer to the question, in the row's own question-first framing, with the verbatim quote
and `[DocketRouter — verified]` tag, the trap/misconception note, any honest negative the corpus
requires, and the draft-for-attorney-review line. Not a memo — sized to the question asked.
