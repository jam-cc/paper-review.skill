# Install Guide

This skill is a single directory. Drop it into the right path on your platform and it will be picked up automatically.

## Quick reference

| Platform | Skill directory |
|---|---|
| Claude Code (project-local) | `<repo-root>/.claude/skills/paper-review/` |
| Claude Code (user-global) | `~/.claude/skills/paper-review/` |
| Cowork / Claude Desktop (macOS) | `~/Library/Application Support/Claude/skills/paper-review/` |
| Cowork / Claude Desktop (Windows) | `%APPDATA%\Claude\skills\paper-review\` |
| Cowork / Claude Desktop (Linux) | `~/.config/Claude/skills/paper-review/` |
| OpenClaw | `~/.openclaw/workspace/skills/paper-review/` |
| Anthropic Agent SDK | Pass the directory path to the SDK's skill loader, or read `SKILL.md` into the system prompt |

The skill folder must be named `paper-review` (matching the `name` field in `SKILL.md` frontmatter). Renaming the folder breaks the slash-command and triggering behavior.

---

## Claude Code

### Option A: install into the current project

```bash
cd /path/to/your/project
mkdir -p .claude/skills
git clone https://github.com/<your-name>/paper-review-skill .claude/skills/paper-review
```

The skill is now available only when Claude Code is invoked inside this project. This is the recommended option if you only review papers in one workspace, or if you want different review styles per project (for example a strict mode for top venues and a lighter mode for workshops).

### Option B: install globally for all projects

```bash
mkdir -p ~/.claude/skills
git clone https://github.com/<your-name>/paper-review-skill ~/.claude/skills/paper-review
```

The skill is then loaded in every Claude Code session.

### Verify

In Claude Code, type `/help` and confirm `paper-review` is in the available-skills list. Or just upload a paper PDF and say "review this," and the skill should trigger automatically.

---

## Cowork (Claude Desktop)

### macOS

```bash
mkdir -p ~/Library/Application\ Support/Claude/skills
git clone https://github.com/<your-name>/paper-review-skill \
  ~/Library/Application\ Support/Claude/skills/paper-review
```

### Windows (PowerShell)

```powershell
$dest = "$env:APPDATA\Claude\skills\paper-review"
New-Item -ItemType Directory -Force -Path "$env:APPDATA\Claude\skills" | Out-Null
git clone https://github.com/<your-name>/paper-review-skill $dest
```

### Linux

```bash
mkdir -p ~/.config/Claude/skills
git clone https://github.com/<your-name>/paper-review-skill ~/.config/Claude/skills/paper-review
```

After cloning, restart Claude Desktop so the skill list refreshes.

---

## OpenClaw

```bash
mkdir -p ~/.openclaw/workspace/skills
git clone https://github.com/<your-name>/paper-review-skill ~/.openclaw/workspace/skills/paper-review
```

---

## Anthropic Agent SDK

If you build your own agent on top of the Claude API + Agent SDK, two integration patterns work:

### Pattern 1: SDK-managed skills

```python
from anthropic_agent import Agent

agent = Agent(
    model="claude-sonnet-4-6",
    skill_dirs=["/path/to/paper-review-skill"],
)
```

### Pattern 2: inline system prompt

If your stack does not yet support skill auto-loading, read `SKILL.md` and prepend it to your system prompt:

```python
import pathlib

skill_md = pathlib.Path("paper-review-skill/SKILL.md").read_text()
system_prompt = f"""{your_existing_system_prompt}

# Embedded skill: paper-review

{skill_md}
"""
```

`SKILL.md` references files under `references/` by relative path; if you go the inline route, also expose those files via the agent's filesystem so the skill body can `Read` them when it instructs to.

---

## Updating

```bash
cd /path/to/installed/paper-review
git pull
```

Cowork users may need to restart Claude Desktop after pulling.

---

## Uninstalling

Just delete the skill directory:

```bash
rm -rf <wherever-you-installed>/paper-review
```

---

## Common issues

**Skill does not trigger when I upload a PDF.**  
Check that the directory is named exactly `paper-review` and that `SKILL.md` is at its root (not nested one level deeper). The triggering key is the `description` field in the SKILL.md frontmatter; if you renamed or modified it, mention "review this paper" or "write a referee report" explicitly to force the trigger.

**Web search returns no results during Phase 1 / Phase 4.**  
Frontier-knowledge research and reference verification both depend on web access. In Claude Code, ensure WebSearch is enabled in settings. In Cowork, the Network category in capability settings must allow general web search. If your network blocks search APIs, the skill will fall back to less-verified output, which is a quality loss.

**Output review's References section keeps re-listing works the paper already cites.**  
This is a model adherence issue. The Phase 4 Prune step in `SKILL.md` explicitly forbids it. If it still happens, prepend a short reminder to your prompt: "Apply the Prune step strictly: do not list works that already appear in the paper's bibliography."

**Reviews end up with bullet points or markdown headers.**  
SKILL.md hard-bans these in the writing-style section. If they appear, the model is overriding skill instructions; restate "plain prose only, no bullets, no headers" in the user message.

---

## Optional: reproduce the example outputs

The `examples/` directory contains a fully worked example (frontier doc + two reviews) for an ACM MM 2026 reviewing batch on diffusion-based style transfer. To regenerate them:

```bash
# Place the two PDFs in a folder, then ask Claude:
# "Review the two papers in this folder using the paper-review skill"
```

The expected output structure is:

```
outputs/
├── frontier_diffusion_style_transfer.md
├── review_1918.txt
├── review_1918.docx
├── review_4910.txt
└── review_4910.docx
```

---

## License

MIT — see [LICENSE](LICENSE).
