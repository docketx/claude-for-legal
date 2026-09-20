# Known defects in the docketx open-law datasets

Measured, dated, and published because a gap in the record is not an absence of authority. Each entry names
how it was measured. If you find another, open an issue at https://huggingface.co/docketx.

## Case law

- **Three Texas courts of appeals stop early** (`docketx/us-caselaw-tx`, measured 2026-09-13, on the dataset
  card): Fifth COA (Dallas) newest opinion 2025-08-26, Fourteenth COA (Houston) 2024-12-06, Twelfth COA
  (Tyler) 2024-11-27. 14.1% of the dataset. The gap is upstream in CourtListener's own export and live API,
  so a refresh will not close it.
- **Nebraska is missing 2011 and 2012 entirely** (`docketx/us-caselaw-ne`, measured 2026-09-20 by grouping
  all 51,154 rows on year): 0 opinions in each, the only zero years between 1871 and 2026, and 2010 holds 111
  against 275–395 on either side. Roughly 750 opinions, both courts.
- **The `citation` field is empty in every case-law row measured** (all state shards, 2026-09). Reporter
  citations come from CourtListener's citation map keyed on `extra.cluster_id`. Blank does not mean
  uncited.

## Statutes

- **Wisconsin is truncated and incomplete** (`docketx/us-statutes`, `wi`, measured 2026-09-19 and
  2026-09-20): 1,002 of 14,534 sections end mid-sentence at a cross-reference and 149 carry only a heading,
  and the chapter table of contents was read through a ~50-entry window so every larger chapter lost its
  later sections — chapter 402 has 104 sections at the source and 60 published. A corrected 18,219-row file
  (damage 1,151 → 13) is built and verified but **not yet published**; what is served is the damaged file.
- **The UCC is absent from four states' statute shards** (measured 2026-09-20 by probing every row's text
  for the Code's own language — "mention merchantability", "fails of its essential purpose", "account
  debtor"): `ct`, `mn`, `ne`, `va`. **Nebraska is the dangerous one**: its chapters run 1–90, so a UCC
  section number returns a *real but unrelated* section — § 2-302 is Agriculture, § 9-406 is Bingo. Rhode
  Island's Title 6A holds 28 sections (Articles 5, 6 stub, 12); Articles 1, 2, 2A, 3, 4, 4A, 7, 8 and 9 are
  absent.
- **Maine publishes 595 sections under a number that is not a citation** (`me`, measured 2026-09-20): in
  Titles 18-A (572) and 11 (23), which number sections in two groups, a third group was appended —
  `11 M.R.S. § 2-102-5` is § 2-102, `18-A M.R.S. § 1-101-2` is § 1-101 — and the correct two-group section
  exists nowhere in the title. **Do not generalise this shape**: Alabama, Rhode Island, Utah, North Carolina
  and Kansas number by title-chapter-section, and their 64,000 N-N-N sections are correct.
- **A section number is unique only within a code.** Texas' `§ 1.001` exists in 30 codes. Maine (single
  code) has 28,706 rows and Delaware 13,847 whose section number does not identify them. Key on
  `(code, section)` or `citation`.
- **Louisiana** (`la`): the row's own text disagrees with its `section` field on 46,431 of 52,622 rows —
  civil-code article numbering. Treat `section` as unreliable there and read the text.

## What has been checked and is clean

Measured 2026-09-20 across all 27 statute shards: no duplicate section numbers within a code in `ak`, `al`,
`ct`, `fl`, `ia`, `id`, `il`, `ks`, `la`, `mn`, `mt`, `nd`, `ne`, `ny`, `or`, `ri`, `sd`, `ut`, `va`, `wa`, `wi`;
no `[Effective …]` merge markers except `de` (367), `ri` (137), `al` (4), `id` (2).
