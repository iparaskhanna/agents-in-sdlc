---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot 中的[自定义智能体][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。你可以在存储库的 `.github/agents` 文件夹中通过 Markdown 文件定义智能体，为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，让它更有效地完成特定类型的工作。团队可以将专业知识编写为可复用的智能体，例如执行 [WCAG][wcag] 规范的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局定义在 `~/.copilot/agents`。每个文件至少包含 `name` 和 `description` YAML 前置元数据，后续 Markdown 提示则定义智能体的行为、专业知识和指令。

### 比较自定义智能体与智能体技能

自定义智能体和[智能体技能][agent-skills-concept]有一些相似之处：两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义智能体**是工作者，**技能**是工具。

自定义智能体拥有自己的上下文窗口，执行工作时可以编排技能（甚至其他智能体）。本实验中的无障碍自定义智能体会根据无障碍指南审查并更新网站；执行过程中，它可以调用拉取请求工作流或测试运行管理等技能。

> [!NOTE]
> 自定义智能体没有唯一正确的编写方式。和其他 AI 用法一样，应在目标环境和场景中进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本练习将学习如何：

- 定义自定义智能体。
- 将任务分配给自定义智能体。

## 场景

Tailspin Toys 致力于让所有用户都能使用众筹平台，不论其视觉能力或偏好如何。近期用户反馈指出，当前深色主题的文字与背景对比度不足，难以阅读。为解决这一无障碍问题，设计团队要求添加可切换的高对比度模式。

无障碍功能至关重要，因此需要尽快实现。你将使用自定义智能体生成此功能。

## 查看无障碍自定义智能体

已为你创建了无障碍自定义智能体。先查看其内容，了解它将如何引导 Copilot。

返回 Codespace，然后查看无障碍智能体文件：

1. 打开 `.github/agents/accessibility.md`。
2. 注意包含 `name` 和 `description` 字段的 YAML 前置元数据。

> [!CAUTION]
> 自定义智能体必须包含带 `name` 和 `description` 的前置元数据。

3. 浏览后续部分，包括：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 示例。
   - 常见陷阱和错误。

## 创建并分配议题

环境中的智能体控制中心是管理智能体的主要位置。可以将任务分配给 Copilot 云智能体、监控任务，并引导正在进行的会话。现在，将创建高对比度模式的任务分配给 Copilot。

1. 前往存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。
9. 从列表中选择 **Accessibility agent**。

    ![云智能体分配界面的屏幕截图，其中突出显示自定义智能体和无障碍智能体](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和下一步

本练习探索了 GitHub Copilot 的[自定义智能体][custom-agents]，即针对特定任务和领域定制的 AI 助手。通过自定义智能体，可以将团队专业知识和标准编写为可复用指导，帮助 Copilot 更有效地完成特定类型的工作。

本练习中，你：

- 了解了自定义智能体的定义方式。
- 将任务分配给自定义智能体。

Copilot 正在实现高对比度模式。接下来，在控制中心[监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [为组织使用自定义智能体做准备][org-custom-agents]
- [为企业使用自定义智能体做准备][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
