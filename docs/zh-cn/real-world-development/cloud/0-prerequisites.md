---
title: "练习 0：准备工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

开始 Copilot 云智能体练习前，需要先做好准备。您将创建 Tailspin Toys 存储库的个人副本，并启动一个 [codespace][codespaces]，用于编辑指令文件和审查云智能体生成的工作。

## 设置实验存储库

您将从[模板存储库][template-repository]创建自己的实例。新实例会包含实验所需的全部文件，供您在后续练习中使用。

1. 在新的浏览器窗口中，打开 [Tailspin Toys 模板][tailspin-template]。
2. 在实验存储库页面上选择 **Use this template**，然后选择 **Create a new repository**，创建个人副本。

    ![Use this template 按钮](../../../_images/ex0-use-template.png)

3. 如果您正在参加由 GitHub 或 Microsoft 举办的活动，请遵循导师提供的说明。否则，可以在您有权使用 GitHub Copilot 的组织中创建存储库。

    ![输入存储库模板设置](../../../_images/ex0-repository-settings.png)

4. 记下您创建的存储库路径（**organization-or-user-name/repository-name**），稍后会用到。

## 创建 codespace

接下来，使用 codespace 完成本实验。

[GitHub Codespaces][codespaces] 是基于云的开发环境，可让您直接在浏览器中编写、运行和调试代码。它提供功能完整的 IDE，并支持多种编程语言、扩展和工具。

1. 打开刚创建的存储库。
2. 选择绿色的 **Code** 按钮。

    ![选择 Code 按钮](../../../_images/ex0-code-button.png)

3. 选择 **Codespaces** 选项卡，再选择 **+** 按钮创建新的 Codespace。

    ![创建新的 codespace](../../../_images/ex0-create-codespace.png)

创建 codespace 需要几分钟，但仍比手动安装所有服务快得多。等待期间，可以先了解 GitHub Copilot 的其他功能，稍后我们会继续介绍。

> [!CAUTION]
> 后续练习还会用到这个 codespace。目前请将它保留在浏览器标签页中。

> [!NOTE]
> 本研讨会可以在 codespace 或本地 [dev container][dev-containers] 中运行。两者都已配置好所需的先决条件。如果想在本地运行，请在 VS Code 中打开克隆的存储库，并在出现提示时选择 **Reopen in Container**。VS Code 将构建与 codespace 相同的开发容器。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## 总结

恭喜，您已创建实验存储库的副本，并开始创建后续练习需要的 codespace。接下来，您将添加云智能体要遵循的自定义指令。

## 下一步

继续学习[练习 1：自定义指令][next-lesson]。

## 资源

- [GitHub Codespaces 概述][codespaces]
- [从模板创建存储库][template-repository]
- [Codespaces 入门][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
