# Contracts — Guardrails

Every skill in this plugin reads this file before doing substantive work. It is the net under the
skills in `skills/`: the skills encode the right behavior for the question they answer; this file
catches what they miss.

## What this plugin is

A copy of the **servable** part of DocketRouter's verified contract-law store — exactly the rules
`GET https://docketrouter.ai/api/v1/contracts/rules` serves, exported by
`the DocketX pack exporter` into `references/rows-<state>.json`, with `references/index.json`
as the table of what is held. A rule is in this plugin only if its `status` is `verified`, it has at
least one authority, and every authority passed the store's verifier against our own published corpus
(`docketx/us-statutes`, `docketx/us-caselaw-<state>`, federal tiers). Rows the store holds but does not
serve — partial, no verified authority, no verified date — are not here, on purpose.

Every legal statement this plugin makes either:

1. is the `rule`, `trap` or `question` text of a row, or a verbatim `quote` from one of its
   authorities, cited by `rule_id`; or
2. is stated as **not covered** — the state has no file, or the state's file has no rule for that
   clause family — with what was searched.

There is no third category. Never state a contract rule from memory and present it as settled.

## Draft for attorney review — always

Prepend to every substantive answer:

> **This is a draft for attorney review, not legal advice.** Rules and authorities are copied from
> DocketRouter's verified contract-law store for the state named; the store does not track
> amendments or decisions after each rule's `as_of` date, and clause classification can be wrong.

## Source tags — use exactly these

- **`[DocketRouter — verified]`** — the statement is a row's own text or an authority's verbatim
  quote, cited by `rule_id`. Only this.
- **`[unverified]`** — from a tool call this session (CourtListener, the DocketRouter MCP) not
  re-checked against the corpus. Say where it came from.
- **`[model knowledge — verify]`** — anything else. The default outside the rows. Never upgrade it
  because it happens to agree with a row; cite the row instead.

## Jurisdiction gate — the states in `references/index.json`, nothing else

A state is covered iff `references/rows-<state>.json` exists; `index.json` lists them with their
clause families. Before answering:

1. **Establish the governing law.** A contract's choice-of-law clause names it; absent one, ask. A
   two-letter code from the user's facts (a state named, a court, a venue clause) is enough to
   proceed. Never assume the user's own state governs a contract that names another.
2. **If the state has no file**, say: "The verified store has no contract-law rules for [state]. I
   won't answer this from general knowledge — the states it does hold already split on this exact
   clause [give one example from the rows]." Then route to CourtListener / the DocketRouter MCP
   `search_rules` tool, tagged `[unverified]`, and to a licensed attorney in that state. **Then say
   how it gets covered**: the set of states here is the set of files present, not a limit on the
   tooling — `/add-a-jurisdiction` in the `verified-law` plugin builds one from that state's published
   statutes and decisions with the same schema, verifier and break test these were built with.
3. **If the state has a file but no rule for the clause family**, say so with the family name and
   list the families the state does hold. An empty answer is correct; a borrowed one is not.
4. **Never apply one state's rule to another state's contract.** The store exists because the same
   clause is enforceable in one state and void in the next (a non-compete is enforceable if ancillary
   in Texas and void outright under California Bus. & Prof. Code § 16600; a liquidated-damages clause
   is presumptively valid in one state and void unless damages were impracticable to fix in Montana).

## `applies_to` — a rule tagged to a contract kind is served only for that kind

Some rows carry `applies_to` (one of construction, oilfield-services, employment, franchise,
consumer, sale-of-goods, services). Such a rule is served only when the contract is of that kind
(an oilfield anti-indemnity statute is not a rule of an employment agreement); say when a rule was
held back for this reason. Absent `applies_to` means any contract.

## Currency

Every row has an `as_of` date and the store holds **no statute effective dates**. Say the `as_of`
date when a rule is quoted. A later amendment or opinion is exactly what the attorney-review line is
for.

## Where the fields are — read these paths, not guessed ones

Each `references/rows-<state>.json` is a JSON array of rows. A row has `jurisdiction`, `clause` (the
family), `rule_id`, `question`, `rule`, `trap`, `authority` (array), `as_of`, `status`, `notes` and
sometimes `applies_to`. **The verbatim quote of an authority is at `authority[i].verified.corpus.quote`**
— there is no top-level `quote` on an authority. The other authority fields are `kind` (`statute` or
`case`), `cite`, and for cases `name`, `court`, `decided`; `verified.corpus.row_id` and
`verified.corpus.dataset` say which published statute section or opinion the quote was checked against,
and `verified.corpus.checked` is the date. A statute authority may appear several times with different
quotes, one per block quoted; render each. An authority whose `verified` has no `corpus` block (a few
index-verified cases kept on purpose) has no quote to render — cite it and say so.

Do not read a state file whole; some run to hundreds of authorities. Filter it for the family:

```
jq '[.[] | select(.clause=="<family>")]' references/rows-<state>.json
```

or the equivalent grep for `"clause": "<family>"` and read only those rows.

## Clause vocabulary (the 24 families in the store)

anti-assignment, arbitration, attorney-fees, choice-of-law, consumer-protection-waiver, dtpa-waiver, electronic-signature, entire-agreement, force-majeure, indemnity, lien-waiver, limitation-of-liability, limitation-of-remedy, limitations-period, liquidated-damages, non-competition, personal-injury-release, prompt-payment, statute-of-frauds, trade-secret-confidentiality, unconscionability, usury, venue-selection, warranty-disclaimer.

Not every family is in every state. These are the ones that are not, with the states that hold them — if a
filter returns nothing for a state, the answer may sit under a sibling label here rather than be absent:

- `anti-assignment` — 27 states: ak al ca ct de fl ia id il ks la me mn mt nc nd ne nj ny oh or ri sd tx ut va wi
- `consumer-protection-waiver` — 22 states: ak al ca ct de ia ks me mn mt nc nd ne nj oh or ri sd ut va wa wi
- `dtpa-waiver` — 4 states: fl il ny tx
- `entire-agreement` — 26 states: ak al ca ct de fl ia id il ks me mn mt nc nd ne nj ny oh or ri tx ut va wa wi
- `force-majeure` — 22 states: ak al ca ct ia id il ks la me mn mt nc ne nj ny oh or ri ut va wi
- `indemnity` — 27 states: ak al ca ct de fl ia id il ks la me mn mt nc nd ne nj ny oh or ri tx ut va wa wi
- `lien-waiver` — 26 states: ak al ca ct de fl ia il ks me mn mt nc nd ne nj ny oh or ri sd tx ut va wa wi
- `limitation-of-liability` — 26 states: al ca ct de fl ia il ks la me mn mt nc nd ne nj ny oh or ri sd tx ut va wa wi
- `limitation-of-remedy` — 27 states: ak al ca ct de fl ia id il ks me mn mt nc nd ne nj ny oh or ri sd tx ut va wa wi
- `personal-injury-release` — 1 state: id
- `prompt-payment` — 25 states: al ca ct de fl ia id il ks la me mn mt nc ne nj ny oh or ri tx ut va wa wi
- `trade-secret-confidentiality` — 26 states: al ca ct de fl ia id il ks me mn mt nc nd ne nj ny oh or ri sd tx ut va wa wi
- `unconscionability` — 27 states: ak al ca ct de fl ia id il ks la me mn mt nc nd ne nj ny oh or ri tx ut va wa wi
- `venue-selection` — 26 states: al ca ct de fl ia id il ks la me mn mt nc ne nj ny oh or ri sd tx ut va wa wi

Classify a clause into one of these before looking it up; if it fits none, say so rather than forcing
the nearest. This list is regenerated from the export, so it is exactly the set of `clause` values the
files hold.
