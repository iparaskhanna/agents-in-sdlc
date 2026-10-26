---
title: "第 2 课 - 添加星级评分：快速见效"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查并验证，然后合并第一个拉取请求。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在来为项目完成首次更改。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。让智能体显示评分、审查更改并将其作为第一个拉取请求合并。

本课将学习如何：

- 在功能分支上开始一段范围明确的 Copilot 对话。
- 让智能体对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，游戏详情页已经显示该评分。但主页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，让智能体在每张卡片上显示现有评分；这是适合首次会话的小型独立更改。

## 对话的组成

**对话**是你使用 Copilot CLI 处理任务的地方。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可供稍后继续使用，文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你的提示和智能体的回复、智能体探索和编辑文件时使用的工具，以及可通过 `/diff` 检查的更改。

## 开始对话并请求更改

开始新的对话来实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目、定位用于显示游戏详情的文件并编写所需代码。现在，你已使用 Copilot CLI 添加新功能。

## 审查差异

所有 AI 生成的更改在合并前都应审查，即使改动很小也不例外。现在在 Copilot CLI 中查看更改。

1. 输入 `/diff` 并检查每个更改过的文件。
2. 确认游戏卡片在有评分时显示数字评分，无评分（`starRating` 为 `null`）时显示 `No rating yet`。
3. 确认测试覆盖了这两种情况。
4. 查看 Copilot 运行检查的结果，并要求它修复任何失败项。
5. 审查完毕后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 具有概率性而非确定性，因此代码可能与示例不同。请审查实际行为，不要要求实现与示例完全一致。

## 检查更改

不能只看代码就认定它有效。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的界面。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示五星制评分。
4. 返回 Copilot，让它停止刚启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成！现在创建拉取请求（PR），将新代码合并到项目中。

1. 让默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和下一步

恭喜！你使用 GitHub Copilot CLI 交付了第一个更改。本课中，你：

- 在功能分支上开始一段范围明确的 Copilot 对话。
- 引导智能体修改游戏卡片。
- 使用 `/diff` 审查更改。
- 运行应用，在转发的浏览器中确认星级评分。
- 创建并合并第一个拉取请求。

接下来，从筛选议题开始，使用计划和自动驾驶模式[构建更大的功能][next-lesson]。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
