---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

上下文对于生成式 AI 至关重要。如果任务必须以特定方式完成，或 Copilot 需要了解背景信息，就应确保它可以获取这些上下文。[指令文件][instruction-files]可提供相关指导，帮助 Copilot 理解你不仅需要它完成什么，还需要它如何完成。

本练习将介绍如何：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义指令和按路径应用的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 练习路径不同，此处不会使用前后对比提示词。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的效果。在之后审查云智能体生成的 PR 时，你会看到指令文件产生的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

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

阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 和多个用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于你与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括针对 Astro、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。顶部的 `applyTo` 字段指定一个相对于存储库根目录的 glob，用于匹配适用文件，例如 `**/*.test.ts`。
5. 查看项目中有关创建单元测试的具体指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部，查看它对其他指令文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将大型指令集拆分成可复用文件，并为 Copilot 提供代码示例。此处路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 注释和文件注释头的规则。

## 添加存储库级标准

接下来添加一条项目级规则，要求代码注释使用 TSDoc 文档注释和文件注释头。云智能体会在本练习路径后续处理分配给它的议题时使用此规则。

编辑前先创建分支。云智能体会从议题目标分支读取指令，但仍应以分支提交更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录存储库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（大约在第 27 行）。这里记录了项目编码标准，但尚无代码文档规则，这也是之前生成的辅助函数没有文档注释的原因。
3. 在现有标准后添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级文件 `.github/copilot-instructions.md` 中，为所有请求提供指导；也可使用 `*.instructions.md` 针对特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将从 `main` 为 Copilot 分配议题，因此必须先将指令更新合并到 `main`，云智能体才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，云智能体会据此读取指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目的处理方式。

## 总结与后续步骤

你已了解 Copilot 如何从项目指令文件获取上下文，并向 `.github/copilot-instructions.md` 添加存储库级标准。后续练习中，云智能体生成的拉取请求会体现此标准。

接下来，将[第一个议题分配给 GitHub Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot - 指令文件及其他资源][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
