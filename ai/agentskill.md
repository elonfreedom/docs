# VS Code Agent Skill

本文件基于官方文档整理，面向仓库维护者与普通开发者，说明如何创建、组织与使用 Agent Skills（SKILL.md）。

## 一句话概览

Agent Skills 是可移植的技能包（指令、脚本、示例、资源），可按需由 Copilot 在 VS Code、Copilot CLI 和其他兼容 agent 中加载。

## 何时使用 Agent Skills

- 构建领域化的、可复用能力（例如：测试、调试、部署流程）。
- 需要随请求按需加载说明与资源，避免重复上下文。
- 希望技能在多个 agent 之间移植（遵循 agentskills.io 规范）。

## 快速创建步骤（最小可用技能）

1. 在仓库中创建目录：`.github/skills/<skill-name>/`。
2. 在该目录下创建 `SKILL.md`，并在顶部添加 YAML frontmatter：

```yaml
---
name: skill-name
description: 简短说明技能能做什么以及何时使用
---
```

3. 在 `SKILL.md` 正文中写清楚：目的、适用情景、逐步流程、输入输出示例，以及对目录中脚本/资源的引用（相对路径）。
4. 可选：在目录添加脚本、模版或示例（例如 `test-template.js`、`examples/`）。
5. 将目录提交到仓库，Copilot 将根据 frontmatter 自动发现并按需加载。

## `SKILL.md` 内容规范（要点）

- Header（必需）：YAML frontmatter，至少包含 `name`（小写、连字符，<=64 字符）和 `description`（<=1024 字符）。
- Body（必需）：清晰、具体的指令，包含：
  - 技能用途（What）
  - 何时使用（When）
  - 逐步流程（How）
  - 输入/输出示例（Examples）
  - 资源引用（相对路径链接）

示例最小模板：

```markdown
---
name: webapp-testing
description: 运行与分析 web 应用测试用例的技能
---

# Instructions

步骤、示例与引用脚本...
```

## Copilot 如何加载技能（逐级加载）

- Level 1（发现）：读取 frontmatter (`name` 与 `description`) 用于快速匹配请求。
- Level 2（加载指令）：当请求与描述匹配时加载 `SKILL.md` 正文到上下文。
- Level 3（资源访问）：仅在需要时读取技能目录下的额外资源（脚本、示例），以节省上下文量。

因此无需手动激活：只要技能存在且描述匹配，Copilot 会按需使用。

## 示例技能：GitHub Actions 调试（摘要示例）

此示例展示 `SKILL.md` 中可以包含的步骤（非完整文件）：

1. 使用工具列出 pull request 的最近 workflow runs（`list_workflow_runs`）。
2. 对失败作业使用 `summarize_job_log_failures` 获取 AI 汇总。
3. 若需更多细节，使用 `get_job_logs` 或 `get_workflow_run_logs` 获取完整日志。
4. 在本地尝试复现失败。
5. 修复失败并验证，然后提交补丁。

这类技能会在正文中给出逐步操作示例、命令或脚本引用、以及常见问题和解决建议。

## 使用与共享技能

- 从社区拷贝技能：可从 `github/awesome-copilot` 或 `anthropics/skills` 等仓库获取并复制到 `.github/skills/`，在使用前务必审查安全性与依赖。
- 个人技能可放在 `~/.copilot/skills/` 供本地使用。

## 安全与自动化执行控制

- 如果技能包含可执行脚本或要求在终端运行命令，VS Code 提供终端工具和自动批准选项（allow-lists、auto-approve）。
- 强烈建议：不要在技能目录中提交敏感凭据；在文档中明确说明权限范围与数据访问边界。

## 常见问题与注意事项

- 描述应具体：`description` 字段应清楚限定何时启用该技能。
- 小心自动运行脚本：向用户说明如何复审与安全批准脚本执行。
- 指明先决条件：所需扩展、Node/运行时版本或外部服务凭据。

## 参考与资源

- Agent Skills 规范：<https://agentskills.io/>
- 官方文档：<https://code.visualstudio.com/docs/copilot/customization/agent-skills>
- 社区技能仓库示例：<https://github.com/anthropics/skills>
