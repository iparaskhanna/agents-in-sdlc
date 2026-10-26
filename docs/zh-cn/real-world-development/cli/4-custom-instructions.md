---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令、添加文档规范，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。若任务需要特定做法，应确保 Copilot 能获取相关指导。[指令文件][instruction-files]不只说明想要什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将探索 Copilot 使用的指令、添加文档规范，并将其应用于代码。

本课将学习如何：

- 探索存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件以确保遵循编码标准。
- 查看指令文件对代码的影响。

## 场景

Tailspin Toys 为开发实践制定了一系列规范：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应说明。
- 可复用 Astro 组件应说明其 `Props` 接口，相关代码更改时也要更新注释。
- 保留现有格式和 lint 指引。

通过指令文件，确保 Copilot 能按照这些实践完成任务。

## 指令文件

自定义指令用于向 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。可以指定编码约定、库以及注释偏好，也可以为整个存储库或特定文件类型的任务添加指令。

指令文件有两类：

- `.github/copilot-instructions.md`：每次向 Copilot 提交此存储库的请求时都会发送的单个指令文件，应包含大多数聊天或 CLI 请求都适用的项目信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境支持的其他指令格式可能有所不同。使用特定格式前，请查阅[自定义指令支持参考][custom-instructions-support]。

## 探索项目中的自定义指令文件

起始项目中已包含一些指令文件。先查看现有文件，再进行更改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。这些指令适用于 Copilot 在此存储库中的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件，包括 Astro、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它使用 glob 指定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目文件的链接。这样可以将较大的指令集拆分为可复用的小文件，并提供示例供 Copilot 参考。

## 更新指令以符合团队规范

现有文件已经提供了良好基础，但仍缺少对新生成 TypeScript 添加 TSDoc 注释的要求。修改核心的 `copilot-instructions.md` 文件以补充这一要求。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，位置大约在第 35 行。
2. 在该部分最后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存指令后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指令。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭对话。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指引：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff`，阅读更改过的 TypeScript 文件。检查新添加的 TSDoc 注释，并确认其准确说明代码。

## 总结和下一步

你已了解 Copilot CLI 如何从指令文件中获取上下文，并将新规范应用到功能中。本课中，你：

- 探索了存储库指令和按路径限定的指令文件如何传递给智能体。
- 更新指令文件以确保遵循编码标准。
- 查看了指令文件对代码的影响。

接下来，[定制并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
