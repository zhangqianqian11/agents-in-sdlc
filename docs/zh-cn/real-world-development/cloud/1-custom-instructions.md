---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：准备工作][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务有特定要求，或 Copilot 需要了解背景信息，就应确保它能访问这些内容。[指令文件][instruction-files]可提供相关指导，让 Copilot 不仅了解您希望完成什么，也了解您希望如何完成。

本练习将学习如何：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全局标准。

> [!NOTE]
> 与 VS Code 和 CLI 课程不同，这里不通过同一提示的前后对比来演示效果。Copilot 云智能体会异步处理 GitHub issue，因此难以实时并排展示变化。稍后审查云智能体创建的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式并具有现代风格。
- 应通过 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加注释块，说明文件用途。

您将使用指令文件，确保 Copilot 获取正确的信息并按照这些实践执行任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解您的编码风格和要求。您可以指定编码约定、库和注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个存储库级指令文件，提供大多数聊天或 CLI 请求都适用的项目信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 可针对特定任务或文件类型提供指导，例如 TypeScript 语言、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体会在生成代码时使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

本研讨会不详细介绍如何创建指令文件，但示例项目展示了常见方法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目说明、目录结构和全局编码标准。
- 使用 `*.instructions.md` 针对文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，确保说明清晰，并提供代码示例。

指令文件没有唯一正确的写法，与 AI 的其他用法一样，需要通过实践找到适合项目的方法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应具备可靠的指令文件集合。查看本项目时，您会发现适用于 [UI 更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令文件。
>
> Copilot 也可以帮助生成指令文件。不同界面提供的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在合适位置介绍具体方式。
>
> 想找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中收集了指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目的自定义指令文件

花些时间查看本存储库提供的指令文件：包括一个核心 `copilot-instructions.md` 和多个适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。注意 **Code standards** 中的 **GitHub Actions Workflows** 子部分。这些指导适用于您与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹查看，其中包含 Astro、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用 glob 确定指令适用的文件；例如，符合 `**/*.test.ts` 的 TypeScript 测试文件会匹配。
5. 查看该文件中针对单元测试的具体指导。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。注意它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件。这样可将大型指令集拆分成可复用的小文件，并提供生成代码时可参考的示例。（这些路径相对于指令文件，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 注释和文件注释头的要求。

## 添加新的存储库标准

接下来需要进行的唯一编辑，是添加一条项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。云智能体稍后处理您分配的 issue 时会采用这条规则。

编辑前先创建分支（云智能体会读取 issue 目标分支中的指令，但仍建议干净地提交更改）：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

存储库级 `.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加编码标准可以改善代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应接近第 27 行）。此处记录了项目编码标准，但尚无代码文档要求。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在 `.github/copilot-instructions.md` 中定义存储库级全局指导，也可以通过 `*.instructions.md` 为特定语言、文件类型或任务提供指导。存储库级文件适合存放代码注释规则等全局标准。

## 提交、推送并合并指令更新

云智能体会读取 issue 目标分支中的指令。下一课分配 issue 时，Copilot 会从 `main` 创建分支，因此必须先将指令更新合并到 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并合并。下一课中，云智能体便会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支和 PR 步骤，直接提交到 `main`。此处采用分支，是为了演示真实项目中的工作方式。

## 总结和后续步骤

您已查看 Copilot 如何获取本项目的上下文，并向 `.github/copilot-instructions.md` 添加了存储库级标准。后续课程中，您会在云智能体生成的 PR 中看到这条标准的作用。

接下来，[将第一个 issue 分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好 Copilot 自定义指令的五个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
