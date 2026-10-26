---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，应确保它能获取这些内容。[指令文件][instruction-files]是提供代码生成指导的重要方式。

本练习将学习如何：

- 探索项目专属上下文、编码指南和文档规范如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 使用当前指令生成筛选功能所需的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的全局规范。
- 使用同一提示重新生成代码，观察新规范的影响。
- 提交指令更新和筛选切片到 `main`，以便 Copilot 在下一练习中使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全遵循设定的规范。Copilot 的输出具有概率性。本练习的目的是观察更新指令后行为的总体变化趋势，而不是让输出逐字匹配示例。

## 指令文件

### 场景

Tailspin Toys 有一套开发规范和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

使用指令文件为 Copilot 提供正确的信息，使其按照这些规范完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。可以指定编码约定、库和注释偏好，也可以为整个存储库或特定文件类型创建指令。

指令文件有两类：

- `.github/copilot-instructions.md`：每次向 Copilot 提交此存储库请求时都会发送的文件，应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：可为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。Copilot Chat、Copilot CLI 和云智能体会在生成代码时使用存储库级指令，以及带有 `applyTo` 前置元数据的 `*.instructions.md` 文件。Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目简介、结构和全局编码规范。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明应采用和避免的写法。

指令文件没有唯一正确的编写方式。通过试验，找到适合项目和场景的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应拥有完善的指令文件。项目中有许多任务类型的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。Copilot 也能帮助生成指令文件；不同界面的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。还可以从 [awesome-copilot][awesome-copilot] 获取模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索项目中的自定义指令文件

阅读存储库提供的核心 `copilot-instructions.md` 和各类 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介和指导，例如 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure**。注意 **Code standards** 中嵌套的 **GitHub Actions Workflows** 指导。这些内容适用于你与 Copilot 的所有交互。
3. 浏览 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。该 glob 相对于存储库根目录，例如匹配任何符合 `**/*.test.ts` 的 TypeScript 测试文件。
5. 查看创建单元测试的项目专属指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾，查看其指向其他指令文件（例如 `unit-tests.instructions.md`）和现有项目文件的链接。路径相对于指令文件本身，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

遵循最佳实践，为更改创建分支。

1. 返回上一个练习中的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前，先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。每个练习所用的模式和模型可能不同；相应步骤会说明具体设置。

先使用当前指令文件发送提示，然后更新指令并重新发送相同提示，观察差异。

1. 关闭之前练习中打开的编辑器选项卡，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的智能体下拉列表中选择 **Agent**，以便 Copilot 应用文件更改。

   ![Chat 视图中的智能体选择器屏幕截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意建议的辅助函数是一个类型化函数，它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的输出具有概率性，因此它可能会在未明确要求时添加文档注释。这也没关系；本练习重点在于更新指令后行为的一致性提升。

## 添加新的存储库规范

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码规范，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（通常靠近第 27 行）。其中记录了项目编码规范，但尚无代码文档要求。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 项目级 `.github/copilot-instructions.md` 适合存放全局指导；`*.instructions.md` 文件适合特定语言、文件类型或任务。刚添加的文档注释规范属于项目级标准。

## 重新发送提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat** 清空对话上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦正确的文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，现在建议的文件开头会出现类似以下注释：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 建议的函数现在也会包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新规范，并将其应用到后续练习会继续使用的真实代码中。

## 提交指令并推送分支

指令文件与存储库中的其他资源一样，使用相同的源代码管理流程。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认指令更新和辅助函数更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新和 Copilot 为辅助函数创建的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也将这些文件加入暂存区。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并关联到待办事项中的文档规范议题。也可以让 Copilot 使用已连接到项目的 GitHub 工具查找议题、创建关联 PR 并合并。

> [!NOTE]
> 这是第一个让 Copilot 代表你操作 GitHub 的练习。项目模板已在工作区中配置 GitHub 工具，无需额外设置。首次调用工具时，VS Code 可能会提示登录 GitHub；按提示授权。稍后的练习会介绍这种连接方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保在智能体选择器中选中 **Agent**。
3. 让 Copilot 查找更新指令文件的议题，并从当前分支创建 PR，说明指令更新和新的发行商辅助函数基础：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 当 Copilot 请求允许代表你执行 GitHub 操作时，审查命令并酌情选择 **Allow**。
6. PR 创建后，让 Copilot 合并 PR 并返回 `main` 分支：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 请求执行 GitHub 操作和 shell 命令时，审查命令并酌情选择 **Allow**。

现在，你已在 GitHub Copilot 的帮助下创建并合并拉取请求。

## 总结和下一步

你探索了 Copilot 如何从指令文件中获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 在**现有**指令下生成的内容。
- 在 `.github/copilot-instructions.md` 中添加新的全局规范。
- 重新发送相同提示，观察建议的代码如何遵循新规范。
- 将指令更新和辅助函数基础提交到 `main`，以便下一练习继续使用。

接下来，使用[智能体模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
