---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并运行测试。开发者往往要查找所有相关文件、完成更改、运行测试、调试、找出遗漏的文件，再继续修改。GitHub Copilot 智能体模式可以帮助处理这类工作。

智能体模式可以在 IDE 中更自主地工作，类似开发者：先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。下面使用智能体模式为网站添加功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但用途不同。智能体模式在 IDE 中工作，便于快速反馈和交互；云智能体则异步工作，像团队成员一样通过议题和拉取请求与你协作。

本练习将学习如何：

- 让 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 使用智能体模式跨 UI 和数据层实现功能。
- 在合并前审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断扩大，你希望用户可以按类别和发行商筛选。上一个练习已经添加发行商辅助函数，现在将使用 Copilot 智能体模式完成其余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

进行更改前，先探索网站，了解现有功能。Tailspin Toys 是一个以开发者为主题的桌游众筹平台，用户可以浏览游戏列表和详情。网站是一个 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在前置元数据中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`；交互功能使用标准 DOM API 编写的小型、作用域明确的 Astro `<script>`。

### 启动网站

`npm run dev` 任务会启动唯一的 Astro 开发服务器。按以下步骤在 GitHub Codespace 中运行：

1. 返回 Codespace。上一个练习结束后应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后会显示类似以下信息：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否在浏览器中打开 `http://localhost:4321`，选择 **x** 关闭对话框。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 Codespace 时，选择终端中的 localhost 链接会自动跳转到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，网站服务器正在其中运行。

### 探索网站

网站运行后，可以查看其主要功能：

- **主页**：显示桌游列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会显示详情，包括标题、描述、发行商和类别。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。每个练习所用的模式和模型可能不同；相应步骤会说明具体设置。

网站已有可用的初始实现，但我们希望添加更多功能。先查看待办事项。使用模板创建项目时，系统已自动创建一组议题；现在让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。该智能体可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的智能体选择器屏幕截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好任何特定模型。编写本研讨会时使用的是 Claude Sonnet 4.5，因此将其列为步骤，以尽可能保持体验一致。但大型语言模型具有概率性，你看到的建议可能与示例不同，这是正常且预期的。

> [!NOTE]
> 由于模型输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审查生成的议题列表。

Copilot 还会根据对用户的价值为议题排序。

## 查看指令文件

启动智能体生成代码前，先查看将用于提供上下文的指令文件。本练习将使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，了解如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向特定文件类型指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置。这样可以复用内容，将复杂指导拆分成更易管理的小块，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要三项应用更改：

- 在数据层（`src/lib/`）添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，添加筛选 UI。

合并前还必须运行并通过测试。Copilot 智能体模式可以完成这些任务。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 指令文件。

   ![选择 UI 指令文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表仍选择 **Agent**。
4. 确认模型仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示，让 Copilot 根据待办议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目、查找相关数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。工作过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo**，无需在工作过程中操作。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中询问是否允许运行测试的对话框屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至暂时出现测试失败，这没关系。Copilot 会在代码生成和测试之间反复迭代，直到任务完成且未发现错误。

   ![Copilot 智能体模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，发现潜在问题。

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，试用新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和下一步

恭喜！本练习中，你使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加了新功能，并了解了如何：

- 使用智能体模式跨 UI 和数据层实现功能。
- 让 Copilot 智能体模式探索项目、识别相关文件并协调更改。
- 在合并前审查智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]并创建拉取请求。

### 附加探索练习 — 实现分页

随着游戏列表不断扩大，需要支持分页。使用本练习中学到的知识，提示 Copilot 为网站添加分页。考虑：

- 遵循现有最佳实践，包括使用现有指令文件。
- 确定分页方式，例如是否允许用户选择每页数量，或使用固定数量。
- 编写提示时提供必要指导，确保 Copilot 按预期实现。
- 可能需要与 GitHub Copilot 迭代、多次请求更改并补充上下文，这是正常的协作流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及使用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
