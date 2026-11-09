---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 VS Code 练习前，需要先做好准备。您将创建 Tailspin Toys 存储库的个人副本、启动一个用于开发的 [codespace][codespaces]，并确认编辑器中的 GitHub Copilot Chat 已启动。

## 设置实验存储库

您将从[模板存储库][template-repository]创建实例。新实例会包含实验所需的全部文件，供后续练习使用。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **Use this template**，然后选择 **Create a new repository**。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果您正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在您有权使用 GitHub Copilot 的组织中创建存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下您创建的存储库路径（**organization-or-user-name/repository-name**），稍后会用到。

> [!NOTE]
> **待办事项已准备就绪**
>
> 从模板创建存储库时，系统会自动为您创建 GitHub issue 待办事项。整个研讨会都会使用这些 issue，无需自行创建。

## 创建 codespace

接下来，使用 codespace 完成本实验。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让您直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但仍比手动安装所有服务快得多。等待期间，可以先了解 GitHub Copilot 的其他功能。

> [!CAUTION]
> 后续练习还会用到这个 codespace。目前请将它保留在浏览器标签页中。

> [!NOTE]
> 本研讨会可以在 codespace 或本地 [dev container][dev-containers] 中运行。两者都已配置好所需的先决条件。如果要在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 使用 GitHub Copilot Chat 和智能体模式

要使用 GitHub Copilot Chat 智能体模式，需要在 IDE 中安装 GitHub Copilot Chat 扩展。使用 GitHub Codespace 时通常已安装此扩展。

> [!TIP]
> 如果尚未安装 GitHub Copilot Chat 扩展，可以从 [Visual Studio Code Marketplace][copilot-chat-extension] 安装。也可以在 Visual Studio Code 中打开 Extensions 视图，搜索 **GitHub Copilot Chat** 并选择 **Install**。

安装扩展后，您可能还需要使用 GitHub 帐户进行身份验证。

1. 返回 codespace。
2. 如果编辑器右侧尚未显示 Copilot Chat，请选择 codespace 顶部的 **Copilot Chat** 图标。
3. 在 Copilot Chat 窗口中输入类似“Hello world”的消息并按 Enter，以启动 Copilot Chat。
4. 如果尚未通过身份验证，系统会提示您登录 GitHub 帐户。按照提示完成身份验证。

    ![Copilot Chat 身份验证提示示例](../../../_images/ex1-copilot-authentication.png)

5. 身份验证完成后，Copilot Chat 窗口应会显示出来。

## 总结

恭喜，您已创建实验存储库的副本，并开始创建后续编写代码时要使用的 codespace。

## 下一步

从[练习 1：自定义指令][next-lesson]开始让 Copilot 工作，并教它遵循项目约定。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
