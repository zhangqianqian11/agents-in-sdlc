---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认方案，使用 Autopilot 根据 issue 实现筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一个小功能，但更大的更改需要更完善的流程。GitHub Copilot CLI 可配合组织现有的工作流，帮助您以正确方式构建正确功能。本课将开始一系列智能体驱动的开发练习：从 issue 出发生成新功能，验证代码并检查功能行为，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作将继续使用同一对话和分支。通常，不同文件类型的工作会使用不同分支或 PR；本课程为了聚焦核心概念而简化了流程。

本课将学习如何：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，您还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的 issue 已描述此功能，但在编写代码前，还需要确认如何组合类别等细节。您将使用 Plan 模式解决这些问题，然后授权 Autopilot 执行范围明确的实现。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则。相反，这些原则变得更加重要。大多数开发者的流程大致如下：

1. 从描述需求的 issue 开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

坚持这种标准流程，可确保 AI 生成的代码满足需求，并经历与手写代码相同的审查过程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：您与智能体协作。智能体提出更改建议，并等待您输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作，编写代码、运行测试并迭代，直至任务完成。

先使用 Plan 模式并审查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的 issue。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，并返回 **Session** 选项卡。

现在提示中应出现 `#7`（或类似编号）。`#` 可将 GitHub issue 或拉取请求（PR）带入上下文。

## 规划筛选功能

规划能让您在将实现任务交给 Copilot 前，先确定功能实现方式。对于复杂任务，花些时间规划总是值得的。现在切换到 Plan 模式并让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方显示 **Plan**。
2. 在上一步添加的 issue 引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定适当的实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据您的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动浏览并审查计划。
6. 要求 Copilot 修改任何不符合您决定的部分。

## 批准 Autopilot

计划制定并审查完毕后，就可以开始实现。让 Copilot 使用 Autopilot 执行任务。

1. 选择 **Accept plan and build on autopilot (recommended)**，或您所用版本中含义相同的选项。
2. 确认提示下方显示 **Autopilot**。
3. 观察 Copilot 如何根据计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再判断检查是否完成。

## 审查并验证实现

生成的代码与其他代码一样，必须在合并前审查。现在检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能实现和测试。
3. 将结果与 issue 和规划阶段的决定进行比较。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决任何失败。

## 体验新功能

代码看起来没问题，但运行是否正常？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 提示端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的组合筛选。
4. 确认重置和无结果时的行为符合 issue 和您的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

您使用不同的对话模式来构建和审查功能。本课中，您：

- 从 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，您将[使用自定义指令][next-lesson]深入了解代码生成方式，确保代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
