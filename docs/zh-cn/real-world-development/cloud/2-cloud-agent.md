---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务，例如尚未解决的安全问题、需要更新的遗留代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成工作后，它会创建一个草稿 PR，供开发人员审查。这样既能委派繁琐的任务、加快开发进度，也能让开发人员专注于更重要的工作。

本练习将使用 Copilot 云智能体探索以下内容：

- 自定义代码生成环境；
- 确保操作安全执行；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债务。最初受雇开发网站的承包商没有留下完善的文档——事实上，网站完全缺少文档。首先，团队希望为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队准备改善游戏发现体验。他们希望每个游戏详情页都能显示相关游戏（即同类别的其他游戏），方便用户继续浏览。当前不需要完善的最终实现，只需可供用户体验验收测试的版本。虽然这项工作目前受阻，但还有优先级更高的议题需要先处理。

这类工作很容易被降低优先级，因此很适合交给 Copilot 云智能体异步处理。开发人员可以继续处理其他任务，之后再回来审查 Copilot 的工作并确认结果符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。与人类开发者协作类似，可以通过多种方式分配任务，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始处理任务。会话进行中或完成后，都可以查看 Copilot 的工作。解决方案准备好供你审查时，它会在拉取请求中提及你。

## 明确界定说明范围的重要性

GitHub Copilot 没有魔法——不能跳过思考。即使看似简单的操作，深入分析后也可能很复杂，因此[请谨慎界定分配给 Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对编程伙伴：分阶段工作、持续学习、试验并逐步调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何引导 Copilot。如果已经完成自定义说明练习，就见过 `.github/copilot-instructions.md` 的作用；否则现在可以快速查看。在将任务分配给 Copilot 云智能体前，只读浏览此存储库中已有的说明文件，之后便能观察它们的影响。

在存储库的 GitHub Web UI 中打开以下文件；如果已有正在运行的 Codespace，也可以在那里查看：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：注意 `applyTo` frontmatter，它会将这些说明限定为适用于 `**/*.test.ts` 文件。

下一节将把 `Code lacks documentation` 议题分配给云智能体。留意生成的拉取请求中是否有 TSDoc 文档注释、TypeScript 约定和文件注释头——这些要求来自上述说明文件。之后的练习会再次介绍如何审查 PR。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保环境状态正常。将任务分配给 Copilot 也一样，因为它执行任务的方式类似软件工程师。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。可以通过创建[专用设置工作流][setup-workflow]来自定义环境，并将其配置在 `.github/workflows/copilot-setup-steps.yml` 中，让工作流在智能体开始工作前运行。这样可以提供所需的开发工具和依赖项。本实验已预先配置该工作流，便于学习流程并完成练习。它会确保 Copilot 获得 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及迁移并填充数据的 SQLite 数据库（供单体 Astro 应用使用）：

```yaml
name: "Copilot Setup Steps"

# 允许在存储库的“Actions”选项卡中测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为步骤所需的最低级别。Copilot 会获得自己的令牌来执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 contents: read 权限。如果设置步骤不克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用其中的数据。
      - name: Set up the database
        run: npm run db:setup
```

这看起来与其他 GitHub 工作流文件类似，但有几点值得注意：

- 其中只包含一个名为 `copilot-setup-steps` 的作业。该作业会在 Copilot 开始处理拉取请求前于 GitHub Actions 中运行。
- 注意 `workflow_dispatch` 触发器，它允许你在存储库的 **Actions** 选项卡中手动运行工作流。这样可以在等待 Copilot 运行前先验证工作流。

## 添加文档

大家都知道文档的重要性，但许多项目的文档已经过时或完全缺失。这类技术债务常常无人处理，降低生产效率，也让维护代码库或引入新团队成员变得困难。Copilot 擅长编写文档，因此可以将这项工作分配给云智能体，让它在后台生成所需文档。后续练习会回来审查结果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot** 打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），标题类似 `[WIP]: Code lacks documentation`。如果列表中尚未出现新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 创建的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定最佳处理方案。

13. 查看列表和即将执行的任务。
14. 向下滚动拉取请求时间线，应该会看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 查看实时会话，观察 Copilot 如何解决问题，包括探索代码、了解状态、停下来思考并确定适当计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们有时间处理其他工作。现在利用这一优势，再分配一项任务给云智能体，然后着手编写应用功能代码。

## 在游戏详情页中添加相关游戏部分

如前所述，GitHub Copilot 云智能体的一大优势是可以拆分工作：你专注于一组任务，它则处理另一组任务。为设计团队添加相关游戏部分可能并不复杂，但也可以利用这段时间处理其他工作。现在把它分配给 Copilot 云智能体！

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为：`Show related games on the game details page`
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

很快就会看到议题中的第一条评论出现一组 👀，表示 Copilot 正在处理任务！

![Copilot 使用眼睛表情表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正在认真处理第二项请求！Copilot 云智能体的工作方式类似软件工程师，因此无需一直监控；完成后再审查即可。接下来了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理。它可以用于解决技术债务、开发新功能，或协助将代码从一个框架迁移到另一个框架。

本课介绍了以下概念：

- 自定义代码生成环境；
- 确保操作安全执行；
- 明确界定议题范围的重要性；
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot 云智能体还可以使用 MCP 服务器][cloud-agent-mcp]，并能读取自定义说明；前面的模块已介绍过这些内容。

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
