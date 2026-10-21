---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者的工作流程通常包括：找出所有相关文件、进行更改、运行测试、调试、找出遗漏的文件、再次更新……工作往往会不断重复。

这正是 Copilot 智能体模式发挥作用的地方。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但面向两种不同的体验。智能体模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

在本练习中，将了解：

- Copilot 智能体模式如何探索项目、识别相关文件并协调进行更改；
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能；
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断扩大，希望用户可以按类别和发行商筛选游戏。上一练习中已经添加了 publishers helper，现在使用 Copilot 智能体模式完成数据层、UI 和测试的其他工作。

## 运行 Tailspin Toys 网站

进行任何更改之前，先探索 Tailspin Toys 网站，了解其当前功能。

该网站是一个面向开发者主题棋盘游戏的众筹平台，用户可以浏览游戏列表和详情页。网站是单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立的后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能则通过使用标准 DOM API 的小型、限定范围的 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤即可在 GitHub Codespace 中运行：

1. 返回 Codespace。完成上一练习后，应该已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，提示打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），同时选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost URL 链接时，它会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有隧道，网站服务器现在就在这里运行！

### 浏览网站

网站运行后，可以探索其功能。主要包括：

- **主页**：显示棋盘游戏列表及其标题、图片和说明。
- **游戏详情页**：选择某个游戏后，会打开显示更多信息的详情页，包括标题、说明、发行商和类别。

## 使用 Copilot 浏览待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 开始全新对话。不同练习使用的模式和模型可能不同；各步骤会在相关位置说明。

网站的初始实现可以正常运行，但还需要添加新功能。先查看待办事项。使用模板创建项目时，系统已自动创建一组议题；请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉列表中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好某个模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此将其列为示例，希望代码建议较为一致，带来良好体验。不过，由于大型语言模型具有概率性，实际收到的建议可能与本工作坊不同。这完全正常，也是预期结果。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 发送以下提示，请 Copilot 查看 GitHub 存储库中的待办事项：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

可以看到，Copilot 还根据它认为对用户最有用的程度，帮助你排定了议题优先级。

## 查看说明文件

启动智能体生成代码前，可以先查看将用于为 Copilot 提供上下文的说明文件。接下来使用 [用户界面（UI）说明文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中介绍了如何为网站添加功能。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中为功能开发提供的整体指导，包括：
   - 架构概览；
   - 组件设计、可测试性和无障碍原则；
   - 指向特定文件类型说明的链接，包括：
     - Astro；
     - Tailwind CSS；
     - Drizzle。

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径是相对于说明文件所在位置的，因此可以复用内容、将复杂说明拆分为较小且易管理的部分，并提供示例和模板。

## 实现筛选功能

要完成筛选功能，至少需要对应用进行三项独立更新：

- 在数据层（`src/lib/`）添加或完善其余筛选逻辑；
- 添加或更新筛选行为测试；
- 更新游戏列表页，加入筛选 UI。

此外，在将更改合并到代码库前，必须运行并通过测试。Copilot 智能体模式可以代为完成这些任务！现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉列表中仍选中 **Agent**。

   ![显示 Chat 视图中智能体选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何开始探索项目并找到与所需功能相关的文件，包括数据层 helper、UI 和测试。随后它会开始修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改旁边都会显示 **Keep** 或 **Undo**，因此无需在它仍工作时选择这些按钮。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中的对话框截图，提示用户确认是否运行测试](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能会看到一些测试失败。这没有问题！Copilot 会反复生成代码并运行测试，直到任务完成且没有发现错误。

   ![完整的 Copilot 智能体模式 Chat 会话屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，留意潜在问题。

> [!CAUTION]
> 请记住，始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行着网站的浏览器，体验新功能！
11. 确认一切正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。我们了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能；
- Copilot 智能体模式如何探索项目、识别相关文件并协调进行更改；
- 合并到代码库之前，如何审查 Copilot 智能体模式生成的更改和测试。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，然后创建该功能的拉取请求。

### 拓展练习——实现分页

随着游戏列表不断扩大，网站需要支持分页。运用本练习中学到的技能，请 Copilot 更新网站以实现分页。可以考虑：

- 遵循现有最佳实践，包括使用现有说明文件；
- 确定分页方式，例如是否允许用户选择每页项目数，还是使用固定值；
- 编写提示时提供必要指导，让 Copilot 按预期实现功能；
- 可能需要与 GitHub Copilot 多轮协作、提出修改请求并提供上下文。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
