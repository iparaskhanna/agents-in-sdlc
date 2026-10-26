---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "了解如何使用斜杠命令管理上下文、选择模型、共享会话，以及选择性地委托给云智能体。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。这些命令可以使用高级功能、查看后台信息并配置工具。你已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。下面再了解一些实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索用于并行工作、工作树以及委托给云智能体的可选命令。

## 场景

你已完成 CLI 核心工作流。现在来看看一些额外功能：管理上下文、切换模型、共享会话，以及选择性地将工作委托给 [Copilot 云智能体][about-cloud-agent]。

## 查看 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会达到模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动它。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及上下文在系统指令、工具、消息和剩余空间之间的分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口即将占满时自动压缩上下文。想自行决定压缩时机时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有不同偏好。Copilot CLI 可以列出并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型及其使用信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和分享经验有助于每个人改进 AI 工具的使用方式。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享会话，请选择适合的目标位置，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送给他人或发布之前，请先审查导出的内容。会话导出文件可能包含提示、回复和项目详细信息。

是否发布链接或 gist 由你决定。不要发布团队不打算公开的存储库或对话内容。

## 可选：扩展并行工作或委托任务

核心研讨会到此结束。Copilot CLI 还提供了适用于大型任务的命令：

- `/fleet` 可将相互独立的子任务分配给子智能体并行处理。
- `/worktree` 可为单独任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云智能体，由它异步工作并可能创建拉取请求。

这些命令会创建额外的工作树或远程工作，因此属于可选内容。尝试前请先启动一个范围明确的新任务，并按常规工作流审查结果。若要深入了解异步智能体工作，请继续学习[云智能体研讨会][cloud-workshop]。

## 总结和下一步

通过斜杠命令，可以配置 Copilot CLI、共享会话并查看幕后运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解了如何使用 `/share` 导出或共享会话。
- 探索了用于并行工作、工作树以及委托给云智能体的可选命令。

Copilot CLI 还有更多斜杠命令和功能值得探索！接下来[回顾所学内容][next-lesson]，并了解继续学习的后续步骤。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
