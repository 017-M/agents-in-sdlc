---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成式 AI 的使用离不开上下文。若任务需要按特定方式完成，应确保 Copilot 能获取相应指导。[指令文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。构建完筛选功能后，你将查看 Copilot 使用的指令、添加文档标准，并将其应用于代码。

本课将介绍如何：

- 了解存储库指令和按路径应用的指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践准则和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 接口契约，相关代码更改时应同步更新注释。
- 应保留现有格式和 lint 指南。

通过指令文件，你可以让 Copilot 获取这些准则，并按要求完成工作。

## 指令文件

自定义指令可为 Copilot 提供上下文和偏好，帮助它了解代码风格和需求。你可以指定编码规范、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个文件，适用于存储库中的**所有**请求。它应包含适用于大多数聊天或 CLI 请求的项目级信息。
- `.github/instructions/*.instructions.md` 可用于特定语言、文件类型或任务。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。依赖特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

起始项目已包含一组指令文件。先查看已有内容，再进行更改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件中的项目简介和编码指导。这些指令适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括针对 Astro、Drizzle 数据层、测试等内容的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它指定指令适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它对其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成可复用文件，并为 Copilot 提供代码示例。

## 更新指令以符合团队准则

现有文件已经打下良好基础，但还有不足。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分（大约在第 35 行）。
2. 将以下内容添加为该部分的最后一项：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 应用更新后的指导

Copilot CLI 会在对话启动时加载存储库指令。保存更新后的指令后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，检查更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认它们准确说明代码。

## 总结与后续步骤

你已了解 Copilot 如何从指令文件获取上下文，并将新标准应用于功能。本课中你：

- 了解了存储库指令和按路径应用的指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 查看了指令文件对代码的影响。

接下来，你将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保以一致的方式运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot - 指令文件及其他资源][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
