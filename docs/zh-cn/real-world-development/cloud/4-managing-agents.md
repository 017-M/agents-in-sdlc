---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几个练习中，你让 Copilot 云智能体执行了三项改进用户体验和添加功能的任务。云智能体虽然会异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心中，可以查看所有具有未关闭 PR 的智能体任务、探索已执行的操作，并引导正在进行的会话。

本课将介绍如何：

- 浏览智能体页面并监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将任务分配给智能体实现高对比度模式后，团队意识到也可以同时添加浅色模式。由于已经在更新网站样式并添加切换功能，将其一并实现很合理。现在引导智能体，使它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主区域和左侧区域中的任务列表，其中应包含：
   - 为代码库更新文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的其他步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 工作进度和采用的方法而变化。

4. 同时查看右侧显示的拉取请求 (PR) 面板，在其中查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

查看正在进行的任务后，让 Copilot 在实现高对比度模式时添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 采用的名称和当前进度而异。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始实际工作。出现类似消息时即可确认。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![突出显示“Steer active session while Copilot is working”对话框的云智能体任务页面](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 注意 Copilot 会确认提示词，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息后，它会在完成当前步骤后将新要求加入工作流程。

这可能还需要几分钟。等待时可以暂停并回顾目前学到和体验的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面——监控和引导 GitHub Copilot 云智能体任务的中心位置。可以跟踪所有进行中和已完成的任务、审查执行的工作，并重定向正在进行的任务以调整范围或补充指导。

本课中你了解了：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
