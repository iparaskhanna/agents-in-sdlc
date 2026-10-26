---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能、指令更新、技能更新、质量保证（QA）配置和测试都保存在同一分支上。现在将它们一起审查并创建拉取请求。你已亲自合并星级评分 PR；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会把功能、指令更新、技能更新和 QA 智能体拆分为几个 PR。为简化研讨会流程，筛选功能和质量工作流一直使用同一对话和分支，所有内容都将进入此 PR。

本课将学习如何：

- 了解 Agent Merge 及其如何自动化合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 希望自动处理剩余 PR 工作，同时由开发者掌控合并授权。

## 介绍 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题，例如修复失败的持续集成（CI）检查、回应审查评论，以及在需要时进行变基，然后在存储库允许时启用 GitHub 自动合并。

此前你一直亲自选择 **Merge pull request**。Agent Merge 可以接手该流程，但启用前仍应审查工作并确认已准备就绪。

## 使用 Agent Merge 管理 PR

代码已完成。现在一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监视 PR 的持续集成过程。全部成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果流程受阻，请查看报告的原因，并在存储库允许时按常规审查流程手动完成合并。

## 总结和下一步

你已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解了 Agent Merge 及其如何自动化合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 在审查后启用 Agent Merge，并确认 PR 已合并。

接下来，[探索 Copilot CLI 中更多实用的斜杠命令][next-lesson]，管理上下文、模型、共享会话并选择性地委托云智能体。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
