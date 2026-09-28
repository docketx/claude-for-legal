---
name: get-my-file
description: >
  What is actually established about getting your file back from your own Texas lawyer, and about
  attorney-client privilege once the relationship is over — rendered strictly from the verified
  reference, including the honest negatives where this plugin's tooling cannot verify a rule that
  likely exists (the Texas Disciplinary Rules of Professional Conduct and the specific privilege
  exception for a dispute between lawyer and client). Use when the user asks "how do I get my file
  back", "can my lawyer keep my documents", "can my old lawyer withhold my file over unpaid fees",
  "does the privilege still apply after I fired my lawyer", or similar. Does not cover any state
  other than Texas.
argument-hint: '[a question about your Texas client file or privilege with your former lawyer, plain text]'
---

# /get-my-file

Answers what is verified about the Texas client file and post-relationship privilege from
`../../references/rows-tx-sue-my-lawyer.json`, and states plainly where this plugin's tooling
cannot verify a rule. Read `../../CLAUDE.md` before running this skill.

## Instructions

1. **Jurisdiction check.** Texas only; otherwise follow the jurisdiction gate in `../../CLAUDE.md`
   and stop.

2. **Named-lawyer check.** If the user names a real lawyer or firm, follow "What this plugin is
   never for" in `../../CLAUDE.md`.

3. **Load the two rows this skill draws on** from `../../references/rows-tx-sue-my-lawyer.json`:
   `right-to-the-file` and `attorney-client-privilege-after-relationship-ends`. Both rows share a
   theme worth stating up front: this plugin found and can verify real, on-point text for both
   questions, but neither one is verified the "normal" way (a direct statute or rules-corpus
   citation) — read each row's `notes` before answering, because the notes explain exactly what
   kind of verification each row does and doesn't have, and that distinction matters to how
   confidently you should state the answer.

4. **Sort the question before answering:**
   - **"Can I get my file back? Can my lawyer keep a copy?"** → `right-to-the-file`. Render the
     two quotes from *In re Cokinos, Boisien & Young* — a Texas court of appeals opinion quoting
     TDRPC Rule 1.15(d) directly — and be precise about what kind of verification this is: it is a
     **verified case-law quote of the rule's own words**, not a verified citation to the
     Disciplinary Rules themselves (this plugin's corpus does not hold the Disciplinary Rules or
     Rules of Disciplinary Procedure as an independently verifiable dataset for Texas at all — see
     the row's `notes`). State the conditional nature of the retention right exactly as quoted:
     the lawyer may retain a COPY, and only "to the extent permitted by other law" and "only if
     such retention will not prejudice the client."
   - **"Does the privilege survive? Can it be used against my lawyer in a fee dispute or
     malpractice case?"** → `attorney-client-privilege-after-relationship-ends`. Render the
     National Tank Co. v. Brotherton quote on the privilege's general scope, tagged
     `[DocketRouter — verified]`. Then give the honest negative in full, not as a footnote: the
     specific exception that lets privileged communications be used in a dispute BETWEEN the
     lawyer and the client (Tex. R. Evid. 503(d)(3), a "breach of duty" exception) was read
     directly from the published corpus file but is not served as a verified authority in this
     row, because `the DocketX corpus verifier` does not support `docketx/court-rules`
     as a dataset. Do not state the exception's substance as `[DocketRouter — verified]` — if you
     describe what Rule 503(d)(3) says, tag it `[unverified]` or `[model knowledge — verify]` as
     appropriate and say plainly that this plugin's verification tooling has a gap here, per the
     row's `notes`.

5. **Do not fill either tooling gap with confident-sounding prose.** It is tempting to state "the
   Disciplinary Rules require X" or "the privilege exception covers Y" as if from a verified
   source, since the underlying rule text is genuinely known and was genuinely read for this
   plugin — don't. The entire value of this skill is in not quietly upgrading a read-but-unverified
   rule to `[DocketRouter — verified]`. Say what was read, say why it isn't tagged that way, and
   point the user to their own attorney for the exact current rule text if precision matters to
   their situation.

6. **Close with the draft-for-attorney-review line** from `../../CLAUDE.md`.

## Output

A direct answer built from the two rows, each quote tagged appropriately (`[DocketRouter —
verified]` for the case-law quotes that did pass the verifier; a clearly flagged tooling-gap
explanation, not a false `[DocketRouter — verified]` tag, for the Rule 503(d)(3) exception), and
the draft-for-attorney-review line.
