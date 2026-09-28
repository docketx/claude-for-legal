# The verification contract, and the break test that proves it

A pack may carry the `[verified]` tag only if everything here is true of it. The last section is the one
that is usually skipped, and it is the one that makes the rest mean anything.

## 1. Every quote is re-derived, never pasted

For each authority, a program reads the named `dataset` + `row_id` out of the published corpus and checks
that the `quote` occurs **verbatim** in that row's own text, whitespace-normalised and nothing else
normalised. Not "contains the gist", not "matches after cleaning". If the corpus has an OCR artefact, the
quote carries the artefact — reproduce it and say so in `notes` rather than silently correcting the
source, because a quote you edited is a quote you cannot re-derive.

## 2. Identity: the row must be the authority it is cited as

Checking that a quote appears *somewhere* proves nothing about attribution. The row id must be
identifiable as the section or opinion the citation names, by at least one independent witness:

- the row's own citation field matching the claimed cite character for character, or
- the section number parsed out of the row's body text, or
- for a case, the case name matching the corpus title **exactly** as the corpus spells it — not as the
  reporter does. (`Groce, Locke & Hebdon`, not `Groce, Locke & Purnell`. `Enters.`, not `Enterprises`.)

When one witness disagrees for a structural reason — a corpus that writes `RS 9:3500` in metadata while
the body opens `§3500.` — record the exception with its cause. **Do not widen the check.** An allow-list
entry naming a known format gap is honest; a looser rule is a weaker test forever.

## 3. The dataset must be allowed for the jurisdiction

A `tx` rule cannot cite a `ca` case corpus. Federal tiers are allowed for every jurisdiction. This sounds
too obvious to test until a pack author relabels an authority to make a check pass.

## 4. Verified means every authority, not the best one

`status: "verified"` requires: at least one authority, **and every authority verified**. One unverified
authority in a row makes the row `partial`. A rule served with a mix is a rule whose reader cannot tell
which half was checked.

## 5. The break test — three planted defects, before every commit

Take a scratch **copy** of the pack and plant exactly three defects:

1. **Change one word inside one quote.** Tests re-derivation (§1).
2. **Repoint one authority's `row_id`** at a different section of the same corpus. Tests identity (§2).
3. **Relabel one authority's `dataset`** to a corpus it does not belong to. Tests §3.

Run the verifier on the copy. **All three must fail, and nothing else may.** If fewer than three fail, the
verifier does not check what you believe it checks; if extra rows fail, it is flaky and its passes mean
nothing either. Record the exact counts in the commit message (`N/N` clean, `N-3/N` with the defects), and
delete the scratch copies afterwards — on every machine you copied them to.

A verifier that has never been broken on purpose has never been tested. It is the difference between "the
checks pass" and "the checks would catch it".

## 6. Verify the instrument before you believe a surprising number

Every measurement here has been wrong at least once in a way that looked plausible:

- a corpus scan reported "no UCC language in any of 27 shards" — it was reading compressed blobs;
- a count of ten flagged rows matched the expected ten exactly — different ten, and two were full drug
  schedules the heuristic mistook for renumbering notes;
- an unanchored log filter (`grep -v 'mux|Control|Warning'`) deleted a real statute section titled
  "Control of controllable electronic record" and turned 28 sections into 27, in a number that reached
  four published rows before it was caught.

So: **give every scan a positive control** — a case it must flag, and a case it must not — and run the
control first. If a number surprises you, suspect the instrument before the world. A scan whose control
fails prints nothing at all; a scan that reports a count it cannot defend is worse than no scan.

## 7. Record what verification could not establish

Not found is a finding. `notes` carries what was searched and did not exist: no appellate opinion on the
point in this corpus, a statute-versus-opinion numbering disagreement left unresolved, a rule set the
verifier cannot reach. A pack whose `notes` are all empty has not been audited; it has been asserted.
