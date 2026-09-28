# The row schema

One row per (jurisdiction, topic, rule). A topic with no verified authority in a jurisdiction still gets a
row — with `authority: []` and `status: "no-verified-authority"`. **A missing row and a padded row are
both worse than an honest empty one**, because a missing row reads as "nothing to say here" and a padded
row reads as law.

```json
{
  "jurisdiction": "tx",
  "topic": "limitation-of-liability",
  "rule_id": "tx-lol-fair-notice",
  "question": "Will a court enforce this cap against the drafter's own negligence?",
  "rule": "What the law actually requires, in the reader's words, with the operative text quoted inside it.",
  "trap": "What makes a clause that reads correctly elsewhere fail HERE specifically.",
  "authority": [
    {
      "kind": "case",
      "cite": "725 S.W.2d 705",
      "name": "Ethyl Corp. v. Daniel Construction Co.",
      "court": "Tex.",
      "decided": "1987",
      "verified": {
        "checked": "2026-09-14",
        "corpus": {
          "dataset": "docketx/us-caselaw-tx",
          "row_id": "791d949e432a5233",
          "quote": "The express negligence doctrine provides that parties seeking to indemnify …",
          "checked": "2026-09-16"
        }
      }
    }
  ],
  "as_of": "2026-09-16",
  "status": "verified",
  "notes": "Anything the verification could NOT establish."
}
```

## Fields

| field | required | what it means |
|---|---|---|
| `jurisdiction` | yes | lowercase code — a state (`tx`), a country's federal tier (`us`), or whatever your corpus uses. **A rule with no jurisdiction is not a weaker rule; it is a sentence that is wrong somewhere.** |
| `topic` | yes | the family this rule belongs to, from the pack's own vocabulary. See "the vocabulary is a join key" below. |
| `rule_id` | yes | unique per (jurisdiction, topic). The same id in two jurisdictions is two rules. |
| `question` | yes | the question a reader actually asks, in their words. |
| `rule` | yes | the answer, with the operative text quoted inside it. Customer-facing: no internal vocabulary. |
| `trap` | no | the thing that makes a facially normal instrument fail here. Omit rather than invent. |
| `authority[]` | yes | may be empty, but then `status` must say so. |
| `as_of` | yes | when this rule statement was established. A rules store is bitemporal by nature; undated rows go stale silently, which is worse than being wrong loudly. |
| `status` | yes | `verified` \| `partial` \| `no-verified-authority` \| `no-verified-date` |
| `notes` | no | what verification could **not** establish. This field is where honesty lives; use it. |
| `applies_to` | no | optional closed vocabulary limiting the rule to a kind of matter. **Absent means "applies to any"**, and absent is the default. A wrong tag hides law; an untagged row is only noise. |

## The vocabulary is a join key

A skill classifies a question into a topic and then looks up that topic's rows for the jurisdiction. **A
rule filed under a near-duplicate name is invisible to it** — the answer becomes "no verified rule" while
the pack holds one. Measured once and it cost four jurisdictions' worth of rows: `limitation-period`
beside `limitations-period`, `electronic-signatures` beside `electronic-signature`, `forum-selection`
beside `venue-selection`, `trade-secrets` beside `trade-secret-confidentiality`.

Keep the vocabulary open so a genuinely new topic can be added — and then **pin it with two tests**: a
topic used by only one jurisdiction must be declared with a reason, and no two topics may differ only by
plural, hyphenation or a shared stem. Both should fail when you re-introduce a near-duplicate; if they
don't, they aren't tests.

## Customer-facing text

`question`, `rule` and `trap` are read by the person asking. They carry no internal vocabulary — no
"this row", no corpus, shard or index names, no research narration. That belongs in `notes`. Pin it with a
test, because it is the kind of thing that leaks back in every time a new author writes a pack.

## What the verification does and does not prove

- A citation check proves an authority **exists**. It does not prove it says what the rule says.
- A corpus quote proves the authority **says the words**. It does not prove the rule is a correct reading
  of them — that is what an adversarial audit is for.
- Statute rows in most published corpora carry **no effective date**. A pack therefore usually cannot
  answer "what did the law say on date X".
- Where the row's own claim is weaker than it looks, `notes` says so. An honest "we could not establish
  this" row is publishable; a row dressed up as checked is not.
