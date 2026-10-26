---
title: "第 1 课 - 安装 GitHub Copilot app"
description: "安装 GitHub Copilot app，连接通过模板创建的存储库，熟悉工作区并尝试快速聊天。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

**[GitHub Copilot app][about-copilot-app]** 是一款用于智能体驱动开发的桌面应用。它基于 GitHub Copilot CLI 构建，并与 GitHub 原生集成，因此存储库、分支和 CI 管道均可直接使用。它适用于同时指挥多个智能体的工作流：每个智能体都在隔离的工作区中运行。你无需手动完成所有工作，还可以自动执行重复性任务。安装 Node.js 并准备好项目副本后，下一步是安装应用并连接该存储库。

本课将学习如何：

- 安装 GitHub Copilot app 并登录。
- 从 GitHub 存储库将项目添加到应用。
- 熟悉工作区，包括模板创建的待办事项。
- 通过快速聊天了解应用本身。

## 场景

团队正在采用 AI 智能体来处理不断增加的待办事项。Copilot app 提供了统一的工作位置，可领取议题、运行智能体、审查更改并合并拉取请求。本课将帮助你完成安装和连接，并熟悉如何发起有关项目的对话。

> [!NOTE]
> 需要符合条件的 Copilot 计划：Copilot Free、Copilot Student 或任何付费计划（Pro、Pro+、Business 或 Enterprise）。如果使用 Copilot Business 或 Copilot Enterprise，管理员必须保持 **GitHub Copilot app** 策略启用。

## 安装并配置 GitHub Copilot app

GitHub Copilot app 支持 Windows、macOS 和 Linux。接下来安装应用、完成身份验证，并将 Tailspin Toys 存储库添加到应用。

1. 在浏览器中打开 [GitHub Copilot app 产品页面][download-app]。
2. 下载适用于当前平台的应用，并按照产品页面上的说明进行安装。
3. 安装完成后打开应用。
4. 选择 **Sign in to GitHub**，并按照提示完成身份验证。如果使用具有数据驻留功能的 GitHub Enterprise Cloud，请选择 **Use GitHub Enterprise**，并在提示时输入 `*.ghe.com` 主机名。GitHub Enterprise Server 不受支持。
5. 如果加入流程中出现存储库选择界面，请选择通过模板创建的 Tailspin Toys 存储库。也可以跳过此步骤，在完成加入流程后再添加项目。
6. 系统提示选择主题时，选择主题并完成加入流程。
7. 在侧边栏的 **Sessions** 中查找存储库。如果已列出，则无需再次添加。
8. 如果尚未连接存储库，请选择 **Sessions** 旁的 **+** 创建新会话，然后选择 **Add GitHub repository**。使用先决条件课程中记录的所有者名称和存储库名称。
9. 确认存储库显示在侧边栏中。在此处使用的布局中，`tailspin-toys` 等存储库文件夹位于 **Sessions** 下方，每个存储库下方显示单独的会话。

## 熟悉工作区

连接项目后，了解工作区的布局。应用将功能组织在侧边栏的几个区域中：

- **New**：启动与 Copilot 的新聊天会话。
- **Pull requests**：通过应用与 GitHub 的原生集成查看拉取请求。在这里，无需离开应用即可浏览和筛选 PR、检查 CI 状态并审查 PR。
- **Issues**：以同样的方式查看议题。可以浏览和筛选议题，并从议题启动会话。
- **Automations**：按计划或按需运行的已保存智能体任务。可用于管理待办事项、定期项目维护或其他重复工作。总结课程会将其作为后续方向提供链接，而不是增加一项研讨会练习。
- **Customize**：通过 MCP 服务器、插件、技能和其他组件为 Copilot app 添加功能。稍后会使用它配置 Playwright MCP。
- **Chats**：适合提问和集思广益的轻量对话，无需单独创建分支或工作区。开始第一次聊天后，此区域才会显示。如果尚未显示，请选择 **Sessions** 旁的 **+** 开始第一次聊天。
- **Sessions**：按存储库文件夹分组显示智能体会话。选择 `tailspin-toys` 等存储库名称可打开存储库视图，选择存储库下方的会话则可返回相应对话。每个会话都在独立工作区中运行，因此可以同时运行多个会话，而更改不会相互冲突。添加星级评分时将启动第一个会话。

完成研讨会时，你会逐步探索这些区域。

> [!TIP]
> 不确定如何操作或某项功能是否可用时，可以直接询问 Copilot。

### 查找模板创建的待办事项

几乎每个项目都有待办事项，Tailspin Toys 也不例外。下面查看通过模板创建项目时生成的待办事项。

1. 在侧边栏的 **Sessions** 下，选择 Tailspin Toys 存储库文件夹，而不是其下的某个会话。然后在存储库顶部导航中选择 **Issues**。
2. 按标题查找以下议题，不要假设议题编号：

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. 选择议题阅读详情。每个议题都可以启动智能体会话。完成一项快速的首次更改后，你将从筛选功能议题启动会话。

> [!NOTE]
> **Issues** 和 **Pull requests** 中的列表会自动筛选，只显示已添加到 Copilot app 的存储库中的项目。要查看其他存储库中的工作项，请先将相应存储库添加到应用。

## 尝试快速聊天

快速聊天适合用来了解应用本身。无需创建分支或工作树，即可提问或集思广益，因此非常适合快速的一次性问题，无需启动会话。

1. 在侧边栏中选择 **Chats** 旁的 **+** 打开新聊天。如果尚未显示 **Chats**，请先选择 **Sessions** 旁的 **+** 开始第一次聊天；之后会显示 **Chats**。
2. 询问应用自身的会话工作方式：

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. 在对话视图中阅读回复。每个会话都在独立的 git 工作树中运行，因此可以并行运行多个智能体，而不会造成更改冲突。你可以随时继续对话或开始新聊天。

## 总结和下一步

你已安装 GitHub Copilot app、连接项目并探索工作区。本课中，你学习了如何：

- 安装应用并登录 GitHub。
- 从 GitHub 存储库添加项目。
- 熟悉工作区，并在 **Issues** 中找到模板创建的待办事项。
- 使用快速聊天提出一次性问题。

接下来，[启动第一个智能体会话][next-lesson]，并用它在游戏卡片上显示星级评分。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
