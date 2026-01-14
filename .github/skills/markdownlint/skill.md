---
name: markdownlint
description: Guide for using the VSCode `markdownlint` extension to check and fix Markdown issues. Use this when asked to configure or run Markdown linting without introducing CLI tools.
---

# Markdown Linting with VSCode `markdownlint`

This skill helps you configure, run and fix Markdown problems using the VSCode `markdownlint` extension only.

## When to use this skill

- You need a lightweight, editor-first Markdown linting workflow.
- You want contributors to fix formatting inside VSCode before opening PRs.
- You prefer to avoid adding CI/CLI dependencies for Markdown linting.

## Setup (Using the plugin)

1. Install the VSCode extension: `markdownlint` by David Anson.
2. Add a workspace recommendation file `.vscode/extensions.json` with the extension id to encourage collaborators to install it.

Example `.vscode/extensions.json`:

```json
{
  "recommendations": [
    "DavidAnson.vscode-markdownlint"
  ]
}
```

## Configure rules

Place a repo-wide `.markdownlint.json` at the repository root so the plugin uses consistent rules across the team.

Example `.markdownlint.json`:

```json
{
  "default": true,
  "MD013": { "line_length": 120 },
  "MD029": false,
  "MD041": false
}
```

Put editor-specific behavior in `.vscode/settings.json`:

```json
{
  "markdownlint.run": "onSave",
  "markdownlint.config": {
    "MD013": { "line_length": 120 },
    "MD029": false
  }
}
```

## Running checks & fixing

- The plugin runs in-editor; set `markdownlint.run` to `onSave` or `onType`.
- To auto-fix supported problems, open the command palette and run `Markdownlint: Fix all`.
- Document in CONTRIBUTING or PR template that contributors should run the plugin and fix all reported issues before opening PRs.

## CI / Automation (limitations and guidance)

- Limitation: VSCode extensions cannot run in standard CI runners—this skill intentionally avoids CLI tools.
- Guidance: use a PR checklist or template to require local fixes.If strict CI blocking is later required,consider adding a lightweight CLI (e.g., `markdownlint-cli` or `remark-lint`) as a separate decision and job.

## Best practices

- Keep `.markdownlint.json` in repo root for consistency.
- Recommend the extension via `.vscode/extensions.json` so contributors get automatic prompts to install it.
- Use `onSave` to reduce noisy linting during typing.
- Prefer minimal rule overrides; document any relaxed rules in CONTRIBUTING.

## Files you may add

- `.vscode/extensions.json` — recommend `DavidAnson.vscode-markdownlint`.
- `.vscode/settings.json` — enable `markdownlint.run` and optional editor settings.
- `.markdownlint.json` — repo ruleset.

## Example PR template reminder

Add to PR template or CONTRIBUTING:

"I have run `Markdownlint: Fix all` in VSCode and verified there are no remaining lint errors."

---

文件位置：[.github/skills/markdownLint/skill.md](.github/skills/markdownLint/skill.md)
