---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并执行操作，例如运行测试。开发者需要找到所有相关文件、进行修改、运行测试、调试、找出遗漏文件并再次更新，流程可能不断重复。

Copilot 智能体模式可以帮助处理这些工作。

Copilot 智能体模式可在 IDE 中更自主地工作。它会先探索现有项目结构，执行所需更新，运行测试等任务，并自动修复发现的问题。现在看看如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体的名称相似，但适用于不同场景。智能体模式在 IDE 中执行任务，支持快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何：

- 让 Copilot 智能体模式探索项目、查找相关文件并协调更改。
- 使用 Copilot 智能体模式跨 UI 和数据层实现新功能。
- 在将代码合并到代码库之前审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断增长，因此你希望允许用户按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先浏览 Tailspin Toys 网站，了解现有功能。

这个网站是带有开发者主题的桌游众筹平台，可列出游戏并显示详细信息。它是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库；没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能由使用标准 DOM API 的小型 Astro `<script>` 添加。

### 启动网站

项目提供了 `npm run dev` 命令，用于启动单个 Astro 开发服务器。通过以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 将其关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，从终端选择 localhost URL 会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到 Codespace 的私有隧道，网站服务器正在其中运行。

### 浏览网站

网站运行后，可以探索以下主要功能：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

网站的初始实现已可正常运行，但还可以添加新功能。先查看待办事项。通过模板设置项目时，系统已自动创建一组议题。让 GitHub Copilot 显示这些项目。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。此智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器示例](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用了 Claude Sonnet 4.5，因此将其列入说明，以便获得较一致的体验。但由于大型语言模型 (LLM) 具有概率性，你收到的建议可能与示例不同，这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度为项目排序。

## 查看指令文件

让智能体生成代码前，先查看用于提供工作上下文的指令文件。本练习将使用 [用户界面 (UI) 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了为网站添加功能的方法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 针对不同文件类型的指令链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件的位置，因此可以复用内容、拆分复杂指令，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用做三项单独更新：

- 在数据层 (`src/lib/`) 添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

此外，合并前需要运行并通过测试。Copilot 智能体模式可以完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表仍选中 **Agent**。

   ![Chat 视图中的智能体选择器示例](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，让 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并查找相关文件，包括数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的示例](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂度，这可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo**。Copilot 完成后，所有更改都会提供 **Keep** 或 **Undo** 选项；工作尚未完成时无需选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 提示用户确认运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有关系！Copilot 会在生成代码和运行测试之间迭代，直到完成任务且未检测到错误。

   ![Copilot 智能体模式完整 Chat 会话](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在潜在问题。

> [!CAUTION]
> 始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，在浏览器中体验新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能。你了解到如何：

- 使用智能体模式跨 UI 和数据层实现新功能。
- 让智能体模式探索项目、查找相关文件并协调更改。
- 在合并到代码库前审查智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 奖励探索练习 - 实现分页

随着游戏列表不断增长，需要添加分页。运用本练习学到的技能，让 Copilot 更新网站以实现分页。需要考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 确定分页方式，例如允许用户选择每页大小或使用固定大小。
- 在提示词中提供足够指导，让 Copilot 按预期实现功能。
- 你可能需要与 GitHub Copilot 迭代，通过请求更改并补充上下文来调整结果。这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
