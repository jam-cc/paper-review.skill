# Design Notes

This document explains why the paper-review skill is structured the way it is. Read this if you are forking the skill, contributing PRs, or just trying to understand what each constraint is doing and why.

## The four-phase process

The single most important design decision is the four-phase split: Frontier Knowledge → Innovation Analysis → Write Review → Prune & Verify. Each phase exists to prevent a specific failure mode that LLM-written reviews fall into.

**Phase 1 (Frontier Knowledge)** exists because the most common LLM reviewing failure is evaluating a paper purely against its own narrative. The paper says it is novel, the LLM looks at the contributions, and concludes it is novel — without ever asking "what already exists in this subfield?" Forcing an upfront, paper-independent web search produces a comparison map that the rest of the review can be grounded in. The output is saved as `frontier_<subfield>.md` so future passes (or other reviewers) can reuse it.

**Phase 2 (Innovation Analysis)** is where the frontier map collides with the paper. The four questions ("is this new, is this necessary, does the evidence support the claim, are the ablations sufficient") are designed to map directly onto the four most common author overclaims: claiming novelty without disambiguating from prior work, claiming necessity without simpler-baseline comparison, citing winning metrics while burying losing ones, and confounding architecture changes with training-procedure changes.

**Phase 3 (Write Review)** is the actual writing step. The unusual constraints (plain prose only, no bullet points, no markdown) are deliberate: bullet-point reviews encourage shallow, listy critique. Continuous-paragraph weaknesses force the reviewer to actually develop the argument from claim → evidence → implication. This is also why each weakness must satisfy the "threatens the contribution OR reveals a methodological gap" test — listy reviews tend to drift toward minor stylistic complaints.

**Phase 4 (Prune & Verify References)** exists because of two well-documented LLM problems: hallucinated reference metadata, and noisy reference sections that recapitulate the paper's own bibliography. The Prune step removes the second problem; the Verify step removes the first. Both must happen at the end, not mid-write, because earlier verification interrupts the writing flow and produces choppy text.

## Why "≤4 references" and "skip references already in the paper"

Reviews are not papers. Their References section serves a different purpose: it anchors critical claims in prior work the authors should have engaged with but did not. If a work is already in the paper's bibliography, listing it again in the review's References section adds noise — the author and area chair already have the full citation in front of them. The same logic applies to inline bracket citations: a bracketed `[5]` that points to an entry the paper already cites just makes the reader cross-reference twice.

The 4-reference cap is empirically calibrated. Reviews with more than 4 external references almost always include weakly relevant ones that exist mostly to "sound thorough." Capping forces selectivity: each surviving reference must be the strongest possible piece of external evidence for one specific weakness.

## Why Questions and Suggestions are not weakness fix lists

Authors and area chairs read Questions looking for "what does the reviewer not understand?" If the Questions section is a thinly-disguised "to fix Weakness 3, do X," then the reviewer has wasted the only place in the review where the authors can clarify their work. Real Questions look like "the paper says X but does not state how X is set; could the authors clarify?" or "the bibliography entry [24] cites paper A but the in-text use invokes a finding that belongs to paper B; is this intentional?"

Suggestions are similarly about the paper as a whole, in priority order. "Release the evaluation lists and re-evaluate baselines" is a holistic suggestion. "To address Weakness 3, please run experiment Y" is a directive disguised as a suggestion.

This separation also disciplines the Weaknesses section: if a reviewer is tempted to write a long weakness with a long fix recipe, that signals either (a) the weakness should be split into a clear weakness + a clear suggestion, or (b) the weakness is actually a question.

## Why the writing-style constraints are so tight

LLMs love bullet points, headers, and bold text. These are appropriate for product specs and bad for review prose. A review's job is to argue, not to enumerate. Continuous prose forces the model to write transitions between ideas; bullet points let it list disjointed observations.

The numerical conventions ("percent" instead of `%`, "to" for ranges instead of en-dashes) are about consistency with the typical formal-prose register that conference review interfaces expect. They are also chosen to render correctly in plain-text submission boxes, which often strip Unicode dashes.

## Why both `.txt` and `.docx` outputs

Review submission systems vary. OpenReview accepts plain text. Some venues (especially journals) want a Word doc. Generating both up front saves a round-trip when the user doesn't yet know which they need.

## The references/ subdirectory

Two files live there:

- `review-examples.md`: short worked examples showing the target prose style. These are deliberately read by the model during Phase 3, not summarized into SKILL.md, because style is best taught by example, not rule.
- `hallucination-patterns.md`: documented cases of LLM citation errors. Read during Phase 4 to set the verification bar appropriately. The 60-80 percent author-list error rate is not a guess; it comes from a real audit of LLM-generated reviews on ECML PKDD 2026.

These files are intentionally separate from SKILL.md so that growing them does not bloat the always-loaded skill body. SKILL.md should stay under ~500 lines; the references/ files can be arbitrarily long.

## Tradeoffs the current design accepts

**Tighter style constraints reduce expressivity.** Some reviewers prefer bulleted weaknesses for skimability. The current design forces continuous prose and accepts that loss in exchange for argument quality.

**Phase 1 is the slowest part.** Frontier-knowledge research can take 10-20 web searches and tens of thousands of tokens. For a quick first-pass review of an obviously-flawed paper, this is overkill. A future `--quick` mode might skip Phase 1 entirely.

**Reference verification is conservative.** When the model can't verify a citation, the SKILL.md says "either find an alternative or remove the reference." This sometimes loses a genuinely-correct citation that just isn't easily web-searchable. The asymmetry is intentional: a missing citation is much less harmful than a hallucinated one.

**The skill assumes the reviewer is acting in good faith.** It does not, and cannot, prevent a reviewer from using it to write hostile reviews on papers they have a conflict with. Use this skill responsibly.
