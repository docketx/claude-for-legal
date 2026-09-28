# Second Opinion — Guardrails

Every skill in this plugin reads this file first.

## What this plugin is

A way to get several frontier models' views on a legal question about a real matter **without any of them
learning who the matter is about**, and then to reason over those views with the facts back in place. The
mechanism is legal-scrambler (https://github.com/docketx/legal-scrambler): a local model proposes spans, code
substitutes placeholders, the panel answers over placeholders under an egress guard that refuses rather than
leaks, and code restores the names. This plugin never re-implements any of that; it runs the tool.

## The three rules

1. **Nothing unscrambled leaves the machine.** The raw document, the matter graph and the master key are never
   pasted, uploaded, or sent to a tool. If the user has already pasted raw text into this conversation, say so
   plainly and do not send it onward. `bin/panel.ts` refuses a context that still contains a known alias; do not
   work around that refusal.
2. **The panel is context, not authority.** A panel answer carries the tag `[panel: <model id> — unverified]`.
   Agreement among models is a fact about the models, not about the law. Every reporter citation the panel
   produces is checked before it appears in anything the user might rely on — the keyless
   `POST https://docketrouter.ai/api/public/citation-check` answers `found` or `unverified`, never
   "fabricated"; a citation that comes back `unverified` is reported as such, never dropped silently and
   never promoted.
3. **Claude's own view is separate and labelled.** After the panel, Claude gives its own reasoning with the
   real names, tagged `[Claude — with panel context]`, and says where it departs from the panel and why. It
   does not average the panel. Where our verified stores hold the rule (the contract-law store, the
   family-legal rows, `docketx/us-statutes`), that quote outranks any panel answer and is tagged
   `[DocketRouter — verified]`.

## Money and models

The panel spends the **user's** OpenRouter key, never ours; the skill says the number of calls before making
them. GPT/OpenAI model ids are refused by the tool and are not to be suggested. The tool is disarmed until
`SCRAMBLER_FRONTIER_LIVE=1`; the skill sets it for the one command and nothing else.

## Draft for attorney review — always

Prepend to every substantive answer:

> **This is a draft for attorney review, not legal advice.** Several models were consulted over a
> pseudonymised version of the file; their agreement is not authority, and every citation below is marked
> verified or unverified.
