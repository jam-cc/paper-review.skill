# Install paper-review

[中文首页](README.md) · [English overview](README_EN.md)

The repository is the skill directory: `SKILL.md` at its root, with supporting files under `references/`. Clone it into a directory named `paper-review` to match the skill name. No package build, provider SDK, or API key is required by this repository. Your assistant may have its own subscription, model, and tool requirements.

## Choose your host

| Host | User installation | Project installation | Invoke |
|---|---|---|---|
| Codex | `~/.agents/skills/paper-review/` | `.agents/skills/paper-review/` | `$paper-review` |
| Claude Code | `~/.claude/skills/paper-review/` | `.claude/skills/paper-review/` | `/paper-review` |
| Gemini CLI | `~/.gemini/skills/paper-review/` | `.gemini/skills/paper-review/` | Ask to use the `paper-review` skill |
| Other Agent Skills hosts | Use the host's documented skill directory | Host-specific | Use its skill selector or an explicit request |
| Chat / custom API agent | Supply the instructions and references as files or context | No automatic installation | Ask to follow the supplied skill |

Paths are documented in [Codex skills](https://developers.openai.com/codex/skills/), [Claude Code skills](https://code.claude.com/docs/en/skills), and [Gemini CLI skills](https://geminicli.com/docs/cli/skills/). Gemini CLI also recognizes `.agents/skills` at user and workspace scope, so a shared Codex installation may already be discoverable there. Avoid duplicate installations unless you intend to maintain separate versions.

These are documented integration paths, not a claim of end-to-end testing on every product or version. Core packaging follows the [Agent Skills specification](https://agentskills.io/specification). `agents/openai.yaml` adds optional Codex display metadata; other hosts can use `SKILL.md` without it.

## macOS / Linux

Run only the block for your chosen host. Existing destinations are not overwritten by `git clone`.

### Codex

```bash
mkdir -p ~/.agents/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.agents/skills/paper-review
```

For one project, run from that project's root:

```bash
mkdir -p .agents/skills
git clone https://github.com/jam-cc/paper-review.skill.git .agents/skills/paper-review
```

### Claude Code

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.claude/skills/paper-review
```

For one project, use `.claude/skills/paper-review` as the destination instead.

### Gemini CLI

```bash
mkdir -p ~/.gemini/skills
git clone https://github.com/jam-cc/paper-review.skill.git ~/.gemini/skills/paper-review
```

For one project, use `.gemini/skills/paper-review` as the destination instead. Refresh with `/skills reload` and inspect `/skills list`.

## Windows / PowerShell

For Codex:

```powershell
$skillRoot = Join-Path $HOME '.agents/skills'
New-Item -ItemType Directory -Force -Path $skillRoot | Out-Null
git clone https://github.com/jam-cc/paper-review.skill.git (Join-Path $skillRoot 'paper-review')
```

For Claude Code, change `.agents/skills` to `.claude/skills`; for Gemini CLI, use `.gemini/skills`. For a project install, use the project directory instead of `$HOME`. If your CLI runs inside WSL, install into the WSL filesystem with the Linux commands.

## Check the installation

Start a new session or reload skills using your host's controls. Select `paper-review` explicitly and provide a readable paper path. For a smoke check, the bundled example can serve as source material:

```text
Use paper-review to critique the Source material section of
/path/to/paper-review/examples/synthetic-ablation.md.
Limit the review to the supplied evidence; do not search for the fictional paper.
Return the review in chat.
```

Check observable behavior: the assistant distinguishes extra data from the module effect, does not claim that it searched, and does not invent references. The example also contains an illustrative answer, so this is a loading check, not an independent evaluation of model quality.

## Claude Desktop / Cowork

Cowork uses skills enabled for your Claude account, rather than an assumed OS-specific `Claude/skills` folder. Follow the current [Cowork skill instructions](https://code.claude.com/docs/en/skills#use-skills-in-cowork-and-cloud-sessions) to enable a custom skill. If the interface requests a ZIP, create one from this checkout:

```bash
git archive --format=zip --prefix=paper-review/ --output=../paper-review.zip HEAD
```

This packages committed files only. Keep `SKILL.md` and `references/` together. Where custom skills are unavailable, use the manual workflow below.

## Other assistants and API applications

1. Supply `SKILL.md` as instructions or a readable attachment.
2. Make the referenced Markdown files accessible through attachments, retrieval, or filesystem tools. Pasting only `SKILL.md` does not supply those files.
3. Supply the paper as task data, then ask the assistant to follow the workflow. Use your SDK's actual instruction and tool interfaces; this repository ships no SDK-specific loader.

For example:

```text
Follow the attached paper-review SKILL.md to review the attached draft.
Use the supplied reference guidance where relevant. Write in English.
If you cannot access external sources, limit novelty claims to the supplied
literature and state the limitation. Return text in this chat.
```

Browsing, PDF interpretation, reference retrieval, and export remain the host's responsibility. See [runtime compatibility](references/runtime-compatibility.md) for supported fallbacks.

## Update or remove

In the installation you want to update:

```bash
git -C /path/to/paper-review pull --ff-only
```

If Git reports local changes or divergent history, inspect them before merging; do not reset local edits just to update the skill. Then refresh the host's skill list.

To uninstall, remove the exact `paper-review` directory you installed, or use your host's skill manager. Keep any reviews stored elsewhere.

## Troubleshooting

**Skill not found:** Check that the host can access the installation and that `SKILL.md` is directly inside `paper-review/`. Reload or restart, then invoke it explicitly. Plain chat interfaces need the files supplied in context; they do not discover your local clone.

**Reference files not found:** Keep the directory structure intact. Resolve `references/` relative to the installed `SKILL.md`, not the current paper directory.

**No web access:** Enable a supported search tool if you want current literature research. Otherwise the assistant should report a review limited to available sources and avoid unsupported novelty conclusions.

**No PDF or Word tool:** Supply extracted text for the review, noting any missing tables or figures. Word export is optional; a text review should still be delivered.

**Wrong format or language:** Specify the desired language and attach the venue's current review form. Those instructions take precedence over the default prose template.
