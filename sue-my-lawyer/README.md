# sue-my-lawyer

A reference plugin, for the claude-for-legal suite, over what a client can do about their OWN
lawyer — built to the same verification standard as DocketRouter's contract-law store and the
family-legal plugin: every legal statement is either a quote copied from our own published corpus
and re-checked verbatim against it, or it is stated as not established, with what was searched.

## What it is

Four skills over one verified reference file:

- **`skills/sue-my-lawyer`** — the front door: routes a question to the right topic (malpractice
  elements, causation, limitations, fee disputes, the client file, criminal-case doctrines) and
  renders the answer with its verbatim quote.
- **`skills/grievance-or-lawsuit`** — the two separate routes (a State Bar grievance versus a
  civil lawsuit or fee-dispute procedure), their different outcomes, and that filing one is not
  filing the other.
- **`skills/get-my-file`** — what is actually established about getting the client file back and
  fee disputes, including the honest negatives where this plugin's tooling cannot verify a rule
  that likely exists.
- **`skills/was-my-defense-ineffective`** — for criminal clients: the Peeler doctrine (civil
  malpractice route) and Strickland v. Washington (the federal constitutional standard), kept in
  clearly separate sections, with routing toward a criminal-appeals or post-conviction attorney.

The reference itself is `references/rows-tx-sue-my-lawyer.json`: **14 rows, 31 verified
authorities, all `status: "verified"`**, covering the elements of legal malpractice, case-within-a-case
causation, the expert-testimony requirement, the anti-fracturing rule, breach of fiduciary duty
and fee forfeiture, the two-year limitations period, the Hughes tolling rule (with Apex Towing and
Erikson v. Renda refinements), the discovery rule, the Peeler doctrine, Strickland ineffective
assistance, the client's right to the file, fee disputes and the State Bar grievance process,
assignability of malpractice claims, and attorney-client privilege after the relationship ends.

Every authority's `verified.corpus` block was re-checked with
`the DocketX corpus verifier` against `docketx/us-statutes` (jurisdiction `tx`),
`docketx/us-caselaw-tx`, and `docketx/us-caselaw-scotus` on 2026-09-21: **31/31 corpus quotes
verified**. See `CLAUDE.md` for the source-tag vocabulary this plugin uses to say, on every
statement, exactly how sure it is.

## What it covers

**Texas only, today**, plus one deliberately narrow federal exception. Every row's `jurisdiction`
is `"tx"` except the Strickland row, which is `"us"` and cites `docketx/us-caselaw-scotus` because
Strickland is the controlling federal constitutional standard, not a second state of general
coverage. `CLAUDE.md`'s jurisdiction gate requires every skill to confirm Texas before answering,
and to say plainly — not answer from general knowledge — when a question names a different state.
Legal malpractice elements, limitations periods, fee-dispute procedures, and the assignability of
malpractice claims all vary by state, so this plugin never extends a Texas answer to another
state's facts.

## What it does not cover

- **Any state other than Texas.** Not answered from training knowledge; routed to CourtListener
  and to a licensed attorney in that state instead.
- **Opining on a specific real lawyer or firm.** This plugin explains the law and the routes; it
  never names, characterizes, or opines on whether a specific real lawyer or firm committed
  malpractice or breached a duty. See `CLAUDE.md`'s "What this plugin is never for" section.
- **The Texas Disciplinary Rules of Professional Conduct and Rules of Disciplinary Procedure as a
  directly cited dataset.** Confirmed NOT held for Texas in `docketx/court-rules` (measured
  2026-09-21: that file's rule_sets are `trcp`, `trap`, `tre`, `txcprc`, `txbcc`, `txprop`, and
  `txfin` — no `tdrpc` or `trdp`). The `right-to-the-file` row instead quotes TDRPC Rule 1.15(d) as
  a Texas court of appeals itself quoted it, in *In re Cokinos, Boisien & Young* — a verified
  case-law quote, not a verified rules-corpus citation.
- **`docketx/court-rules` generally, as a verified corpus dataset.** Even where that dataset does
  hold Texas material (the Rules of Evidence, for example — Rule 503's privilege exceptions were
  read directly from the published file), `the DocketX corpus verifier` does not support
  `docketx/court-rules` as a dataset at all. This is the same tooling gap already documented for
  `plugins/family-legal`'s Illinois financial-affidavit row. The `attorney-client-privilege-after-relationship-ends`
  row states exactly what was read and why it isn't cited as a verified authority.
- **Day-to-day grievance-filing mechanics** (where to file, what form, what deadline) — those live
  in the Texas Rules of Disciplinary Procedure, covered by the same tooling gap above.
- **A specific dollar amount or percentage for fee forfeiture**, the full multi-factor Burrow test
  restated in full, and whether a Strickland victory on its own satisfies Peeler's exoneration
  requirement — each named as a gap in the relevant row's `notes`.

## How every statement is verified

`references/rows-tx-sue-my-lawyer.json` follows the same schema as DocketRouter's
`data/contract-law/rows-*.json` and `plugins/family-legal`'s reference files: each row's
`authority[].verified.corpus` block records a `dataset`, `row_id`, verbatim `quote`, and `checked`
date (plus `cl_opinion_id` for a case authority). `the DocketX corpus verifier` (run on
the corpus host against the published corpus, never from memory) re-derives every quote from the row id and
confirms it occurs verbatim, whitespace-normalized, in the source's own published text, and that
the row is identifiably the authority the citation names.

A copy of this reference with three planted defects (a changed word inside a quote, a quote
repointed at a different section's row id, and an authority relabeled to the wrong dataset) was
run through the same verifier as a break test on 2026-09-21 and failed on all three (28/31, with
exactly the three planted defects as the failures) before the planted copy was deleted from both
the Mac and the corpus host — see the commit message for the exact run.

## Grievance versus lawsuit

`CLAUDE.md` treats this as a standing rule, not a topic-by-topic judgment call: any answer that
touches a State Bar grievance or a civil claim must say which one it is talking about, and must
say that filing a grievance is not the same as filing a lawsuit and does not by itself get the
client any money. See the `fee-disputes-and-grievance-process` row and `skills/grievance-or-lawsuit`.

## Limitations periods

`CLAUDE.md` singles this out as the single most dangerous fact in this plugin: limitations and
tolling rules are stated only as verbatim quotes, every limitations answer says the clock may
already be running, and every limitations answer tells the user to consult a legal malpractice
attorney now — not as boilerplate, but because a delayed or wrong answer here can be unrecoverable
in a way most of this plugin's other content is not.

## Criminal cases

The Peeler doctrine (the Texas rule barring a civil malpractice suit by an unexonerated former
criminal defendant) and Strickland v. Washington (the federal constitutional ineffective-assistance
standard) are different tests for different purposes and are never blended into one answer. See
`CLAUDE.md`'s "Criminal cases" section and `skills/was-my-defense-ineffective`.
