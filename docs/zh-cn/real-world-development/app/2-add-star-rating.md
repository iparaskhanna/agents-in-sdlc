---
title: "第 2 课 - 添加星级评分：快速见效"
description: "在 GitHub Copilot app 中启动第一个智能体会话，对游戏卡片进行一项小改动，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

浏览工作区并尝试快速聊天后，现在可以启动**智能体会话**，对项目进行第一次更改。此次改动很小：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。让智能体显示评分、审查更改，并通过第一个拉取请求合并更改。

本课将学习如何：

- 启动智能体会话并了解其结构。
- 要求智能体对项目进行一项范围明确的小改动。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认更改。
- 打开并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，该评分已显示在游戏详情页上。但主页的游戏卡片只显示标题、类别、发行商和描述。作为热身，让智能体在每张卡片上显示现有评分；这项小型独立更改非常适合作为第一个会话任务。

## 会话的组成

**会话**是与智能体的对话，在独立工作区中运行。每个会话都有**专用的 git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而不会造成更改冲突。会话按存储库分组显示在侧边栏中，选择任一会话即可切换。

会话中包含三类内容：与智能体的**对话**、智能体探索和编辑文件时的**工具活动**，以及带有差异的**已更改文件**列表。

## 启动会话并请求更改

现在启动新会话，探索项目并实现功能。在[上一课][prior-lesson]中，你从 GitHub 存储库添加了项目。接下来为该存储库创建新会话并请求更改。

1. 返回（或打开）GitHub Copilot app。
2. 选择侧边栏顶部的 **+** 启动会话。
3. 为存储库选择 `tailspin-toys`。
4. 在提示框下方选择 **new working tree** 和 **Interactive** 模式。使用以下提示请求更改：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示发送给 Copilot。

Copilot app 会先创建新的工作树，即项目的隔离副本。然后探索项目，找到需要更新的文件并编写必要的代码。现在，你已使用 Copilot app 添加新功能。

## 审查差异

所有 AI 生成的更改在合并前都应审查，即使改动很小也不例外。现在在 Copilot app 中查看更改。

1. 在应用右上角选择 **Toggle review panel**。在打开的面板中选择 **Changes**，查看更改的文件和差异。

    ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 应能看到 `GameCard.astro` 中添加了代码，这是显示游戏详情的核心文件。内容应类似以下代码：如果评分存在就显示评分，如果 `starRating` 为 `null`，则显示 "No rating yet"：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性而非确定性，因此代码可能与示例不同，但应大致相似。

## 检查更改

打开浏览器前，先审查智能体自动检查的结果。确认测试覆盖数值类型的 `starRating` 和 `null` 回退状态。缺少先决条件或跳过检查不算通过；批准安装请求前请先审查。

不能只看代码就认定它有效。让 Copilot 打开网站，在浏览器画布中检查更新后的 UI。

> [!TIP]
> 画布是 Copilot app 内的交互式小组件。稍后会探索自定义画布并创建自己的画布；现在先使用内置浏览器画布。

1. 使用以下提示让 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 稍等片刻，应用会启动，Copilot app 内会打开浏览器窗口。
3. 确认有评分的游戏卡片显示五星制评分。
4. 完成后，使用以下提示让 Copilot 停止为此会话启动的开发服务器并关闭浏览器画布：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 打开并合并第一个拉取请求

功能已完成！现在创建拉取请求（PR），将新代码合并到现有代码库。

1. 选择右上角的 **Create PR**。
2. 如果系统提示，请选择 **Sign in with your browser** 并按照提示完成身份验证。
3. 如果 Copilot 说明存储库策略要求先关联议题才能创建 PR，请确认它应遵循该流程。
4. Copilot 会开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查面板中打开 PR。可按需审查。
6. 准备就绪后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并 PR。

## 总结和下一步

恭喜！你使用 GitHub Copilot app 交付了第一个更改。本课中，你：

- 启动了智能体会话并了解了会话结构。
- 引导智能体对游戏卡片进行一项范围明确的小改动。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认星级评分。
- 打开 PR 1、审查检查结果并明确执行合并。

接下来，从筛选议题开始，使用 Plan 和 Autopilot 模式[构建更大的功能][next-lesson]。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#安装并配置-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
