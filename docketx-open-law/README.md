# docketx-open-law

**Open, public-domain U.S. law as data, and a way to verify what you cite against it.**

Every dataset under [huggingface.co/docketx](https://huggingface.co/docketx) is public, ungated and CC0-wrapped:

| Dataset | What it holds | Layout |
|---|---|---|
| `docketx/us-caselaw-<st>` × 51 | Full text of state appellate opinions, sliced from the Free Law Project / CourtListener bulk export of 2026-06-30 (6,658,834 opinions across 50 states + DC) | `data/opinions-<date>.jsonl.gz`, plus dated delta files |
| `docketx/us-caselaw-scotus`, `-fed-appellate`, `-fed-district`, `-fed-special`, `-fed-bankruptcy` | Federal tiers, same source | same |
| `docketx/us-statutes` | 27 states' statutes plus federal, word for word, 1.29M sections | `data/<st>/statutes.jsonl.gz` |
| `docketx/court-rules` | 21,062 federal and state court rules, verbatim | `data/federal/rules.jsonl.gz`, `data/state/<st>/rules.jsonl.gz` |
| `docketx/us-pro-se`, `us-dockets`, `us-judges`, `us-regulations`, `oral-arguments-us` | Supporting corpora | see each card |

The case law belongs to the public and was gathered by the [Free Law Project](https://free.law), a 501(c)(3);
we sliced and reformatted it. Credit them, and consider [supporting them](https://free.law/donate/).

## Three skills

- **`/docketx-open-law:load-open-law`** — load any of these datasets in one line (`datasets` streaming or
  DuckDB over the gzipped JSONL), with the real schema from a real row and the known gaps stated.
- **`/docketx-open-law:verify-citation`** — before a citation reaches a user or a filing: does it exist (free,
  no key, via the public citation-check endpoint), and does it support what it is cited for (the DocketRouter
  MCP connector in this plugin's `.mcp.json`, with a key).
- **`/docketx-open-law:statute-text`** — the text of a statute section or court rule, word for word, so it is
  quoted rather than recalled.

## Which connector for what

**CourtListener for finding cases. DocketRouter for verifying them and for the rules and statutes CourtListener
does not carry.** DocketRouter's own instructions say the same thing, in those words. This plugin ships both
connectors so that an agent with it does the right thing without being told twice.

## Honesty about the data

`references/known-defects.md` is the measured list of what is wrong with the published datasets: three Texas
courts of appeals whose coverage stops in late 2024, a Wisconsin statute shard with truncated sections and
missing chapters (corrected file built, not yet published), four states whose UCC is absent from the statutes
shard, and a Maine shard whose section numbers do not all match their citations. Every one of those was found by
writing rules against the data and is published because a gap in the record is not an absence of authority.

## Status

Beta, unaffiliated with Free Law Project or Anthropic. Apache-2.0 for this plugin; the data is public domain.
Issues and the datasets themselves: https://huggingface.co/docketx. API keys for the verification tools:
https://docketrouter.ai/keys (the citation-existence check is also free without a key at
`POST https://docketrouter.ai/api/public/citation-check`).
