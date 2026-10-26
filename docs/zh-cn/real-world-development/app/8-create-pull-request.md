---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选实现、指令更新、技能更新、质量保证（QA）配置文件和测试都保存在同一分支上。现在一并审查这些内容并创建拉取请求。你已自行合并星级评分拉取请求（PR）；这次将使用 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体拆分成几个 PR。为简化研讨会流程，整个筛选和质量工作流都保留在同一会话和分支中，并纳入此 PR。

本课将学习如何：

- 了解 Agent Merge 及其如何自动化合并流程。
- 检查完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 希望自动处理剩余 PR 工作，同时由开发者掌控合并授权。

## Agent Merge 简介

**Agent Merge** 可在 GitHub Copilot app 中自动处理拉取请求合并前的其余工作。启用后，应用会话会读取 PR 并处理阻塞项，例如修复失败的持续集成（CI）检查、回应审查评论和按需变基；GitHub 允许后就会合并。它在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

此前你一直自行选择 **Merge pull request**。Agent Merge 可以接手此职责，但要编辑代码和执行合并仍需要你的明确授权。授予合并权限前，请审查它获准执行的操作和实际工作。

## 使用 Agent Merge 管理 PR

代码已完成并审查。现在让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁的下拉菜单。
3. 选择 **Agent Merge**。按钮会变为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮启动流程。仅在下拉菜单中选择选项不会启动流程。

Agent Merge 流程启动后会：

- 创建带标题和说明的拉取请求。
- 如果会话从议题启动，则在 PR 正文中引用相关议题。
- 对目标分支变基，或处理潜在合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控其他开发人员或 Copilot 代码审查提供的 PR 反馈，并更新代码以解决评论。
- 可以选择在所有检查成功后自动合并 PR。

我们希望 Agent Merge 在所有检查通过后合并 PR。

5. 在授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟进处理进度并解决报告的阻塞项。继续前确认 PR 状态为 **Merged**；启用合并权限或智能体正在运行，都不能证明 PR 已完成合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或缺失的权限。继续前请先解决这些阻塞项。

## 总结和下一步

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动化合并流程。
- 检查了完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

接下来，[使用现有画布并创建分类画布][next-lesson]，探索如何借助画布检查、规划和可视化工作。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
