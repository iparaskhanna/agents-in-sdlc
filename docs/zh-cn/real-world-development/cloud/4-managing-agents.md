---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot 云智能体处理了三项任务，以改善用户体验并添加功能。云智能体以异步、自主的方式工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有有未关闭拉取请求（PR）的智能体任务、了解已执行的操作，还可以引导正在进行的会话。

本练习将学习如何：

- 使用智能体页面监控云智能体任务。
- 引导正在进行的会话，要求添加更多功能。

## 场景

将创建高对比度模式的任务交给智能体后，团队决定同时添加浅色模式。既然已经在更新网站样式并添加切换功能，加入浅色模式也很合理。你将引导智能体，让它在实现高对比度模式的同时添加浅色模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的任务状态。

1. 前往 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成任务所需的各种步骤。

> [!NOTE]
> 显示的具体步骤会因 Copilot 的工作状态和采用的方法而异。

4. 同时查看右侧显示的 PR 面板，了解 PR 和已更改文件，以便进一步监控。

## 引导云智能体

现在，让 Copilot 在实现高对比度模式时同时添加浅色模式。

1. 选择与高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到设置完成并开始执行任务。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务屏幕截图，其中突出显示 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其加入工作流程。

## 让 Copilot 继续工作

和之前一样，Copilot 会继续处理更新后的任务。它会先完成发送消息时正在执行的步骤，然后将新请求加入工作流程。

这仍需要几分钟，可以利用这段时间回顾目前学习的内容。

## 总结和下一步

本练习探索了 Copilot agents 页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。可以在此跟踪所有进行中和已完成的任务、审查正在执行的工作，并重定向进行中的任务以调整范围或提供补充指导。

本练习中，你：

- 使用控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，要求添加更多功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
