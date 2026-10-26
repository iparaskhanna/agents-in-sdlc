---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，应确保它能够获取这些上下文。[指令文件][instruction-files]可提供指导，让 Copilot 不仅了解你要做**什么**，也了解你希望**如何**完成。

本练习将学习如何：

- 探索项目专属上下文、编码指南和文档规范如何通过存储库自定义指令及按路径限定的指令文件传递给 Copilot。
- 向 `.github/copilot-instructions.md` 添加新的全局规范。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不使用前后对比提示。Copilot 云智能体通过 GitHub 议题异步工作，因此难以实时并排展示效果。稍后审查云智能体创建的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发规范和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 应以 TSDoc 文档注释的形式为代码添加文档。
- 每个文件开头都应有一段注释，说明文件的用途。

使用指令文件为 Copilot 提供正确的信息，使其按照这些规范执行任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好，帮助它更好地理解编码风格和需求。你可以指定编码约定、库以及注释偏好，也可以为整个存储库或特定文件类型的任务创建指令。

指令文件有两类：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送的单个指令文件，应包含与大多数聊天或 CLI 请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定语言（例如 TypeScript）或任务（例如创建 UI 组件或新的单元测试）提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时会使用存储库级指令文件以及带 `applyTo` 前置元数据的 `*.instructions.md` 文件。
>
> Copilot 还[支持使用其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

全面讨论如何创建指令文件超出了本研讨会的范围，但示例项目展示了有代表性的做法：

- 将 `copilot-instructions.md` 限定为项目级指导，例如项目简介、结构和全局编码规范。
- 使用 `*.instructions.md` 为文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，保持指导清晰，并提供代码示例说明应采用和避免的写法。

指令文件没有唯一的编写方式，AI 的使用方式也是如此。通过试验，找到适合项目和场景的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应拥有完善的指令文件集合。浏览本项目的指令文件时，你会看到针对 [UI 更新][ui-instructions]、[Astro][astro-instructions]等多种任务的文件。
>
> Copilot 也可以帮助生成指令文件。不同界面的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时介绍这些功能。
>
> 需要模板或入门示例？可以探索 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索本项目中的自定义指令文件

花些时间阅读本存储库提供的指令文件：一个核心 `copilot-instructions.md` 和一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。在与 Copilot 交互时，这些内容都会生效。
3. 打开 `.github/instructions` 文件夹浏览文件。这里有 Astro 文件、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob，确定哪些文件适用；例如，任何匹配 `**/*.test.ts` 的 TypeScript 测试文件都会命中。
5. 查看创建单元测试的项目专属指令。
6. 滚动到 `.github/instructions/drizzle.instructions.md` 文件底部。查看它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将大型指令拆分为可复用的小文件，并引用可供 Copilot 遵循的示例。这里的路径相对于指令文件本身，而非存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码规范，但尚未要求添加代码文档。接下来的步骤将添加 TSDoc 文档注释和文件注释头的规则。

## 添加新的存储库规范

接下来只需编辑一次：添加项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后分配议题时，云智能体会采用这项规则。

编辑前，先创建并切换到新分支（云智能体会读取议题目标分支上的指令，但仍建议清晰地提交更改）：

1. 在 Codespace 终端中运行：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码规范，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常靠近第 27 行。注意其中记录了项目编码规范，但尚未说明代码文档要求。
3. 在现有规范下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，指令文件可以放在存储库级别的 `.github/copilot-instructions.md` 中以提供全局指导，也可以使用 `*.instructions.md` 文件针对特定语言、文件类型或任务提供指导。刚添加的项目级文档注释规范适合放在存储库级指令文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。下一练习将从 `main` 分支创建云智能体分支，因此这些指令更改必须先进入 `main`。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，云智能体会读取这些指令。

> [!TIP]
> 如果希望本研讨会直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。创建分支是为了模拟真实项目中的处理方式。

## 总结和下一步

你探索了 Copilot 如何从指令文件中获取上下文，并向 `.github/copilot-instructions.md` 添加了新的全局规范。后续练习中可以在云智能体创建的拉取请求中看到这项规范的效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
