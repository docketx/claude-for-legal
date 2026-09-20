---
name: load-open-law
description: >
  Load any docketx open-law dataset from Hugging Face — a state's case law, a state's statutes, or the
  court rules — in one line, streaming, with the real row schema and the known gaps stated. Use when the user
  says "load Texas case law", "get me the Ohio statutes as data", "I want to build my own RAG over case
  law", "how do I read the docketx datasets", or wants to run legal retrieval locally or offline.
argument-hint: '[caselaw <state> | statutes <state> | court-rules [<state>|federal]]'
---

# /load-open-law

Every dataset under https://huggingface.co/docketx is public, ungated, and public domain. This skill tells
you exactly how to read one, with the schema taken from a real row rather than from memory, and it tells you
what is wrong with the data before you trust it.

## Instructions

1. **Pick the dataset from the request.**

   | Request | Dataset | File |
   |---|---|---|
   | case law for a state | `docketx/us-caselaw-<st>` (two-letter code, e.g. `tx`, `ny`, `ca`) | `data/opinions-<date>.jsonl.gz` — the base slice — plus any `data/<st>-delta-*.jsonl.gz` added since |
   | U.S. Supreme Court / federal | `docketx/us-caselaw-scotus`, `-fed-appellate`, `-fed-district`, `-fed-special`, `-fed-bankruptcy` | same layout |
   | statutes for a state | `docketx/us-statutes` | `data/<st>/statutes.jsonl.gz` (27 states + federal are held; check the card's list) |
   | court rules | `docketx/court-rules` | `data/federal/rules.jsonl.gz` or `data/state/<st>/rules.jsonl.gz` |

   If the state is not in a dataset, say so; do not substitute a neighbour.

2. **Load it streaming.** These files are large (a state's case law is hundreds of MB) — never download whole
   files to answer a question. Two ways, both run against the live files on 2026-09-20:

   ```python
   # datasets library — streams, no full download
   from datasets import load_dataset
   ds = load_dataset("docketx/us-statutes", data_files="data/tx/statutes.jsonl.gz", split="train", streaming=True)
   row = next(iter(ds))
   ```

   ```python
   # DuckDB — SQL straight over the gzipped JSONL on Hugging Face
   import duckdb
   duckdb.sql("""
     SELECT section, citation, title
     FROM read_json_auto('https://huggingface.co/datasets/docketx/us-statutes/resolve/main/data/tx/statutes.jsonl.gz')
     WHERE code = 'Business & Commerce Code' LIMIT 5
   """).show()
   ```

3. **Know the schema — this is what a real row looks like.**

   **Statutes** (`docketx/us-statutes`, read 2026-09-20 from `data/tx/statutes.jsonl.gz`, row 1):
   ```json
   {"id": "tx-stat_242d02d0f74eec18", "level": "state", "jurisdiction": "tx",
    "code": "Agriculture Code", "section": "1.001", "citation": "Tex. Agric. Code § 1.001",
    "title": "Tex. Agric. Code § 1.001", "text": "Sec. 1.001. PURPOSE OF CODE. (a) This code is enacted as a p…",
    "source": "https://statutes.capitol.texas.gov/", "license": "public-domain-government-work",
    "retrieved_at": "2026-08-28", "extra": "{\"chapter\": \"1\", \"code\": \"AG\", \"unit\": \"section\"}"}
   ```
   Every column is a string, and `extra` is a **JSON string** you must `json.loads` yourself. A section
   number is unique only *within a code*: Texas' `§ 1.001` exists in its Agriculture, Education and Civil
   Practice Codes and more. Always key on `(code, section)` or on `citation`, never on `section` alone.

   **Case law** (`docketx/us-caselaw-tx`, read 2026-09-20 from `data/opinions-2026-06-30.jsonl.gz`, row 1):
   ```json
   {"id": "c79481b94f844406", "doc_type": "opinion", "jurisdiction": "tx",
    "title": "Stefanie M. Helmuth v. Pennymac Loan Services, LLC", "text": "…",
    "source": "cl-bulk://2026-06-30/opinions/11264194", "license": "public-domain-edict-of-government",
    "retrieved_at": "2026-06-30", "citation": "", "court": "tx-txctapp1", "date": "2026-02-19",
    "extra": {"cl_opinion_id": "11264194", "cluster_id": "10797521", "opinion_type": "020lead",
              "text_kind": "plain_text", "source_label": "courtlistener"}}
   ```
   Here `extra` is a **JSON object** in the raw file. **`citation` is empty in every case-law row measured.**
   Reporter citations for these opinions come from CourtListener's citation map keyed on `extra.cluster_id`;
   do not tell a user a case has no citation because this field is blank. `opinion_type` distinguishes a lead
   opinion (`020lead`) from concurrences and dissents — a quote from a dissent row is not the holding.

4. **State the known gaps before you state what the data holds.** Read `references/known-defects.md` and
   surface every entry that touches the dataset the user asked for. Say it in one sentence up front:
   "Note: this dataset's Fifth Court of Appeals coverage stops at 2025-08-26 — nothing after that from that
   court is a gap in the record, not an absence of authority."

5. **Credit the source.** The case law is the Free Law Project / CourtListener public record; say so when you
   hand a user the data, and point at https://www.courtlistener.com for search and https://free.law/donate/.

6. **Never present a row as verified law.** These are public-domain documents held word for word. A row's
   text is what was published; whether a case is still good law, or a statute still in force, is not in the
   row. For a citation you are about to rely on, run `/docketx-open-law:verify-citation`.
