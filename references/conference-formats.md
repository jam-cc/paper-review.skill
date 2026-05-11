# Conference and Journal Output Formats

Different venues expect different review structures, scoring rubrics, and confidence reporting. This file documents the formats encountered most often. Read it before Phase 3 to choose the right output template for the venue the user mentions.

If the user does not specify a venue, default to the **CVPR / ECCV / ACM MM** family (prose Strengths + Weaknesses + Questions + Suggestions, no scoring fields), since this is the most common. If the user mentions a specific venue, switch to the matching format below.

## Family 1: CVPR / ECCV / ACM MM / AAAI

The default prose-only format the SKILL.md specifies. No scoring fields in the body of the review (those are filled in separately on the OpenReview / CMT form). Sections:

```
Review of Submission [number]

Title. [paper title]

1. Summary
2. Strengths
3. Weaknesses
4. Questions and Suggestions
5. References
```

Length: typically 800 to 2,000 words. AAAI reviews tend to be slightly shorter than CVPR.

## Family 2: NeurIPS / ICML / ICLR

These venues require explicit per-axis scoring at the end. Append the following scoring block after Section 5:

```
Soundness. [1 to 4] [one-sentence justification keyed to specific weaknesses]
Presentation. [1 to 4] [one-sentence justification]
Contribution. [1 to 4] [one-sentence justification]

Overall recommendation. [1 to 10 for NeurIPS/ICML, 1 to 10 for ICLR with named bands]
Confidence. [1 to 5]
```

Score rubrics (NeurIPS/ICLR convention):

- Soundness 4: excellent; 3: good; 2: fair (paper has technical weaknesses); 1: poor
- Presentation 4: excellent; 3: good; 2: fair; 1: poor
- Contribution 4: excellent; 3: good; 2: fair; 1: poor
- Overall: 10 strong accept, 8 accept, 6 weak accept, 5 borderline, 4 borderline reject, 3 reject, 1 strong reject
- Confidence: 5 absolutely certain, 4 confident with full familiarity, 3 fairly confident with possible unfamiliarity, 2 willing to defend but might be wrong, 1 educated guess

Each axis justification should reference specific weaknesses by number ("Soundness 2 because Weakness 1 and Weakness 4 reflect a methodological gap and a baseline reproduction issue"). Avoid generic justifications.

## Family 3: ECML PKDD / ECAI

Prose-only like Family 1 but typically expects shorter reviews (600 to 1,200 words). The author-response cycle is usually shorter, so Questions should be especially crisp and actionable. No mandatory scoring block in the review body.

## Family 4: Journals (IEEE TPAMI, AIJ, IJCV, TKDE, CSUR, T-MM, ESWA)

Journal reviews are longer (often 1,500 to 3,500 words for first review) and use a different weakness structure. Common conventions:

- **Major weakness / Minor weakness split.** Write as two separate numbered lists in the Weaknesses section. A "minor weakness" is a fixable presentation or scoping issue; a "major weakness" is something that requires substantive revision.
- **Per-section critique.** Long-form journal reviews often walk through Sections 2 (Related Work), 3 (Method), 4 (Experiments), 5 (Discussion) and comment on each. The Weaknesses still go through the four-question audit, but the Suggestions section is where the per-section comments naturally live.
- **Recommendation language.** Use the journal's specific recommendation vocabulary: "Major Revision," "Minor Revision," "Reject and Resubmit," "Accept with Minor Revisions," "Reject." Do not use conference scoring.

Append at the end:

```
Recommendation. [Major Revision / Minor Revision / Reject and Resubmit / Accept / Reject]
Confidence. [1 to 5]
```

## Family 5: Workshop reviews (NeurIPS / CVPR / ICML workshops)

Shorter, more lenient. Workshop reviewers should focus on the paper's potential as a discussion piece rather than its readiness for publication. Use Family 1's structure but cap the review at ~600 words and bias the recommendation toward weak accept unless the paper has a fundamental flaw.

## Common adaptations regardless of venue

**Anonymity.** Never reveal the reviewer's identity through specific phrasing ("as I showed in my paper..."), and do not name the reviewer's institution.

**Public-figure constraint.** Do not write content that names a specific author of the submission, even if you can guess from writing style or topic; review the paper, not the people.

**Scoring justification cross-references.** Whenever a venue requires scoring, the justifications must explicitly cross-reference the weakness numbers in Section 3. This is what makes the scoring auditable.

**Confidence calibration.** Confidence is about the reviewer's familiarity, not the paper's quality. A confident reviewer who knows the field well can give a paper a 6/10 with high confidence. Do not deflate confidence to soften a low score; that confuses the area chair.

**Length.** When in doubt, longer is better than shorter, but every additional sentence must do work — either deepening an existing weakness with more evidence or raising a new genuine concern. Padding hurts the review's credibility.
