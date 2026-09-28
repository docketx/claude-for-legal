---
name: add-a-jurisdiction
description: >
  Builds a verified reference pack for a jurisdiction nobody has done yet — the state, province or
  country the user cares about — from a published corpus of its statutes and decisions: picks the
  topics, finds and quotes the authorities, writes the rows, runs the verifier, break-tests it, and
  reports what could not be established. Use when the user asks "can you add <jurisdiction>", "this
  doesn't cover my state", "how do I extend this", "make a pack for <place>", or when a question hits a
  jurisdiction with no pack installed. The output is a pack that earns the [verified] tag, or an honest
  statement of what was missing.
argument-hint: '[jurisdiction] [area of law] [where its statutes and cases are published]'
---

# /add-a-jurisdiction

Read `../../CLAUDE.md`, `../../references/SCHEMA.md` and `../../references/VERIFICATION.md` first. This
skill is the reason the scaffolding is not jurisdiction-specific: a place with no pack is a morning's
work, not a missing feature.

## Instructions

1. **Find the corpus, and say what it is.** A pack needs machine-readable published text — statutes and,
   ideally, decisions — that can be re-read by id. An official bulk download, a public dataset, or a
   library the user already has. **If there is no such corpus, stop here and say so**: without one,
   nothing downstream can be verified, and a pack that cannot be re-derived is a list of assertions. Say
   what you looked for and what would unblock it.

2. **Check access before fetching anything.** If the corpus is on the open web: `robots.txt` is the first
   request to every host, an unreadable or challenge-page `robots.txt` is **unverifiable, not
   permissive**, and one request per second is the ceiling. Never spoof a user agent, replay a session
   token or solve a challenge — a source you can only read by pretending to be something else is a source
   you cannot honestly tell anyone you have.

3. **Pick the topic vocabulary before writing rows.** If packs are already installed, **use their topic
   names exactly** — the vocabulary is a join key (`SCHEMA.md`), and a near-duplicate name makes a rule
   invisible. A genuinely new topic is allowed; declare it, with one line on why it is not one of the
   existing ones.

4. **One row per topic, and write the honest empties too.** A topic the jurisdiction has no authority for
   gets a row with `authority: []` and `status: "no-verified-authority"`, not silence.

5. **Quote, never paraphrase, and read in context.** For each rule, find the operative text and quote it
   verbatim inside the `rule`. Read the neighbouring sections a rule's scope depends on. **A sentence
   survives only if the authority text, read in context, says it** — not if it is the sort of thing the
   statute is about.

6. **Run the verifier** over the pack: every quote re-derived, identity proven, dataset allowed. Report
   the exact `N/N`.

7. **Break-test it** — three planted defects in a scratch copy, all three must fail, nothing else may
   (`VERIFICATION.md` §5). Report the counts and delete the scratch copies.

8. **Report what you could not establish**, in `notes` and in your summary: topics with no authority in
   this corpus, statute-versus-opinion disagreements left unresolved, anything the verifier could not
   reach. This section is the measure of a good pack, not a blemish on it.

## What makes a pack good

The first jurisdiction takes a day; the tenth takes an afternoon, because the topics and the verifier are
already there. What does not get easier — and what a reviewer should look for — is whether the pack says
plainly where its own law runs out. A pack of twenty confident rows and no `notes` has not been audited.

## Worked examples

The reference packs shipped alongside this plugin were each built exactly this way, and each turned up
something the author did not expect on the way — a statute whose section number the citing opinion gets
wrong, a corpus whose metadata and body number sections differently, a section repealed since the case
that quotes it. Those are in their `notes`. Expect one per jurisdiction; be suspicious of a pack with
none.
