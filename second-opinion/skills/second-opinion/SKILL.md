---
name: second-opinion
description: >
  Gets a second opinion on a legal question from a panel of frontier models WITHOUT exposing the client:
  scrambles the case file locally with legal-scrambler, has every model on the panel answer the same
  question over placeholders only, restores the answers by code, checks every citation they produced, and
  then gives Claude's own view with the panel side by side as context. Use when the user says "get a second
  opinion", "what would other models say", "sanity-check this argument", "am I missing something", or wants
  to test a position before relying on it. Spends the user's own OpenRouter key; says how many calls first.
argument-hint: '[the question, and the path to the document it is about]'
---

# /second-opinion

Read `../../CLAUDE.md` first: the three rules (nothing unscrambled leaves the machine; the panel is context,
not authority; Claude's view is separate and labelled) govern every step below.

## Instructions

1. **Scramble first, or confirm it is done.** The document must go through legal-scrambler on the user's
   machine (one matter id per case). If the `family-legal` plugin is installed, `/prepare-your-file` does
   this; otherwise:
   ```sh
   cd legal-scrambler && export LIAISON_MODEL=local/qwen3:8b SCRAMBLER_MASTER_KEY=<theirs>
   npm run -s scramble -- --matter <case-id> --stats < document.txt > document.scrambled.txt
   ```
   Never read `document.txt`. If the user pasted raw text already, say so and do not send it anywhere.

2. **Say the cost before spending it.** One call per model on the panel, plus tool calls if DocketRouter
   tools are on. State the model ids and the number of calls, and that the key is theirs. Suggested panel:
   `deepseek/deepseek-v4-flash` plus one or two others the user names. Never a GPT/OpenAI id.

3. **Run the panel** (the context is the scrambled file; the question may name the client — the tool
   scrambles it with the same graph):
   ```sh
   SCRAMBLER_FRONTIER_LIVE=1 OPENROUTER_API_KEY=<theirs> npx tsx bin/panel.ts \
     --matter <case-id> --models <a,b,c> --question "<the question>" \
     < document.scrambled.txt > panel.json
   ```
   Exit 4 means the context still contained a real name and the tool refused: go back to step 1. Exit 6
   means the frontier is disarmed. A model that fails is listed under `models[].error` and the rest of the
   panel stands.

4. **Verify every citation in `panel.json`** before quoting any answer. `citations` lists each reporter
   citation with the models that used it. The keyless endpoint takes **one citation per call** — loop:
   ```sh
   for c in "384 U.S. 436" "928 S.W.2d 483"; do
     curl -s https://docketrouter.ai/api/public/citation-check -H 'content-type: application/json' \
       -d "{\"citation\": \"$c\"}"; echo
   done
   ```
   (With a DocketRouter key, the MCP `check_citations` tool takes 1–100 in one call and returns the same
   statuses.)
   `found` → cite with `[verified: exists]` (existence only — support is a separate check); `unverified` →
   keep it visible as `[unverified]` and say which model produced it. Never drop or upgrade one.

5. **Render the panel, then Claude's view — in that order, clearly separated.**
   - *Panel* — one paragraph per model, tagged `[panel: <model id> — unverified]`, in the model's own words
     with the names restored (the tool already did that; `unknown_placeholders` lists anything it could not
     restore — leave those as written). Then a short table: where the models agree, where they split, and
     `agreement.citations_by_all` vs `citations_by_one`.
   - *Claude's view* — tagged `[Claude — with panel context]`: the reasoning with the real facts, where it
     departs from the panel and why, and which verified rule (if any store holds one) settles the point,
     tagged `[DocketRouter — verified]` with its quote.

6. **One line on what happened**, every time: the file was scrambled locally (counts by type from
   `--stats`), N models answered over placeholders, M citations checked (found / unverified), answers
   restored by code. If a step was skipped, say which.

## Never

- Never send the raw document, the graph, or the master key anywhere, and never bypass a refusal.
- Never present agreement as authority, or a panel citation as verified without the check.
- Never guess an unknown placeholder.
- Never use, or suggest, a GPT/OpenAI model.
