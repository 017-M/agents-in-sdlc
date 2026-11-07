---
title: "练习 0：先决条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先做好准备。你将创建 Tailspin Toys 存储库的副本、启动一个用于操作的 [Codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已正常运行。

## 设置练习存储库

你将从[模板][template-repository]创建自己的存储库副本，其中包含练习所需的全部文件。

1. 在新的浏览器窗口中打开 [Tailspin Toys 模板][tailspin-template]。
2. 在练习存储库页面中选择 **Use this template**，然后选择 **Create a new repository**。

    ![“Use this template”按钮](../../../_images/ex0-use-template.png)

3. 如果你参加的是 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，请在你有权使用 GitHub Copilot 的组织中创建存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下新存储库的路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建存储库时，系统会自动创建一组 GitHub 议题作为待办事项。整个研讨会都会使用这些议题，无需自行创建。

## 创建 Codespace

接下来，你将使用 Codespace 完成练习。

[GitHub Codespaces][codespaces] 是一种基于云的开发环境，可让你直接在浏览器中编写、运行和调试代码。它提供功能齐全的 IDE，支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，然后选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

Codespace 创建需要几分钟，但仍比手动安装所有服务快得多。等待期间，可以了解 GitHub Copilot 的其他功能，接下来就会用到。

> [!CAUTION]
> 后续练习还会回到此 Codespace。现在先将它保留在浏览器标签页中。

> [!NOTE]
> 本研讨会设计为在 Codespace 或本地 [dev container][dev-containers] 中运行。这两种方式都预装了顺利完成练习所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 会构建与 Codespace 相同的 dev container。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

若要使用 GitHub Copilot Chat 智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。若使用 GitHub Codespace，该扩展通常已安装。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开“扩展”视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，可能需要使用 GitHub 账户进行身份验证才能启用它。

1. 返回 Codespace。
2. 如果编辑器右侧没有显示 Copilot Chat，请选择 Codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息，然后按 <kbd>Enter</kbd>。这会激活 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示登录 GitHub 账户。按照说明完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 完成身份验证后，Copilot Chat 窗口会显示出来。

## 总结

恭喜！你已创建练习存储库的副本，并开始创建 Codespace。开始编写代码时会用到它。

## 下一步

现在开始使用 Copilot。继续学习[练习 1 - 自定义指令][next-lesson]，让 Copilot 了解项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [开始使用 Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
