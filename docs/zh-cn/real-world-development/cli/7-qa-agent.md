---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，定义清晰的 QA 流程。

本课将介绍如何：

- 了解自定义智能体如何使用指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

创建拉取请求 (PR) 前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义项各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 使用这些能力评估需求并报告发现。它不会取代其他自定义项，也不需要另开对话。

## 创建 QA 配置

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置。配置会定义 QA 执行的检查及必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 让默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，先检查配置是否包含预期的 QA 流程和边界，避免在只需要验证功能时让 Copilot 修改实现。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 虽可选，但可让智能体显示明确名称。
3. 检查指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持性证据，在修改实现代码前征求同意，并且不会提交更改或创建拉取请求。
5. 如果配置遗漏了职责或边界，请先让默认智能体修订，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话启动时加载项目智能体。创建配置后，恢复同一个筛选对话并选择 QA，以便它使用已有的议题和规划决策。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚创建智能体，它可能不会出现在可用智能体列表中。它仍然可用，上述命令会启用它。

2. 使用以下提示词，让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体将开始工作。
4. 完成后，阅读它提供的报告。

## 总结与后续步骤

你已为工作流添加可复用的专业角色，并审查了它的工作。本课中你：

- 了解了自定义智能体如何使用指令、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 智能体，根据筛选议题审查了结果。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备好，可以审查。接下来，[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
