# Design notes

## One workflow across hosts

The repository packages instructions and reference material, not a model client. `SKILL.md` uses the portable Agent Skills frontmatter and describes capabilities rather than provider tool names. Host-specific installation belongs in `INSTALL.md`; fallback behavior belongs in `references/runtime-compatibility.md`. Optional `agents/openai.yaml` metadata improves Codex discovery without changing the workflow for other hosts.

This separation keeps the review process usable in a native skill runner, an ordinary chat with attachments, or an API agent. It does not imply that those environments have equal tool access or produce equal-quality results.

## Why four phases

Frontier research supplies a basis for judging novelty beyond the paper's own narrative. Contribution analysis tests the relationship between claims and evidence. Drafting turns those findings into coherent arguments. Final pruning and verification checks the references and the claims they support.

The phases describe a full review. A focused reference audit or a critique of one experiment should use only the relevant parts. The user's scope takes precedence over workflow completeness.

## Evidence without quotas

A weakness should explain a material concern using a traceable observation. Numerical evidence is useful for empirical claims, while a theoretical concern may depend on an assumption or derivation. Neither a target weakness count nor a reference budget should cause the assistant to invent issues.

The default four-reference cap is an editorial choice, not a benchmark result. It encourages selecting necessary external evidence and avoiding repetition of the paper's bibliography. User or venue requirements can override it. Verification must test both the identity of a reference and whether it supports the argument.

## Prose and venue forms

Connected paragraphs encourage the assistant to explain claim, evidence, and implication. Numbered concerns help authors refer to them. The style is a default: a supplied form, language choice, or formatting request takes precedence.

Conference formats are templates rather than fixed score tables. The assistant must use a supplied or verified current rubric for numeric recommendations. It should not judge a workshop paper more favorably by default or hide uncertainty in an invented confidence score.

## Outputs and missing tools

Text is the primary deliverable. A frontier document keeps sources and verification notes auditable without bloating the review. Word is optional because not every host can generate it. In chat-only environments, the assistant can deliver both prose and compact notes directly.

Missing web access limits claims about current literature, but does not prevent inspecting the paper's internal evidence. The assistant must disclose that limitation, avoid claiming searches it did not perform, and remove unsupported novelty accusations along with their unverified citations.

## Validation boundary

Package validation can check frontmatter, resource paths, metadata, and installation examples. It cannot prove that a model will follow the workflow or that every host discovers the skill. Compatibility claims distinguish documented installation support from actual end-to-end testing. The bundled synthetic example illustrates the intended review style; it is not a performance evaluation.
