---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

很少有组织完全没有技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务，并在完成后创建草稿 PR，供开发者审查。这样可以将繁琐任务交给智能体处理，加快开发流程，让开发者专注于更宏观的工作。

本练习将介绍如何：

- 自定义代码生成环境。
- 确保操作安全。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初开发网站的承包商没有提供代码文档。团队希望首先为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示同一类别中的相关游戏，让用户可以继续浏览。他们暂时不需要完善的实现，只需要可用于验收测试用户体验的版本。目前这项工作被其他更高优先级的议题阻塞。

这两项任务都可能很快被降级优先级，适合交给 Copilot 云智能体异步处理。开发者可以继续处理其他工作，之后再回来审查结果。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式启动任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看详情。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，使用它并不意味着可以跳过思考。即使看似简单的操作，也可能隐藏复杂性，因此请注意[合理界定云智能体任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程助手：分阶段工作、学习、试验并逐步调整。生成式 AI 不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果你已完成相关练习，就已看过 `.github/copilot-instructions.md`；如果没有，现在可以快速查看。分配任务前，以只读方式查看存储库中的指令文件，稍后便能观察它们的影响。

在 GitHub 网页界面或 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards**，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

在下一节中，将标题为 `Code lacks documentation` 的议题分配给云智能体。查看 PR 时，注意 TSDoc 文档注释、TypeScript 约定和文件注释头；这些都来自上述指令文件。后续练习会再次检查这些内容。

## 为云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行设置脚本，确保所有依赖项和工具就绪。将任务分配给 Copilot 时也一样，它会以类似软件工程师的方式工作。

云智能体使用 [GitHub Actions][github-actions] 环境执行任务。可以通过在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]，在它开始工作前运行设置步骤，使其获得所需开发工具和依赖项。本实验已预先配置此工作流，以确保顺利完成并提供学习机会。它会为单体 Astro 应用安装 Node.js、项目依赖项、端到端测试所需浏览器，以及完成迁移和填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 仅为这些步骤授予所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如需在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果未在设置步骤中克隆存储库，Copilot 会在步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流类似，但有几点需要注意：

- 它只包含名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会先运行此作业。
- `workflow_dispatch` 触发器允许你在存储库的 **Actions** 选项卡中手动运行工作流。这对测试工作流很有用，无需等待 Copilot 启动它。

## 添加文档

大家都知道文档很重要，但多数项目的信息要么已过时，要么完全缺失。这类技术债务经常无人处理，会拖慢工作效率，让代码库更难维护，也让新开发者更难加入。Copilot 擅长生成文档，因此很适合把这项工作交给 Copilot 云智能体；它会在后台生成文档，后续练习中再审查结果。

1. 在新浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的 PR，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办列表。

> [!NOTE]
> Copilot 可能需要几分钟才能在 PR 中显示待办列表，因为它需要创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方案。

13. 查看待办列表和 Copilot 将执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，了解 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定计划，以及创建代码。

这可能需要几分钟。云智能体的一项主要优势是可异步处理任务，让你专注于其他工作。接下来利用这一点，将另一个任务交给云智能体，同时编写应用的新功能。

## 在游戏详情页添加相关游戏

Copilot 云智能体的一项优势是可以拆分工作：你专注于一组任务时，它可以处理另一组。即使为设计团队添加相关游戏部分不一定耗时，也可以把时间留给其他工作。现在将这项工作分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在议题的第一条评论中看到 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它会像软件工程师一样工作，因此无需持续监控；等任务完成后再审查即可。现在可以继续了解自定义智能体。

## 总结与后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、实现新功能，或协助将代码迁移到其他框架。

本课中你了解了：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作。接下来了解如何创建和使用自定义智能体。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
