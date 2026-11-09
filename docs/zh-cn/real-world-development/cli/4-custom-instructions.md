---
title: "第 4 课 - 使用自定义指令指导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务有特定要求，就应让 Copilot 能够访问相关指导。[指令文件][instruction-files]不仅说明您希望生成什么代码，也说明代码应该如何组织。现在，您已经构建了筛选功能，接下来将查看 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

本课将学习如何：

- 了解存储库指令和按路径限定的指令文件如何提供给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值，包括存在时可注入的 `db` 参数。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

您将使用指令文件，让 Copilot 获取正确的信息并按照上述实践完成任务。

## 指令文件

自定义指令可为 Copilot 提供上下文和偏好，帮助它理解您的编码风格和要求。您可以指定编码约定、库以及注释风格，也可以为整个存储库或特定文件类型创建指令。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个存储库级指令文件，涵盖该存储库大多数请求都适用的信息。
- `.github/instructions/*.instructions.md` 文件可针对特定语言、文件类型或任务提供指导。

> [!NOTE]
> 其他指令格式及其支持情况会因环境而异。使用特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 查看本项目的自定义指令文件

初始项目已包含一些指令文件。先查看已有内容，再进行更改并观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此文件适用于 Copilot 在此存储库中的所有交互。
4. 打开 `.github/instructions` 文件夹，查看其中的指令文件，包括 Astro、Drizzle 数据层、测试等文件。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用 glob 确定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他指令文件和项目文件。这种做法可将大型指令集拆分成可复用的小文件，并让 Copilot 参考具体示例。

## 更新指令以符合团队指导

现有文件是不错的起点，但仍有不足。修改核心 `copilot-instructions.md`，要求为新生成的 TypeScript 添加 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在启动对话时加载存储库指令。保存更新后，退出并恢复筛选会话，Copilot 便会在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明代码。

## 总结和后续步骤

您了解了 Copilot CLI 如何读取指令文件，并将新标准应用于功能。本课中，您：

- 查看了存储库指令和按路径限定的指令文件如何提供给智能体。
- 更新了指令文件以确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，您将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件等资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
