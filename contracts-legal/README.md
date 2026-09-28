# contracts-legal

State contract-law rules for clause review, copied from DocketRouter's verified contract-law store —
**601 rules and 3,597 authorities across 28 states**, and only the rules the
public API itself serves. Every rule carries the question it answers, the rule, the *trap* that makes a
facially normal clause fail in that state specifically, and its authorities with verbatim quotes, each
re-checked against our own published statute and case-law corpus. Data is CC0 (`references/LICENSE`).

The same rules are free and keyless at `GET https://docketrouter.ai/api/v1/contracts/rules?jurisdiction=<st>&clause=<family>`.

## Skills

- **`skills/contract-rule`** — `<state> <clause family>` → the question, rule, trap and authorities, verbatim.
- **`skills/review-clause`** — paste a clause or agreement → classified into the store's 23 families, each
  checked against the governing state's rules with the words on both sides quoted, and an honest list of
  what the state's store does not cover. Never rewrites from memory.
- **`skills/contract-trap`** — the state-specific catch, one family or all of them, as a checklist.

Run legal-scrambler (`family-legal`'s `/prepare-your-file`, or `bin/scramble.ts`) on an agreement that
names real parties before pasting it; party names are not needed for a clause review.

## What is held

| state | rules | authorities | families | as_of range |
|---|---|---|---|---|
| Alaska (`ak`) | 18 | 54 | 18 | 2026-09-21 → 2026-09-21 |
| Alabama (`al`) | 22 | 91 | 22 | 2026-09-17 → 2026-09-17 |
| California (`ca`) | 24 | 158 | 22 | 2026-09-14 → 2026-09-17 |
| Connecticut (`ct`) | 22 | 103 | 22 | 2026-09-16 → 2026-09-16 |
| Delaware (`de`) | 22 | 98 | 22 | 2026-09-14 → 2026-09-17 |
| Florida (`fl`) | 22 | 96 | 22 | 2026-09-16 → 2026-09-17 |
| Iowa (`ia`) | 22 | 113 | 22 | 2026-09-19 → 2026-09-19 |
| Idaho (`id`) | 20 | 92 | 20 | 2026-09-21 → 2026-09-22 |
| Illinois (`il`) | 22 | 165 | 22 | 2026-09-16 → 2026-09-17 |
| Kansas (`ks`) | 22 | 111 | 22 | 2026-09-19 → 2026-09-19 |
| Louisiana (`la`) | 17 | 109 | 17 | 2026-09-21 → 2026-09-21 |
| Maine (`me`) | 22 | 163 | 22 | 2026-09-20 → 2026-09-20 |
| Minnesota (`mn`) | 22 | 133 | 22 | 2026-09-17 → 2026-09-17 |
| Montana (`mt`) | 22 | 118 | 22 | 2026-09-20 → 2026-09-20 |
| North Carolina (`nc`) | 22 | 113 | 22 | 2026-09-17 → 2026-09-17 |
| North Dakota (`nd`) | 20 | 73 | 19 | 2026-09-21 → 2026-09-21 |
| Nebraska (`ne`) | 22 | 414 | 22 | 2026-09-20 → 2026-09-20 |
| New Jersey (`nj`) | 24 | 139 | 22 | 2026-09-14 → 2026-09-17 |
| New York (`ny`) | 22 | 181 | 22 | 2026-09-16 → 2026-09-17 |
| Ohio (`oh`) | 22 | 58 | 22 | 2026-09-17 → 2026-09-17 |
| Oregon (`or`) | 22 | 97 | 22 | 2026-09-17 → 2026-09-17 |
| Rhode Island (`ri`) | 22 | 321 | 22 | 2026-09-20 → 2026-09-20 |
| South Dakota (`sd`) | 17 | 64 | 17 | 2026-09-21 → 2026-09-21 |
| Texas (`tx`) | 27 | 99 | 22 | 2026-09-14 → 2026-09-16 |
| Utah (`ut`) | 22 | 80 | 22 | 2026-09-17 → 2026-09-17 |
| Virginia (`va`) | 22 | 165 | 22 | 2026-09-20 → 2026-09-20 |
| Washington (`wa`) | 22 | 119 | 22 | 2026-09-16 → 2026-09-16 |
| Wisconsin (`wi`) | 22 | 70 | 22 | 2026-09-17 → 2026-09-17 |

Exported by `the DocketX pack exporter` from `data/contract-law/` on 2026-09-28: all 607
store rows, of which 601 are servable and 6 carry status `no-verified-authority` with an empty
authority list. Those 6 are findings, not omissions: each records that the question was asked and
nothing was found. Corpus report generated 2026-09-25.

## How every rule was verified

Each authority's `verified` block records what was checked and when. Statute quotes and case quotes were
re-derived by `the DocketX corpus verifier` on our corpus host against the published
`docketx/us-statutes` and `docketx/us-caselaw-<state>` datasets — never from memory — confirming the quote
occurs verbatim in the section's or opinion's published text and that the row id is identifiably the
authority cited. Every state addition was break-tested before commit: a copy with three planted defects (a
changed word inside a quote, a row id repointed at a different section, an authority relabelled to the
wrong dataset) fails the verifier on exactly those three. The 2026-09-17 adversarial audit of the served
rules (`the DocketX contract-law audit of 2026-09-17` in DocketRouter) is the standard every row is held to: a
sentence survives only if the authority text, read in context, says it.

## What it does not do

- **No state outside the table**, and no clause family a state's file lacks, is answered from general
  knowledge. The gate in `CLAUDE.md` says what to say instead.
- **No currency.** Every rule has an `as_of` date; the store holds no statute effective dates and does not
  track later amendments or decisions. That is what "draft for attorney review" means here.
- **No drafting.** A redline may restate a rule's own requirement in the clause's words; anything more is
  an attorney's work.
