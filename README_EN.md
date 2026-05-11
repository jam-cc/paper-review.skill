# paper-review.skill

> Distill "writing a rigorous, evidence-based, non-fluff conference review" into a reusable AI Skill.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blueviolet)](https://claude.ai/code)
[![Cowork](https://img.shields.io/badge/Cowork-Ready-green)](https://claude.com)

Hand Claude an ML/AI conference paper PDF and get back a structured, evidence-grounded review — Summary, Strengths, Weaknesses, Questions, Suggestions, References. Weaknesses use the paper's own numbers. Questions are genuine clarifying questions, not directives. Every reference is web-search verified to avoid hallucinated authors and venues.

Designed for NeurIPS / ICML / ICLR / CVPR / ECCV / AAAI / ACM MM / ECML PKDD style reviewing.

[Install](#install) · [Usage](#usage) · [Design Principles](#design-principles) · [Detailed Install](INSTALL.md) · [**中文**](README.md)

---

## Why this skill exists

LLM-written reviews fall into a recurring set of failure modes:

1. **Vague critiques without evidence.** "The experiments are insufficient" — but which experiment, missing what?
2. **Novelty claims with no reference frame.** "This isn't novel" — but compared to which prior work, differing in what specific way?
3. **Hallucinated citations.** In our experience, 60 to 80 percent of LLM-generated author lists for academic references contain errors. Venue swaps are also common (e.g. confusing FlexEdit with FlexiEdit).
4. **Questions that are actually directives.** "To address Weakness 3, please run experiment X" gets jammed into the Questions section, crowding out the genuine clarifying questions a reviewer should raise.
5. **References section bloat.** Re-listing works the paper already cites (StyleID, IP-Adapter, etc.) adds noise, not signal.

This skill encodes a four-phase process (Frontier Knowledge → Innovation Analysis → Write Review → Prune & Verify References) plus a tight set of writing constraints (plain prose, no bullet points, ≤4 references, never duplicate the paper's own bibliography) to prevent these failure modes by construction.

---

## Install

### Claude Code

```bash
# Install into the current project
mkdir -p .claude/skills
git clone https://github.com/<your-name>/paper-review-skill .claude/skills/paper-review

# Or install globally (available across all projects)
git clone https://github.com/<your-name>/paper-review-skill ~/.claude/skills/paper-review
```

### Cowork (Claude Desktop)

```bash
mkdir -p ~/Library/Application\ Support/Claude/skills
git clone https://github.com/<your-name>/paper-review-skill \
  ~/Library/Application\ Support/Claude/skills/paper-review
```

### Anthropic Agent SDK / Claude API

Load the entire `paper-review-skill/` directory as a skill, or read `SKILL.md` and concatenate its contents into your system prompt.

See [INSTALL.md](INSTALL.md) for cross-platform paths and troubleshooting.

---

## Usage

Upload the paper's PDF to Claude and say:

```
Please review this NeurIPS submission
```

or

```
Help me referee this MM26 paper
```

The skill triggers automatically and runs four phases:

1. **Phase 1 — Frontier Knowledge.** Web-search the relevant subfield from the last 2-3 years and write a `frontier_<subfield>.md` document as the comparison baseline, independent of the paper's own narrative.
2. **Phase 2 — Innovation Analysis.** With the frontier map in hand, interrogate each claimed contribution: is it actually new, is it necessary, does the evidence support the claim, and are the ablations sufficient?
3. **Phase 3 — Write Review.** Produce five sections in plain prose: Summary (2-4 factual sentences), Strengths, Weaknesses (each structured as "what the paper claims → what the data shows → what this means"), Questions (genuine clarifying questions for the authors), and Suggestions (holistic recommendations for the paper as a whole).
4. **Phase 4 — Prune & Verify References.** Remove any citation that already appears in the paper's own bibliography. The remaining ≤4 references must each be web-search verified for authors, venue, year, and pages.

Output: `review_<id>.txt` + `review_<id>.docx` + `frontier_<subfield>.md`.

### Reviewing multiple papers

```
Review the two papers in the MM26 folder
```

The skill iterates the four-phase process per paper.

---

## Design Principles

### 1. A few deadly weaknesses beat a long list of surface complaints

Each weakness must satisfy one of two conditions: it threatens the paper's core contribution, or it reveals a fundamental methodological gap. Otherwise it belongs in Questions or Suggestions. Keep to 3-7 weaknesses, ideally 4.

### 2. Cite only works the paper missed and that anchor a weakness

A review is not a paper. Its References section is not a re-listing of the paper's bibliography. Before adding any citation, check whether the cited work already appears in the paper's own reference list. If it does, do not even bracket-cite it inline — refer to it by name in prose ("StyleSSP already manipulates..."). Only works the paper does not cite are eligible for the review's References section, capped at four entries (ideally three).

### 3. Questions are for the authors, not directives at them

Questions are genuine ambiguities the reviewer encountered: "What value is X set to?", "Why does this measurement predict that property?", "Is this citation a mix-up?"  
Suggestions are holistic recommendations for the paper as a whole, in priority order. Neither is a per-weakness fix list.

### 4. Hard writing constraints

Plain text only. No bold, no bullet points, no dashes or colons used for structure. Each weakness is a single continuous paragraph. Spell out "percent". Use "to" for ranges, not en-dashes.

### 5. Verify every reference via web search

Most LLM-generated reference metadata contains errors. Always cross-check authors, venue, year, and pages against WebSearch and DBLP before including a reference.

---

## Project Structure

This project follows the [AgentSkills](https://agentskills.io) standard — the entire repo is a skill directory:

```
paper-review-skill/
├── SKILL.md                      # Skill entry point (frontmatter + four-phase process)
├── README.md                     # Chinese README
├── README_EN.md                  # This file
├── INSTALL.md                    # Cross-platform install instructions
├── LICENSE                       # MIT
├── .gitignore
├── references/                   # Reference docs loaded on demand by SKILL.md
│   ├── review-examples.md        #   Calibration examples for review prose style
│   ├── hallucination-patterns.md #   Documented LLM citation-hallucination patterns
│   └── conference-formats.md     #   Per-venue review structure and scoring conventions
├── examples/                     # (empty by default) accepts only fully synthetic examples, never real review artifacts
└── docs/                         # Design documents
    └── DESIGN.md
```

---

## Scope and Limits

**Good fit for**

- Reviewing for ML / AI / CV / NLP / DM venues (NeurIPS, ICML, ICLR, CVPR, ECCV, AAAI, ACM MM, ECML PKDD, etc.)
- Journal reviewing (IEEE TPAMI, IJCV, TKDE, etc.)
- Self-review / red-teaming your own paper before submission
- Rebuttal preparation by pre-empting likely AC questions

**Currently not a great fit**

- Pure-theory math papers (lack of experimental data; frontier knowledge needs deeper domain grounding)
- Non-English papers (verification infrastructure assumes English titles)
- Very small subfields (web search hit rate for frontier knowledge is low)

---

## Contributing

PRs welcome. Especially valued directions:

- `references/conference-norms.md`: per-conference review-length, scoring rubric, and confidence conventions
- `references/subfield-priors.md`: per-subfield "high-complexity method vs simple baseline" cheat sheets to bootstrap Phase 1 faster
- `examples/`: anonymized real review samples for style calibration
- Multilingual outputs: enable Chinese-language reviews for some domestic venues

When submitting PRs, preserve the core constraints in SKILL.md (four phases, ≤4 references, plain prose). Extensions go through new `references/*.md` files rather than rewriting the SKILL.md body.

---

## Acknowledgments

Inspired by Anthropic Skills team's [skill-creator](https://github.com/anthropics/skills) template and the "the entire repo is a skill" project layout from [titanwings/colleague-skill](https://github.com/titanwings/colleague-skill). The reference-hallucination patterns are based on cases observed during real ECML PKDD 2026 reviewing.

---

MIT License © 2026
