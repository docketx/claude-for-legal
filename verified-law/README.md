# verified-law

Scaffolding for doing legal work with Claude where **every legal statement is either a verbatim quote
from a named published source, re-checked by machine, or is stated as not established with what was
searched.** There is no third category.

This plugin is deliberately **jurisdiction-agnostic**. It ships the structure, not the law:

- `references/SCHEMA.md` — the row schema: question, rule, trap, authorities with verbatim quotes,
  `as_of`, status, and the `notes` field where honesty lives.
- `references/VERIFICATION.md` — the verification contract, and the **break test**: three planted
  defects that must all fail, or your verifier is unproven.
- `CLAUDE.md` — the source tags (`[verified]` / `[unverified]` / `[model knowledge — verify]`), the
  jurisdiction gate, the hard stops.
- `skills/add-a-jurisdiction` — a place nobody has done yet is a morning's work, not a missing feature.
- `skills/what-the-authority-says` — answers only from installed packs, and says a useful "no".
- `skills/audit-a-pack` — adversarial audit: verification proves the quote is real, this asks whether
  the rule is a fair reading of it.
- `skills/prepare-your-file` — pseudonymise a real document on your own machine before any of the above
  reads it ([legal-scrambler](https://github.com/docketx/legal-scrambler)).

**The jurisdiction gate is the packs, not a list in a file.** A jurisdiction is covered iff a reference
pack for it is installed. Install one, and it is covered. Install none, and every skill still works —
it just tells you, precisely, what is missing and how to add it.

## Reference packs

The scaffolding is the contribution; the packs are a head start. These are built to the contract above
and are free to use, copy and extend (CC0 where the underlying law is public domain, which it is):

| pack | what it holds |
|---|---|
| **contract law** | **601 rules · 3,597 authorities · 28 jurisdictions** — indemnity, limitation of liability, liquidated damages, non-competition, warranty disclaimers, anti-assignment, arbitration, choice of law, venue, attorney fees, limitations periods, usury, statute of frauds and more |
| **family law** | **113 rules · 256 authorities · 8 states** — residency, grounds, waiting periods, property regimes, division standards, support, custody, disclosure, temporary orders, settlement |
| **court forms** | **2,044 official self-help forms and guides · 9 states**, with the court and the source URL for each |
| **attorney malpractice** | **14 rules · 34 authorities** — elements, case-within-a-case, limitations and tolling, fee forfeiture, the grievance route versus the civil route |

Every authority in every pack was re-derived from a published corpus, and every pack was break-tested
before it shipped. Where a jurisdiction's law on a point could not be established, the row says so
instead of guessing — those `notes` are the most useful part of the packs.

## Why the structure and not just the data

Data goes stale and is always partial; a legal question arrives in a jurisdiction nobody has covered
yet. What transfers is the discipline: quote or say you don't know, prove the quote mechanically, break
your own verifier on purpose, write down what you could not establish, and never reason by analogy from
the state next door. That is what is in this plugin, and it works the same whether one pack is installed
or fifty.

**Every output is a draft for attorney review, not legal advice.**
