# Codex Skills

Small, opinionated skills for OpenAI Codex. The repository favors concise guidance, adaptive workflows, and verifiable outcomes over rigid orchestration frameworks.

## Available skills

### `codex-lead-development`

Lead a coding task from an isolated feature branch through planning, adaptive subagent delegation, implementation, independent review, verification, and pull-request delivery.

Highlights:

- scales from direct implementation to a small multi-agent team;
- keeps the main Codex thread focused on decisions and integration;
- uses `gpt-6-sol` for subagents by default, including implementation and review;
- reserves `gpt-6-astra` for individual assignments that genuinely need stronger reasoning;
- uses one reviewer for small changes and up to three scoped reviewers for complex changes;
- iterates on material findings and actionable CI feedback without merging automatically.

## Install

Clone the repository and copy the skill into your personal Codex skills directory:

```bash
git clone https://github.com/oda02/codex-skills.git
mkdir -p ~/.codex/skills
cp -R codex-skills/skills/codex-lead-development ~/.codex/skills/
```

Start a new Codex task so the installed skill is discovered.

## Use

```text
Use $codex-lead-development to implement this feature and deliver a reviewed PR.
```

The skill intentionally leaves implementation details to Codex. It defines ownership, delegation boundaries, review quality, evidence, and delivery expectations without imposing a fixed role sequence on every task.

## Influences

The workflow was informed by the official [Codex subagents documentation](https://learn.chatgpt.com/docs/agent-configuration/subagents) and ideas from:

- [obra/superpowers](https://github.com/obra/superpowers) — focused briefs, independent review, scoped re-review, and final whole-branch verification;
- [RavenClaude `spawn-team`](https://github.com/mcorbett51090/RavenClaude/tree/main/plugins/ravenclaude-core/skills/spawn-team) — deciding whether delegation is worthwhile and keeping orchestration flat;
- [sontek/sontek-skills](https://github.com/sontek/sontek-skills) — atomic workflows and fresh-context finding verification;
- [getsentry/skills](https://github.com/getsentry/skills) — actionable feedback and bounded PR/CI iteration.

## License

MIT
