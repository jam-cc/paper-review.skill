# Changelog

## v0.3.0

- **Removed all real-content examples from the `examples/` directory.** Previous releases incorrectly bundled frontier-knowledge documents that were generated during real reviewing batches. Even though those documents are paper-independent in content, the choice of subfield to map and competitors to flag is the reviewer's intellectual work and is tied to specific submissions under double-blind review. The `examples/` directory now ships empty with a README that explains it accepts only fully synthetic content.

## v0.2.0

- Added `references/conference-formats.md` documenting per-venue review structures and scoring conventions for CVPR/ECCV/ACM MM, NeurIPS/ICML/ICLR, ECML PKDD/ECAI, journals (TPAMI/AIJ/IJCV/TKDE/CSUR), and workshops. SKILL.md now reads this in Phase 3 to pick the right output template.
- Expanded the `examples/` gallery to four entries spanning ACM MM 2026 (diffusion style transfer), ECCV 2026 (industrial video anomaly detection), CVPR 2026 (training-free reference-consistent generation), and ECML PKDD 2026 (VLM-based zero/few-shot anomaly detection). Each example ships only the paper-independent frontier-knowledge document to respect review confidentiality.

## v0.1.0 (initial open-source release)

- Four-phase reviewing process: Frontier Knowledge → Innovation Analysis → Write Review → Prune & Verify References
- Hard cap of 4 references in the review's References section, with the rule that references already present in the paper's bibliography must not be re-cited (neither inline nor in the References section)
- Strict prose-only writing constraints: no bullet points, no markdown, continuous-paragraph weaknesses
- Questions are reserved for genuine clarifying questions to the authors; Suggestions are reserved for holistic recommendations on the paper as a whole; neither is a per-weakness fix list
- Reference files in `references/`:
  - `review-examples.md` — calibration prose for the target review style
  - `hallucination-patterns.md` — documented LLM citation-hallucination cases (based on ECML PKDD 2026 reviewing audit)
- Project layout follows the AgentSkills convention; the entire repo is the skill directory and can be cloned directly into Claude Code / Cowork / OpenClaw skill paths
