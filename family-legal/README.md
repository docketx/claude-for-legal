# family-legal

A family-law reference plugin for the claude-for-legal suite, built to the same verification
standard as DocketRouter's contract-law store: every legal statement is either a quote copied from
our own published statute corpus and re-checked verbatim against it, or it is stated as not
established, with what was searched. One verified reference file per state; no state's rule is
ever applied to another state's facts.

## What it is

Skills over per-state verified reference files:

- **`skills/prepare-your-file`** — run first. Pseudonymises the case document on the user's own
  machine with [legal-scrambler](https://github.com/docketx/legal-scrambler) so that names,
  children, SSNs, accounts and addresses never leave it; restores the answer by code afterwards.
- **`skills/divorce-in-<state>`** for each of the eight states — route a divorce question to the right row in that state's reference
  and render the answer with its verbatim statutory quote and source tag.
- **`skills/financial-disclosure-texas`** — what a Texas divorce actually requires a party to
  disclose, inventory, or produce, and when (and the honest negatives where it does not).
- **`skills/what-the-statute-says`** — for a given topic, the common assumption versus what the
  statute actually says, drawn from each row's `trap` field.

## The references, and how each was verified

| state | file | rows | authorities | verifier (corpus host, `docketx/us-statutes` + case law where cited) |
|---|---|---:|---:|---|
| Texas | `references/rows-tx-family.json` | 14 | 27 | 27/27 |
| California | `references/rows-ca-family.json` | 15 | 34 | 34/34 |
| Washington | `references/rows-wa-family.json` | 14 | 26 | 26/26 |
| Illinois | `references/rows-il-family.json` | 14 | 34 | 34/34 |
| Virginia | `references/rows-va-family.json` | 13 | 27 | 27/27 |
| Iowa | `references/rows-ia-family.json` | 15 | 23 | 23/23 |
| Utah | `references/rows-ut-family.json` | 14 | 31 | 31/31 |
| Nebraska | `references/rows-ne-family.json` | 14 | 42 | 42/42 |

Each file covers the same topics where the state's code has them: residency and venue, grounds,
the waiting period, the property regime, the division standard, spousal support / maintenance,
child support guidelines, custody or parenting arrangements, required disclosures, temporary
orders, mediation, settlement agreements, name restoration — plus what is distinctive to that state
(Texas's fault grounds and maintenance caps; California's summary dissolution and automatic
restraining orders; Washington's legal-separation conversion; Illinois's joint simplified
dissolution and irrebuttable six-month separation presumption).

Every authority's `verified.corpus` block records a `dataset`, `row_id`, verbatim `quote` and
`checked` date, and was re-derived by `the DocketX corpus verifier` (run on the corpus host against
the published corpus, never from memory), which confirms the quote occurs verbatim in the section's
published text and that the row is identifiably the section the citation names. Before each commit,
a copy with three planted defects (a changed word inside a quote, a row id repointed at a different
section, an authority relabelled to the wrong dataset) was run through the same verifier and failed
on exactly those three. See each state's commit message for the run. `CLAUDE.md` has the source-tag
vocabulary every skill uses to say, on every statement, exactly how sure it is.

## What it does not cover

- **Any state without a reference file.** Not answered from training knowledge; routed to
  CourtListener and to a licensed attorney in that state instead. The eight covered states already
  disagree on residency, grounds, division and maintenance — nothing here is safe by analogy.
- **Protective orders and safety planning.** Out of scope entirely, not just handled carefully.
  Every skill checks for this first and, when it applies, routes to the National Domestic Violence
  Hotline (1-800-799-7233 / text "START" to 88788) and to an attorney before anything else — it
  does not answer the substantive legal question, even where a verified row touches family
  violence in passing.
- **Rules of civil procedure and court forms.** The corpus behind these rows is statutes; where a
  disclosure duty lives in a court rule or an official form rather than the code, the row says so
  (Texas Rule 194; the Illinois Supreme Court financial-affidavit rule; Washington's mandatory
  worksheet form is cited from the RCW section that requires it).
- **Statute effective dates.** The store holds none; a session-by-session change can postdate a
  row. Every answer is a draft for attorney review for this reason among others.
- **Honest negatives** recorded in each file's `notes` rather than answered from memory — for
  example, whether a fault finding changes division or maintenance in Texas; any dollar or
  percentage cap on California spousal support (searched, none found); Washington's 26.19.080
  apportionment formula; any post-filing waiting period in Illinois (none found in 135 sections).

## Family-violence routing

This plugin treats family violence as a hard gate, not a topic: `CLAUDE.md`'s family-violence
caveat is checked first, before jurisdiction and before routing to any skill, and a question whose
real content is a protective order or safety planning is never answered — it is routed to the
hotline and to an attorney instead. See `CLAUDE.md` for the exact routing text every skill uses.
