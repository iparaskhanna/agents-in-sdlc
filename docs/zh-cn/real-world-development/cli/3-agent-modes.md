---
title: "第 3 课 - 智能体模式：计划和自动驾驶"
description: "使用计划模式确定实现方案，使用自动驾驶模式从议题构建筛选功能，并在交互模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已为项目添加了一个小功能。更大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作流，帮助我们以正确的方式构建正确的功能。本课开启一系列课程，你将遵循典型的智能体驱动开发流程：从议题中生成新功能、确保代码有效且功能符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作将继续使用同一对话和分支。通常，不同文件类型的工作会使用不同分支或 PR；本研讨会为聚焦核心概念而简化了流程。

本课将学习如何：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在计划模式中明确需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续完成该功能时，你还将更新存储库指令、定制现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。待办议题描述了功能，但组合类别等细节需要在编码前达成一致。你将使用计划模式敲定这些决定，然后批准范围明确的自动驾驶实现。

## 背景

将 AI 编码智能体加入开发流程，并不会改变软件开发的基本原则；这些原则反而更加重要！大多数开发者的工作流程都类似于：

1. 从已创建的议题开始，明确要完成的工作。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码审查通过且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多都与上述步骤类似。

遵循这一标准流程，可以确保 AI 生成的代码符合要求，并经过与手写代码相同的审查流程。

## 对话模式

**对话模式**控制智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 在模式间切换：

- **交互模式**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **计划模式**：智能体先制定计划，且不能编辑项目文件。
- **自动驾驶模式**：智能体自主工作，编写代码、运行测试并反复改进，直到认为任务完成。

先使用计划模式审查方案，再使用自动驾驶模式实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果已停止，请重新启动。
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
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求（PR）加入上下文。

## 规划筛选功能

在交给 Copilot 实现功能前，先确定实现方式。对于复杂任务，花些时间规划总是值得的。切换到计划模式并让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到计划模式。确认提示下方的模式标记显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方案。

3. Copilot 可能会询问筛选功能的具体行为。根据自己的偏好回答，没有唯一正确的答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划与已决定的方案不符，请让 Copilot 修改。

## 批准自动驾驶

计划已撰写并审查完毕，现在开始实现。使用自动驾驶模式让 Copilot 执行计划。

自动驾驶模式会让 Copilot 持续迭代，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前安装版本中类似的选项。
2. 确认提示下方的模式标记显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实现，所以请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再确认检查已完成。

## 审查并验证实现

代码生成后，和任何代码一样，在合并前必须审查。检查代码并运行网站，确认功能正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到交互模式。确认模式标记不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 根据议题和规划时达成的决定审查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的结果，并要求 Copilot 修复失败项。

## 试用新功能

代码看起来没问题，但运行效果如何？像之前一样启动应用，通过 Codespaces 转发的端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及计划中约定的组合筛选。
4. 确认重置和无结果时的行为符合议题和已作出的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和下一步

你使用不同的对话模式构建并审查了功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在计划模式中明确需求。
- 使用自动驾驶模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来，使用[自定义指令][next-lesson]，深入了解如何确保生成的代码遵循已记录的规范。

## 资源

- [GitHub Copilot CLI 中的自动驾驶模式][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
