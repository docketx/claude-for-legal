---
name: statute-text
description: >
  The text of a statute section or court rule, word for word, from the DocketRouter connector's search_rules
  — so a rule is quoted rather than recalled. Use when the user says "what does Rule 12(b)(6) say", "quote
  Tex. Bus. & Com. Code § 2.316", "the exact text of TRCP 166a", or any time a skill would otherwise state
  a rule's text from memory.
argument-hint: '[rule or statute name, number, or topic]'
---

# /statute-text

Court rules and statutes are not case law and are not in a case-law search engine. CourtListener does not
carry them; DocketRouter holds 21,062 court rules and 1.29M statute sections verbatim, and this skill reads
them from there.

## Instructions

1. **Call `search_rules`** on the DocketRouter connector with the name, number or topic as given
   (`"Rule 12(b)(6) failure to state a claim"`, `"TRCP 166a"`, `"Tex. Bus. & Com. Code 2.316"`), `k` = 5.

2. **Quote the returned text verbatim**, in a block, with the section or rule identifier and the source the
   tool reports. Do not clean it up, do not summarise inside the block. Summary goes after the block and is
   labelled as yours.

3. **If the tool returns nothing, say the rule is not in the held corpus** and tag any text you supply from
   memory `[model knowledge — verify]`. The held corpus is 27 states plus federal for statutes; a state
   outside that list is not held, and the right answer is "not held here", never a neighbour's rule.

4. **Currency.** The held text carries a `retrieved_at` date, not an effective date. Say what date the text
   was retrieved and that amendments after it are not reflected. If the user needs current status, that is
   a `[verify against the official code]` item for the attorney.

5. **Never edit the quote to fit the question.** If a section number returned does not match the one asked
   for, say so — several states' shards carry section-number defects (see `references/known-defects.md`),
   and a plausible-looking section is not the section asked for.
