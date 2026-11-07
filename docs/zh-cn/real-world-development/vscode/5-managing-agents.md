---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

使用 GitHub Copilot 智能体模式时，Copilot 会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作在本机实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联审查每项差异、接受或拒绝单项更改，并通过后续提示引导对话、完善或扩展工作，无需离开编辑器。

本练习将介绍如何：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行时引导智能体，在高对比度模式之外添加浅色模式切换开关。

## 场景

[练习 4][previous-lesson]中的无障碍自定义智能体正在实现高对比度模式。工作期间，设计团队又提出新需求：为觉得默认主题太暗的用户添加浅色模式切换开关。无需从头开始新对话，你将引导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会在聊天中显示差异和 **Keep** / **Undo** 控件。

1. 返回练习 4 中打开的 Copilot Chat 视图。
2. 智能体工作时，滚动聊天查看每项工具调用，包括文件读取、文件编辑以及需要运行的终端命令。
3. 智能体提出文件编辑时，差异会内联显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体重新尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看智能体在本次会话中修改的完整文件集。

> [!NOTE]
> 如果工具调用请求权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式的实现。完成时会看到总结更改内容的最终消息。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一对话中继续添加浅色模式切换开关。由于继续使用同一聊天会话，智能体会保留之前工作的上下文，包括修改的组件、样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续之前的工作，探索已做的更改并提出浅色模式的实现。
3. 像审查高对比度模式更改一样，审查每项新编辑，并使用 **Keep** 或 **Undo**。
4. 如果智能体的实现方向不符合预期（例如尝试以不喜欢的方式重构切换 UI），可以选择聊天输入框旁的 **Stop** 按钮停止，再发送澄清提示。

> [!TIP]
> 引导进行中的会话比从头开始更快。可以用后续提示要求调整（“简化 CSS，改用 CSS 变量，不要重复选择器”）、纠正方向（“不要添加第三个切换开关，将浅色模式集成到现有设置面板”）或扩展范围（“为两个切换开关都添加键盘快捷键”）。

## 审查工作集

提交前快速审查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查已更改文件的完整列表。应能看到 Astro 组件、样式和相关测试的更新。
3. 打开一些已更改文件并查看差异。确认其中体现了自定义智能体的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结与后续步骤

你已使用 Copilot Chat 智能体模式监控并引导进行中的会话：逐项审查文件编辑、接受或拒绝更改，并在保留上下文的同时发送后续提示扩展工作。

本课中你了解了：

- 实时监控智能体工作。
- 接受或拒绝单项文件编辑。
- 使用后续提示在会话进行时引导智能体。

在[下一课][next-lesson]中，你将通过拉取请求发布无障碍改进，并审查本地自定义智能体在实验中生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
