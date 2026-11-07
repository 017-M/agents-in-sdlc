---
title: "练习 1 - 自定义指令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

上下文对于生成式 AI 至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，就应确保它可以获取这些上下文。[指令文件][instruction-files]是提供代码生成指导的重要方式。

本练习将介绍如何：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义指令和按路径应用的指令文件传递给 Copilot。
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。
- 再次运行相同提示词，观察生成的代码如何采用新标准。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循设置的标准。Copilot 具有概率性。本练习旨在让你观察更新指令前后行为的变化趋势，而不是逐字匹配输出。

## 指令文件

### 场景

Tailspin Toys 制定了一些开发实践准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加文档。
- 每个文件开头都应添加描述文件用途的注释块。

通过指令文件，你可以让 Copilot 获取这些信息，并遵循相应实践完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它了解代码风格和需求。你可以指定编码规范、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个文件，适用于存储库中的**所有**请求。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可用于特定任务或文件类型，例如 TypeScript、Astro、UI 组件或新单元测试。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

创建指令文件的完整讨论超出了本研讨会范围。示例项目展示了一种有代表性的方法：

- 将 `copilot-instructions.md` 用于项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明应如何或不应如何编写。

指令文件没有唯一的正确写法，AI 的使用方式也不止一种。通过实践找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应具备完善的指令文件集合。浏览此项目时，你会看到针对 [UI 更新][ui-instructions]和 [Astro][astro-instructions]等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应练习会在适用时介绍具体方式。
>
> 需要模板或起点？浏览包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花点时间阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 和多个用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括针对 Astro、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段指定一个相对于存储库根目录的 glob，用于匹配适用文件，例如 `**/*.test.ts`。
5. 查看项目中有关创建单元测试的具体指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它对其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将大型指令集拆分成可复用文件，并为 Copilot 提供代码示例。此处路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 注释和文件注释头的规则。

## 创建更改分支

遵循最佳实践，为更改创建分支。

1. 返回上一练习中使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

先在当前指令文件不变的情况下发送提示词，观察自定义指令的效果。稍后会更新文件并重新发送相同提示词进行比较。

1. 关闭之前练习留下的编辑器选项卡，让 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中的智能体选择器示例](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，生成的辅助函数是带类型的函数，以 `db` 客户端作为第一个参数，并返回带类型的发行商数组。这源自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此它也可能在未被要求时添加文档注释。即使如此也没关系，本练习的重点仍是观察添加指令后的一致性改进。

## 添加存储库级标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项存储库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。这里记录了项目编码标准，但尚无代码文档规则，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有标准后添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，指令文件可放在存储库级文件 `.github/copilot-instructions.md` 中，为所有请求提供指导；也可使用 `*.instructions.md` 针对特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 重新运行提示词并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除对话内容。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送**与之前相同的提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意生成的文件现在以类似下面的注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意生成的函数现在包含类似下面的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会在此基础上继续。

## 提交指令并推送分支

指令文件和存储库中的其他资源一样，使用相同的源代码管理流程。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他支持性更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求，并关联待办事项中的文档标准议题。Copilot 可以通过已连接的 GitHub 工具查找议题、创建用于关闭该议题的 PR 并合并它。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已配置 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。后续练习会介绍这种连接如何工作（模型上下文协议）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求你**允许**它代表你执行 GitHub 操作时，请先审查命令，再酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并将本地分支切回 `main`：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求你允许执行 GitHub 操作和 shell 命令时，请先审查命令，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结与后续步骤

你已了解 Copilot 如何从指令文件获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示词，观察 Copilot 如何遵循**现有**指令。
- 向 `.github/copilot-instructions.md` 添加存储库级标准。
- 再次发送相同提示词，观察生成代码如何采用新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，在[智能体模式][next-lesson]中运用这些指令，构建跨代码库的新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot - 指令文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
