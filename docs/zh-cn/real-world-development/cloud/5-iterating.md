---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，已与 GitHub Copilot 一起处理多个改善用户体验和添加功能的任务：请 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在查看代码更改，必要时向 Copilot 提供反馈以改进结果。

### 场景

前面已经多次强调，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终要审查生成的代码，并遵循正常的 DevOps 流程。因此，在为其余团队启用审查前，先检查 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议。

## GitHub Copilot 云智能体与安全

由于 Copilot 云智能体会异步且无人监督地执行任务，因此设置了若干安全限制以确保操作安全：

- Copilot 只能读取存储库，并且**只能**写入用于提交代码的分支。
- 云智能体在 GitHub Actions 中运行，并会创建独立的临时环境来工作。
- GitHub Actions 工作流必须经过人工批准才能运行。
- [默认限制对外部资源的访问](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），它为代码添加了文档。使用 GitHub.com 的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期情况**，因为 Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更具体的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。等待时可以休息一下，或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包括新生成的 TSDoc 文档注释和其他文档。实际更改内容会有所不同。

    浏览更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自练习 2 开始时查看的自定义说明文件。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待审批的提示。
8. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会排入拉取请求的检查列表。如果一切正常，单体 Astro 应用的项目检查会全部通过。这可能需要几分钟。

## 请 GitHub Copilot 修改更改

与 Copilot 协作并不是单向的。可以像提及团队其他成员一样，在拉取请求评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理请求。由于结果具有非确定性，无法规定应该使用的确切措辞。可以请 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头；
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释；
- 创建介绍 Astro 应用结构的 README。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样标记 **@copilot**。可以采用上述想法，也可以提出希望在代码库中看到的其他文档改进。
2. 选择 **View Session**，查看 Copilot 如何完成工作。注意它会启动新会话来执行更新。
3. 可以选择 **Back to pull request** 返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应该会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。确认结果后，可以将 PR 从草稿转换为可审查状态，并合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在返回 Copilot 为解决“在游戏详情页显示相关游戏”议题所创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 或更具体的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待审批的提示。
7. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 工作流应会排入拉取请求的检查列表。如果一切正常，单体 Astro 应用的项目检查会全部通过。这可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。打开 Codespace 并启动终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！和之前一样，可以继续通过 Copilot 云智能体迭代并请求更新。例如，可以让 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配这项工作时**还没有**更新自定义说明）。与之前一样，可以在 **Conversation** 选项卡中添加评论提出请求，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的无障碍功能。此 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 在任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 或更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现时，重点关注：
   - 用于切换模式的 UI 组件；
   - 使用 local storage 保存用户偏好；
   - 高对比度和浅色模式的 CSS 或样式更改；
   - 无障碍属性（ARIA 标签、键盘导航等）；
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审查更新并确认没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待审批的提示。
8. 如果有工作流等待审批，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流应会排入拉取请求的检查列表。如果一切正常，单体 Astro 应用的项目检查会全部通过。这可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。打开 Codespace 并启动终端，然后运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常且设置在页面重新加载后仍然保留。

可以看到，无障碍自定义智能体帮助 Copilot 遵循无障碍最佳实践实现功能。如果发现无障碍问题或改进机会，可以像处理之前的 PR 一样在评论中标记 **@copilot**，请求更新。

## 可选练习——继续委派任务

云智能体最适合处理真实待办事项，让你有时间转而处理其他事情。可以通过以下方式养成这种工作习惯：在存储库中创建更多议题并分配给 Copilot。可考虑以下任务：

- 在游戏详情页创建支持者意向表单；
- 在游戏列表页实现分页；
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

已完成云智能体环境的全部练习。在这些课程中：

- **检查了存储库随附的自定义说明文件**，以便稍后观察这些说明对云智能体输出的影响；
- **将议题分配给 Copilot 云智能体**，并观察它设置环境、规划和异步执行任务；
- **创建并使用无障碍自定义智能体**，添加高对比度和浅色模式切换；
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导正在进行的无障碍会话；
- **审查并迭代云智能体创建的拉取请求**，通过标记 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

云智能体环境的练习已经完成。若想继续探索，其他环境可以补充本次所学内容：

- 🖥️ **[VS Code 环境](../../vscode/)**——直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)**——在终端中通过 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接，交付两项经审查的更改。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体；
- 为其他领域创建自定义智能体；
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
