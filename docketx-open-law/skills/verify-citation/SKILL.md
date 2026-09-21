---
name: verify-citation
description: >
  Before a citation reaches a user or a filing: does it exist, and does the cited opinion actually support
  the proposition it is cited for. Uses the DocketRouter MCP connector's check_citations and check_support.
  Use when the user says "check these cites", "is this case real", "does Smith v. Jones actually say that",
  or when any skill in this suite is about to emit a citation tagged [model knowledge — verify].
argument-hint: '[citation | citation + proposition | pasted brief]'
---

# /verify-citation

Two checks, in order, and the tags they produce. This skill never says a citation is "nonexistent": the
existence check is against an offline table of 18 million reporter citations plus an index, and a real
citation can be absent from both.

## Instructions

1. **Extract every reporter citation** from what the user gave you — `488 U.S. 222`, `928 S.W.2d 483`,
   `2021 WI 45` and the like. Keep each exactly as written.

2. **Existence — no key needed.** For each citation, `POST https://docketrouter.ai/api/public/citation-check`
   with `{"citation": "<as written>"}`. One citation per call; loop. It answers in under a second and needs no
   account. Measured live 2026-09-20: `384 U.S. 436` → `{"status":"found","name":"Miranda v. Arizona"}`;
   `999 U.S. 999` → `{"status":"unverified","name":null,"note":"Not found in the library or the bulk citation
   table. Absence is not proof a citation is fabricated — but a filing should not rest on a citation no
   library can find."}`. For each result:
   - `found` → tag `[DocketRouter — found]` and carry the `name` it returned.
   - `unverified` → tag `[unverified]` and repeat the note above in your own words. Never "nonexistent".
   If the DocketRouter MCP connector is configured with a key, `check_citations` does the same for 1–100
   citations in one call and returns the same statuses; use it when you have a key, the public endpoint when
   you do not. If neither answers, tag every citation `[model knowledge — verify]` and say the check did not
   run. Do not silently downgrade.

3. **Support — needs the DocketRouter connector with a key.** For each citation the user relies on for a proposition, call `check_support` with the
   citation and the proposition (10–600 characters, the specific claim). Report exactly what comes back:
   - `supports` → quote the verbatim passage the tool returned. Do not paraphrase it.
   - `does_not_support` → say so, and that the case is real but does not stand for this.
   - `unclear` → say the opinion is in the index but the tool could not confirm the proposition from its text.
   The quote is either verbatim from the opinion or absent. There is no third state.

4. **Scope honesty.** `check_support` reads the opinion from DocketRouter's index, which is one shard set —
   its `get_instructions` tool names which jurisdictions. For a citation outside that scope the support check
   will say so; tell the user the opinion exists (if step 2 found it) but its text is not in the index, and
   point them at CourtListener to read it.

5. **Output.** A table: citation, existence result, support result, quote (verbatim or "—"), tag. Then one
   line: how many were found, how many unverified, how many supported. Anything unverified or unsupported is
   flagged `[review]` for the attorney, as everywhere in this suite.
