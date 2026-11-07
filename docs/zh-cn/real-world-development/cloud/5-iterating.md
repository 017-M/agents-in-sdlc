---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本研讨会中，你已与 GitHub Copilot 协作完成多项改进用户体验和添加功能的任务：为代码添加文档、实现供设计团队迭代的相关游戏功能，以及添加高对比度和浅色模式切换开关等无障碍功能。现在审查代码更改；必要时向 Copilot 提供反馈，改进结果。

### 场景

如前所述，引入生成式 AI 不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循常规 DevOps 流程。现在先审查 GitHub Copilot 生成的文档、相关游戏功能和无障碍功能，再为团队启用审查。

## 安全与 GitHub Copilot 云智能体

Copilot 云智能体会异步且无人监督地执行任务，因此设有安全限制：

- Copilot 对存储库只有读取权限，对它用于编写代码的分支才有写入权限。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- 任何 GitHub Actions 工作流在运行前都需要人工批准。
- [默认情况下，外部资源访问受限](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含代码文档。通过 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法广泛访问外部资源，包括外部 MCP 服务器调用。如有需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。此时请等待它完成，这可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡，审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。检查 TSDoc 文档注释、TypeScript 约定和文件注释头；它们来自练习 2 开始时查看的自定义指令文件。
6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会出现在 PR 检查区域的队列中。顺利的话，单体 Astro 应用的项目检查会通过。这可能需要几分钟。

## 请求 GitHub Copilot 更改

与 Copilot 协作并非单向过程。你可以像 @提及团队成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论并启动新会话处理反馈。由于结果具有非确定性，无法提供固定的请求文本。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明其用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 为 Astro 应用结构创建 README。

1. 添加评论并提及 **@copilot**，请求更新生成的文档。可以使用上述建议，或提出其他期望的文档更改。
2. 选择 **View Session**，观察 Copilot 执行工作。注意它会启动新会话来进行更新。
3. 选择 **Back to pull request** 返回 PR。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直到满意。满意后，将 PR 从草稿转换为准备审查状态，然后合并到 `main` 分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准的指示器。
7. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流会进入 PR 检查队列。顺利的话，单体 Astro 应用的项目检查会通过；这可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，可以继续迭代，请求更新，例如调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时尚未更新自定义指令）。在 **Conversation** 选项卡中发表评论，Copilot 会看到并启动新会话。

## 审查无障碍功能

最后，审查无障碍自定义智能体实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心要求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题中会包含 **[WIP]**。请等待它完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查实现，尤其关注：
   - 用于切换模式的 UI 组件。
   - 用于持久化用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准的指示器。
8. 如果有工作流等待批准，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会进入 PR 检查队列。顺利的话，单体 Astro 应用的项目检查会通过；这可能需要几分钟。
10. **可选：**可以切换到 Codespace 中的此分支，手动测试无障碍功能。在 Codespace 中打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常且刷新页面后仍会保留设置。

可以看到，无障碍自定义智能体引导 Copilot 按照无障碍最佳实践实现功能。如果发现无障碍问题或可改进之处，可以像之前一样在评论中提及 **@copilot** 请求更新。

## 可选练习 - 继续委派工作

云智能体适合处理待办事项，让你专注于其他工作。可以在自己的存储库中再创建几个议题并分配给 Copilot，养成这种协作方式。例如：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体练习路径。在这些课程中，你：

- **检查了存储库附带的自定义指令文件**，以便之后观察其对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换开关。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，使用 `@copilot` 请求更改并批准工作流。

## 回顾与后续步骤

云智能体练习路径已完成。其他练习路径可以补充你已学的内容：

- 🖥️ **[VS Code 练习路径](../../vscode/)**：在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 练习路径](../../cli/)**：通过终端交付两个经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
