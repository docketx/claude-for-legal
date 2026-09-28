---
name: which-form-where-to-file
description: >
  Given a state and a family-law situation, lists the official court self-help guides,
  instructions and forms held for that state -- title, kind, court, source_url and license --
  so a self-represented person knows which document to look at and which court publishes it.
  Built from docketx/us-pro-se (19 state court self-help publishers), not from any statute or
  case-law corpus. Use when the user asks "which form do I need", "where do I file", "does my
  state have a form for X", "what does the court call this document", or names a state plus a
  family-law topic (divorce, custody, child support, spousal support, paternity, name change,
  adoption, property division, fee waiver, service of process). Does not state what the law is,
  does not answer protective-order or safety-planning questions, and never invents a form number.
argument-hint: '[a two-letter state code and a family-law situation, plain text]'
---

# /which-form-where-to-file

Lists official court self-help guides, instructions and forms for a state and topic from
`../../references/forms-<st>.json`, never from memory. This skill is a **forms locator**, not a
rules-of-law skill -- it never states what a statute requires, only which official document a
court publishes and where. Read `../../CLAUDE.md` before running this skill for the source-tag
vocabulary, the family-violence caveat, and the honest-negative rule, all of which apply here.
The Texas-only jurisdiction gate in `../../CLAUDE.md` governs the rules-of-law skills built on
`rows-<st>-family.json`; this skill's own jurisdiction scope is the state list in step 1 below,
independent of that gate.

## What this is built from, and its limits

`docketx/us-pro-se` is **12,103 official court self-help documents from 19 state court
systems** -- self-help guides, instructions and forms, held verbatim with the exact URL each came
from. The dataset's own README says this plainly, and every answer from this skill must carry the
same words:

> **This is not the law.** Forms and instructions are not statutes and not case law. A court's
> self-help page explains a procedure; it does not create one, and it can be out of date or wrong
> about your situation. Nothing here should be quoted as authority, cited as law, or treated as
> legal advice.

Two more limits, carried from the dataset card, worth knowing even though neither currently
affects a shipped `forms-<st>.json`:

- **Iowa's records are retrieval/reference only, not training data.** Iowa's `robots.txt`
  carries `Content-Signal: search=yes,ai-train=no,use=reference`, stamped into every Iowa
  record's own `license` field. (Iowa's own family-law record count in the dataset is 7-8 after
  the topic-selection regex in `an internal DocketX tool` -- below the 10-record floor
  for a `forms-ia.json` of its own, so this reservation is background, not something this skill
  currently has to enforce.)
- **Maryland's forms library is excluded, not thin.** `mdcourts.gov/robots.txt` disallows
  `/courtforms`, so the Maryland court-forms library was never crawled; the few Maryland records
  held are self-help pages only.

## States held (as of the last build)

Run `an internal DocketX tool` to reproduce these numbers from the dataset directly.
As of the last build, nine states cleared the bar for their own `forms-<st>.json`:

| State | File | Records | Notes |
|---|---|--:|---|
| California | `forms-ca.json` | 666 | Deepest coverage; all 8 topics represented |
| Washington | `forms-wa.json` | 473 | All 8 topics represented |
| Wisconsin | `forms-wi.json` | 271 | All 8 topics represented |
| Hawaii | `forms-hi.json` | 211 | Many PDF titles are extraction artifacts ("Name, Address & Phone Number") -- see "Bad titles" below |
| Nebraska | `forms-ne.json` | 180 | All 8 topics represented |
| South Dakota | `forms-sd.json` | 97 | No paternity-specific record beyond a few; check `custody-parenting` too |
| Utah | `forms-ut.json` | 94 | All 8 topics represented |
| Virginia | `forms-va.json` | 43 | Thin on divorce (2); most of Virginia's held divorce material lives in `rows-*-family.json` for other states, not here |
| Illinois | `forms-il.json` | 9 | **Thin.** Only `divorce`, `custody-parenting` (guardianship-of-a-minor and one procedural rehearing form) and `name-change` topics are represented -- no held record for child-support, spousal-support, paternity, property, protective-order, fee-waiver or service in Illinois. Say so plainly if asked about one of those. |

Every other state in the dataset -- Alabama, Delaware, Idaho, Indiana, Iowa, Kentucky, Maine,
Maryland, Oregon, Rhode Island -- has either no family-law-topic records after selection, or too
few (under 10) to justify its own file, **or** (Delaware specifically) failed a manual precision
check: Delaware's 20 regex-matched records turned out on reading to be generic Delaware
civil-court navigation pages, a glossary, and a broken link that mention "custody" or "child
support" only in boilerplate nav menus or one-line case-type examples, not real family-specific
guidance. That is an honest negative, not a gap to quietly paper over.

If asked about a state with no `forms-<st>.json`: say plainly, **"No official forms held for
[state]."** Name the states that are held (the table above). Never invent a form number or a
court name to fill the gap.

## Instructions

1. **Family-violence check, first, before anything else.** If the situation is actually about a
   protective order, restraining order, domestic violence, or "am I safe" -- not "which form do I
   file" -- stop and follow the family-violence caveat in `../../CLAUDE.md` verbatim:

   > If you are in danger right now, call 911. For confidential support, safety planning, and
   > local resources, the National Domestic Violence Hotline is 1-800-799-7233 (or text "START"
   > to 88788), available 24/7. Please also talk to a licensed attorney -- a family-law or
   > domestic-violence advocacy organization can often connect you with one at low or no cost.

   Do this **even if the state's `forms-<st>.json` holds a matching `protective-order`-topic
   record.** Do not list those records. Do not name the form number. The hotline line is the
   whole answer for that part of the question; nothing else about protective orders gets
   answered here, regardless of how the question is phrased.

2. **Identify the state.** If the user doesn't name one, ask. Map it to a two-letter code and
   check the table above. If there is no `forms-<st>.json` for that state, say so in the exact
   words above and stop -- do not answer from another state's file and do not answer from general
   knowledge.

3. **Load the file.** Read `../../references/forms-<st>.json` in full. Each record has `id`,
   `kind` (`self-help-guide` / `instructions` / `form`), `title`, `court`, `source_url`,
   `license`, and `topic` (one of `divorce`, `custody-parenting`, `child-support`,
   `spousal-support`, `property`, `protective-order`, `name-change`, `paternity`, `fee-waiver`,
   `service`, `general`).

   Note on `custody-parenting`: this bucket also holds guardianship-of-a-minor and
   conservatorship-of-a-minor records. The fixed topic list has no separate slot for
   guardianship, and a minor guardianship petition is close enough in kind -- who has legal
   authority over a child -- to file it there rather than invent a category the reference schema
   doesn't have. Say so if the user specifically asked about guardianship and the matches you
   found are guardianship-flavored, so they aren't surprised the topic label says
   "custody-parenting."

4. **Match the situation to a topic**, then filter the state's records to that `topic`. A
   question can span more than one topic (e.g., "filing for divorce with kids" touches `divorce`
   and likely `custody-parenting` and `child-support`) -- return matches from every topic the
   question actually touches, not just the first one.

   If nothing in the file matches the topic asked (for example, Illinois and child support, per
   the table above), say so directly: name the topics that state's file *does* cover, per the
   table, rather than stretching a `general`-topic or wrong-topic record to fit.

5. **Render the answer.** For each matching record:
   - Give the `title` (verbatim), the `kind`, and the `court`.
   - Give the `source_url` as a citation, but never fetch it -- this skill answers only from the
     dataset's own recorded metadata, never from a live fetch of a court website.
   - Note the `license` if it carries a reservation worth surfacing (for example, a "no license
     asserted" note is normal and doesn't need calling out every time; an explicit training-use
     or resale restriction would).
   - **Bad titles**: a large share of this dataset's PDF-sourced titles are extraction artifacts
     -- the literal first line of a fillable form, like "Name, Address & Phone Number" or "STATE
     OF HAWAI'I CASE ID/NUMBER" rather than a descriptive name. This is most common in Hawaii and
     Wisconsin. When a matched record's title is one of these, say so plainly ("this dataset's
     title field for this record is a form-header artifact, not a descriptive name") and lean on
     `court` + `source_url` + the surrounding topic match to describe what the document actually
     is, rather than presenting the garbled title as if it were informative.
   - Never invent, guess, or "helpfully" supply a form number, revision date, or court division
     that isn't in the record itself.

6. **Close every answer** with the dataset's own words: **"Forms and instructions are not the
   law"** -- state plainly that a form or instructions page explains a procedure but doesn't
   create one, can be out of date, and is not a substitute for a licensed attorney's review, using
   the quoted block above or a direct paraphrase of it. This is not optional and does not get
   dropped for brevity.

## Output

For a matched state/topic: a short list of records (title, kind, court, source_url, license note
if any), grouped by topic if more than one topic matched, followed by the "forms and instructions
are not the law" line. For an unmatched state: "No official forms held for [state]," naming the
states that are held. For a protective-order question: the hotline line alone, nothing else.
