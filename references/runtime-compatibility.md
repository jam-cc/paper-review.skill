# Runtime compatibility

The skill defines a workflow. The host supplies tools and permissions. Installation does not give a model browsing, PDF vision, filesystem access, or an API key.

| Capability | Use what the host provides | If unavailable |
|---|---|---|
| Read papers | Native PDF reading, file tools, or a local PDF extractor | Work from supplied text; request missing sections only when needed. Mark figures or tables you cannot assess. |
| Read references | Filesystem access relative to the skill directory, or supplied reference contents | Ask for the relevant resource or disclose that its guidance was unavailable. Do not pretend to have loaded it. |
| Research and verify | Web search, browser, scholarly connector, or supplied primary sources | Review internal evidence; mark novelty and literature coverage as provisional. Never describe remembered citations as verified. |
| Save outputs | Workspace file tools or a sandbox filesystem | Return review and source notes in chat. |
| Export Word | A document tool or an installed library | Deliver text; explain the missing export only if requested. |

## Native skill hosts

For Codex, Claude Code, Gemini CLI, and other Agent Skills hosts, let the host discover `SKILL.md` and load references on demand. Use the actual tool names exposed by the session. Do not assume a tool named `WebSearch`, `Read`, or `Bash` exists, and do not require a provider-specific CLI to review a paper.

See [INSTALL.md](../INSTALL.md) for paths and invocation examples. Provider-specific UI metadata in `agents/openai.yaml` is optional; it does not change the portable review instructions.

## Chat and API integrations

In a chat interface, supply `SKILL.md`, the paper, and relevant reference files as accessible attachments or text. Ask the assistant to use the workflow explicitly. A link to a local file is insufficient if the host cannot read that filesystem.

In an API application, load the skill body as instructions using your SDK's supported interface and expose the reference files through retrieval/file tools or include them in context. Supply the paper separately as task data. Enable your own search and file-writing tools if needed. This repository does not ship an SDK, model client, credentials, or a search backend.

Keep the application's existing instructions and permissions. Installing or embedding the skill does not authorize uploading a private manuscript elsewhere, publishing reviews, or changing unrelated settings.
