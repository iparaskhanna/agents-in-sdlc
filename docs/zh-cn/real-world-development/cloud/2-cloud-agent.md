---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

许多组织都面临技术债务，例如未解决的问题、需要更新的旧代码，或因缺乏时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这既能委托繁琐任务、加快开发流程，也能让开发者专注于更重要的工作。

本练习将探索如何：

- 自定义代码生成环境。
- 确保操作安全。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 希望处理一些技术债务。最初负责创建网站的承包商没有留下合适的文档，实际上完全没有文档。第一步是为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改善游戏发现体验。他们希望在游戏详情页显示相关游戏，即同一类别中的其他游戏，以便用户继续浏览。暂时不需要完善的实现，只要能用于验收测试和 UX 探索即可。虽然这项工作目前受阻，但还有优先级更高的议题。

这些都是容易被搁置的任务，适合分配给 Copilot 云智能体异步处理。开发者可以先处理其他工作，稍后再审查 Copilot 的实现。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。与协作开发类似，可以通过多种方式启动任务，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。你可以在会话进行中或完成后查看它。准备好审查建议方案时，它会在 PR 中提及你。

## 明确指令的范围

GitHub Copilot 没有魔法，不能免去思考。即使看似简单的操作，深入了解后也会发现复杂之处，因此应注意[为 Copilot 云智能体设定合适的任务范围][cloud-agent-best-practices]。把它视为 AI 结对编程助手：分阶段工作、学习、试验并持续调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

之前的练习介绍了自定义指令及其如何引导 Copilot。如果已完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的效果；否则，现在可以快速查看。在分配工作前，先只读查看此存储库提供的指令文件，以便稍后观察它们的影响。

在存储库的 GitHub 网页界面或已启动的 Codespace 中打开：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` 前置元数据，它将指令限定为 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，请留意生成的 PR 是否遵循 TSDoc、TypeScript 约定和文件注释头；这些要求来自上述指令文件。稍后的课程审查 PR 时还会再次检查。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和初始化脚本。Copilot 的工作方式类似软件工程师，因此分配任务时也需要准备环境。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以通过在 `.github/workflows/copilot-setup-steps.yml` 中创建[专用设置工作流][setup-workflow]来定制环境，使其在开始工作前运行并安装所需工具和依赖项。实验室已预先配置此工作流，确保 Copilot 可使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的“Actions”选项卡手动测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为这些步骤设置所需的最低权限。Copilot 执行操作时会使用自己的令牌。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果设置步骤不克隆存储库，完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流文件类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理 PR 之前于 GitHub Actions 中运行。
- 注意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这便于先验证工作流是否成功，而不必等待 Copilot 启动它。

## 添加文档

大多数项目要么文档过时，要么完全缺少文档。这类技术债务往往无人处理，会降低效率，并增加维护代码库或引入新成员的难度。Copilot 擅长生成文档，因此这是适合分配给云智能体的议题。它会在后台生成所需文档，后续练习再审查结果。

1. 在新的浏览器选项卡中前往 GitHub 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。
10. 选择 **Pull Requests** 选项卡。
11. 打开新建的 PR，标题可能类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 生成待办事项可能需要几分钟，因为它正在创建环境（运行前面介绍的工作流）、分析项目并确定处理方式。

13. 审查待办事项列表和 Copilot 将要完成的任务。
14. 在 PR 时间线中向下滚动，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 查看实时会话，观察 Copilot 如何探索代码并了解当前状态、暂停思考并决定合适方案，以及编写代码。

这可能需要几分钟。云智能体的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来利用这一点，再分配一个任务给云智能体，同时着手为应用编写功能。

## 为游戏详情页添加相关游戏区域

GitHub Copilot 云智能体的优势之一是可以拆分工作：你处理一组任务，智能体处理另一组。为设计团队添加相关游戏区域也许不需要很久，但这些时间仍可用于其他工作。现在把任务分配给云智能体。

1. 返回 GitHub 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

不久后，你会在议题的第一条评论中看到 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在处理第二项请求。云智能体的工作方式类似软件工程师，因此无需持续监控；等任务完成后再审查即可。接下来了解如何创建和使用自定义智能体。

## 总结和下一步

本课探索了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给 Copilot，让它异步处理技术债务、创建新功能，或帮助将代码从一个框架迁移到另一个框架。

本课中，你：

- 自定义了代码生成环境。
- 了解了如何确保操作安全。
- 了解了明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建并使用自定义智能体。云智能体也可以使用 [MCP 服务器][cloud-agent-mcp]，并能访问前面练习中介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
