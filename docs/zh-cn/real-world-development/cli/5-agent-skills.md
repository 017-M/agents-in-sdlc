---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能、自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已手动验证代码，也通过指令文件确保代码符合标准。那么测试、lint 和持续集成 (CI) 的其他环节该如何处理？

对于这类任务，**智能体技能**最为合适！技能可以帮助 Copilot 了解如何正确执行这些操作。

本课将介绍如何：

- 查看现有的 `quality-checks` 技能。
- 自定义其结果格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 指令、脚本和资源

智能体技能将可复用的任务指令、可执行脚本和支持资源打包在一起，由智能体按需加载。技能的核心是一个同名文件夹，其中包含 Markdown 文件 `SKILL.md`。该文件包含说明技能名称和描述的 frontmatter、功能概述以及调用时机指南。文件夹中还可以包含供技能调用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要额外文件或文件夹。Tailspin Toys 的 `quality-checks` 技能仅包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

现在查看 Tailspin Toys 团队用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述可帮助 Copilot 判断何时调用技能。
3. 阅读指令，了解其如何引导 Copilot 执行测试和 lint。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 更改前运行技能

技能可直接通过 Copilot CLI 调用，也可以使用自然语言请求。现在让 Copilot 运行这三项检查。

1. 返回 Interactive 模式中的筛选对话。
2. 使用以下提示词：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告。

## 自定义报告

现在改进报告，让它说明运行了哪些检查、结果如何以及工具报告了什么。为技能添加报告格式要求。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改完成后，观察新格式的效果。Copilot CLI 无需重启对话即可重新加载编辑后的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前相同的提示词：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最终报告，并与第一次的报告比较。

## 总结与后续步骤

你已自定义并使用现有的智能体技能。本课中你：

- 查看了用于单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

此更改会与筛选功能一起纳入功能 PR。接下来，你将通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和配套导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
