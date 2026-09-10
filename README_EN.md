# paper-review.skill

This skill helps AI assistants review research papers, examine novelty, experiments, and references, and suggest specific revisions.

Use it with **Codex, Claude Code, and Gemini CLI**, or supply the instructions and materials to another AI assistant.

Use it to check your own paper before submission or help draft a referee report: **which claim needs more evidence, which comparison is unfair, and what deserves your attention first.**

[![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent_Skills-portable-16803c.svg)](https://agentskills.io)

[Quick start](#quick-start) · [See an example](#what-useful-feedback-looks-like) · [Install guide](INSTALL.md) · [中文](README.md)

## What useful feedback looks like

“The experiments are insufficient” leaves you guessing. This skill asks the assistant to explain which conclusion the missing evidence affects.

This **fictional, hand-written example** illustrates the intended style. It is not a measured model result:

> The paper attributes the accuracy gain to its new module, but Table 2 changes both the module and the training set size. The baseline uses 10,000 examples; the full method uses 20,000. This comparison cannot separate the architectural effect from the effect of additional data, so it does not yet support the claim that the module causes the improvement.

You now have a specific comparison to investigate. Read the [synthetic source material and analysis](examples/synthetic-ablation.md).

## When to use it

- **Before submission:** Check contribution claims, baseline fairness, and ablations to plan your next revision.
- **In a lab discussion:** Bring evidence from tables, equations, and experimental settings to the conversation.
- **For rebuttal preparation:** Separate misunderstandings from missing evidence and claims that need narrowing.
- **For assisted peer review:** Where the applicable review and confidentiality rules permit it, prepare a grounded draft for your own verification.

The examples focus on empirical ML / AI / CV / NLP research. You can request English or Chinese output. The workflow also supports conceptual arguments, though it is not a specialist proof checker.

## Quick start

Choose one set of commands for your assistant. Git is required. If the destination already exists, inspect that installation first.

### Codex

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.agents/skills/paper-review
```

Attach the paper or provide an accessible path, then ask:

```text
Use $paper-review to self-review papers/draft.pdf before submission.
Check whether the experiments support the contributions and prioritize the concerns.
```

### Claude Code

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.claude/skills/paper-review
```

```text
/paper-review Review papers/draft.pdf, focusing on baselines and ablations.
```

### Gemini CLI

```bash
mkdir -p ~/.gemini/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.gemini/skills/paper-review
```

```text
Use the paper-review skill to review papers/draft.pdf.
```

Start a new session or refresh the skill list after installation. Paths follow the hosts' official documentation. See the [install guide](INSTALL.md) for project installs, Windows, Cowork, and manual integration.

### Other assistants / APIs

Provide [SKILL.md](SKILL.md), its relevant `references/` files, and the paper as accessible files or context. Ask the assistant to follow the workflow. An assistant that reads Markdown can use the instructions; automatic discovery, browsing, and export depend on its host. **No dedicated SDK or particular model is required.**

## From paper to review

| Phase | What the assistant does | What you get |
|---|---|---|
| 1. Research the field | Find direct predecessors, competitors, and simpler baselines; record sources and comparison conditions | A concrete basis for discussing novelty |
| 2. Check contributions | Examine novelty, complexity, claim support, and controlled ablations | Concerns tied to evidence |
| 3. Write the review | Organize Summary, Strengths, Weaknesses, and Questions and Suggestions | A draft you can read and discuss |
| 4. Prune and verify | Remove redundant citations and check the sources behind critical claims | Traceable references |

The default is connected prose with at most four necessary external references and no minimum quota. Your requested format, language, and review form take precedence. You can also request a focused check of references or a single contribution.

## Outputs

When file writing is available, the default destination is `outputs/` in your working project. You can choose another directory.

```text
outputs/
├── review_<id>.txt             # Review text
├── frontier_<subfield>.md      # Related work, evidence links, verification notes
└── review_<id>.docx            # On request, when Word export is available
```

Without file tools, the assistant returns the review and source notes in chat. Without browsing or supplied external literature, it can still assess internal evidence, but must state the limits on novelty and literature coverage.

## What the workflow asks for

**Evidence behind criticism.** Numbers point to tables; conceptual concerns point to assumptions or arguments. An explanation that is missing is different from a result that is wrong.

**Fairness without a weakness quota.** Report the concerns the evidence supports, and credit careful experiments and useful contributions.

**Reference checks based on sources.** Unverified references stay out of the final bibliography. Claims that depend on them must be removed or qualified too. These checks can reduce errors; they cannot guarantee an error-free model.

**A workflow you can move between assistants.** Instructions name capabilities instead of hard-coded tool APIs. Installation guidance follows official host documentation; end-to-end testing across all hosts is still outstanding.

## Inside the repository

Start with [SKILL.md](SKILL.md). Supporting files cover [runtime adaptation](references/runtime-compatibility.md), [review prose](references/review-examples.md), [reference verification](references/hallucination-patterns.md), and [review formats](references/conference-formats.md). [Design notes](docs/DESIGN.md) explain the choices.

Contributions are welcome, especially reproducible compatibility reports and **fully synthetic** review examples. Do not submit real manuscripts, reviewing artifacts, or anonymized versions. See [examples/README.md](examples/README.md).

If this helps you catch a problem before submission, consider starring the project. Feedback about a rule that helped, or one that got in the way, is welcome too.

Inspired by the skill layout in [Anthropic Skills](https://github.com/anthropics/skills) and [colleague-skill](https://github.com/titanwings/colleague-skill). MIT License; see [LICENSE](LICENSE).
