---
name: prepare-your-file
description: >
  Run this BEFORE any other skill in this plugin reads a real case document. Pseudonymises a family-law
  file (petition, decree, financial declaration, messages, medical or school records) on the user's own
  machine with the open-source legal-scrambler, so that names, children's names, SSNs, account numbers,
  addresses, phones, emails and dates of birth never leave the machine — and puts them back into the
  answer afterwards. Use when the user pastes or points at a document that is about real people, or
  asks "is it safe to give you my divorce papers", "can you read my decree", "redact this before we
  start". Does not analyse the document itself; it prepares it for the other skills.
argument-hint: '[path to the document, or "paste" to scramble text the user is about to paste]'
---

# /prepare-your-file

A divorce file is the most personally identifying document most people will ever own: the parties,
the children, Social Security numbers, bank and retirement accounts, home addresses, employers,
medical and school records. This skill makes sure none of that is sent anywhere. Read
`../../CLAUDE.md` first — the family-violence caveat applies here too: if the document is about
a protective order or safety planning, stop and follow that caveat before scrambling anything.

The tool is **legal-scrambler** (Apache-2.0, https://github.com/docketx/legal-scrambler). Its rule:
a local model proposes *spans*, code does every substitution in both directions, and the answer is
restored by code from a sealed ledger. Its proof table (0 of 1,984 entities leaked on 40 real court
dockets; byte-exact restore) is in that repository, not asserted here.

## Instructions

1. **Never read the raw document into the conversation.** If the user has pasted it already, tell
   them plainly that the pasted text has been seen by the model in this session, and that the next
   document should go through this skill first. Do not summarise or quote what was pasted.

2. **Check the tool is installed** (one time, on the user's machine, in a directory of their choosing):
   ```sh
   git clone https://github.com/docketx/legal-scrambler && cd legal-scrambler && npm install
   ollama pull qwen3:8b        # the local model; any Ollama model works, nothing non-local is accepted
   export LIAISON_MODEL=local/qwen3:8b
   export SCRAMBLER_MASTER_KEY=$(node -e 'console.log(require("node:crypto").randomBytes(32).toString("base64"))')
   ```
   Tell the user to keep `SCRAMBLER_MASTER_KEY` somewhere safe: it is the only thing that can turn
   placeholders back into names, and it never leaves their machine. Without Ollama, `--regex-only`
   still removes SSNs, accounts, phones, emails and addresses by shape — but **not names**; the
   tool says so on stderr and so must you.

3. **Scramble, with one matter id per case** so the same person is the same placeholder in every
   document of the case:
   ```sh
   npm run -s scramble -- --matter <case-id> --stats < document.txt > document.scrambled.txt
   ```
   Read `document.scrambled.txt` — never `document.txt`. The `--stats` line on stderr gives counts
   by type and any reasons for refusal; it never contains a value. Exit code 4 means the tool
   refused to release the document rather than release it partially: report the reason, do not
   work around it.

4. **Work on the scrambled text** with the other skills in this plugin. Placeholders look like
   `[PERSON_1]`, `[CHILD_2]`, `[SSN_1]`, `[ADDRESS_3]`. Refer to people by their placeholders in
   everything you write. Reporter citations and statute references are left intact by design
   (public law is public) — quote them normally.

5. **Restore the answer** for the user, on their machine:
   ```sh
   npm run -s unscramble -- --matter <case-id> < answer.txt > answer.restored.txt
   ```
   If the tool exits 5 it has listed placeholders the matter does not know; those are left as-is
   in the output and must not be guessed. Say which ones and why that is the correct behaviour.

6. **Say what happened**, in one line, every time: the file was scrambled locally, N items of which
   types were replaced (from `--stats`), the model saw placeholders only, and the answer was restored
   by code from the sealed store. If any step was skipped — the user pasted raw text, or ran
   `--regex-only` — say that instead, in the same line.

## What this skill never does

- It never sends the raw document, or the matter graph, or the master key, anywhere.
- It never uses an upstream model to find names; the scrambler refuses any model id that is not
  `local/…`, and so does this skill.
- It never guesses what an unknown placeholder stands for.
- It is not a redaction for court filing. A court's own redaction rules (e.g. Tex. R. Civ. P. 21c)
  are a separate, verified question for `/what-the-statute-says`; this skill only protects the
  user's data from the model.
