---
title: "练习 3 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题实现筛选功能，并在 Interactive 模式中审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

刚才已经为项目添加了一项小功能。较大的更改则需要更完善的流程。GitHub Copilot CLI 支持融入组织现有工作流，帮助团队以正确的方式构建所需功能。本练习将开启一系列课程，介绍典型的智能体驱动开发流程：从议题着手创建新功能，确认代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 继续完成此功能工作流时，将使用同一会话和分支。通常，不同文件类型的工作会分别使用不同分支或 PR；本工作坊为聚焦核心概念，简化了这一流程。

在本练习中，将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发布商筛选游戏。待办事项中的议题描述了这个功能，但筛选类别的组合方式等细节需要在编码前确定。将使用 Plan 模式敲定这些决定，然后授权 Autopilot 在明确范围内实现功能。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则；事实上，这些原则会变得更加重要！大多数开发者遵循的流程大致如下：

1. 从已提交的议题开始，明确需要完成的工作。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这一标准流程，可以确保 AI 生成的代码符合要求，并接受与手写代码相同的审查。

## 对话模式

**对话模式**决定智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在不同模式间切换：

- **Interactive**：与智能体协作。智能体提出更改建议，并等待输入后再继续。
- **Plan**：智能体先制定计划，在此模式下不能编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试并反复调整，直到认为任务完成。

先在 Plan 模式中制定并审查计划，再使用 Autopilot 实现。

## 从议题开始

开始筛选功能开发前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果 Codespace 已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main` 分支，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键突出显示标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

注意，提示现在以 `#7`（或类似编号）开头。使用 `#` 可以将 GitHub 上的议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划有助于在将功能或任务交给 Copilot 之前，先明确实现方案。处理任何复杂工作时，花些时间规划通常都是好做法。下面切换到 Plan 模式，请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划！它会先探索项目，再确定最佳方案。

3. Copilot 可能会询问筛选功能应如何工作。根据偏好回答即可，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动以审查计划。
6. 如果计划中的任何部分与已确定的方案不符，请让 Copilot 修改。

## 批准 Autopilot

计划已经完成并审查，现在开始实现。使用 Autopilot 让 Copilot 执行工作。

Autopilot 会让 Copilot 持续处理问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或安装版本中名称相近的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何根据计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再判断检查是否完成。

## 审查并验证实现

代码生成后，与其他代码一样，合并前必须审查。现在一起审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选功能实现和测试。
3. 将结果与议题以及规划时作出的决定进行比较。
4. 审查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并让 Copilot 修复失败项。

## 体验新功能

代码看起来不错，但能否正常运行？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用后，选择 **Open in Browser**。
3. 尝试按类别筛选、按发布商筛选，以及按计划确定的筛选组合。
4. 确认重置和无结果状态符合议题要求和已作出的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

已经使用不同的对话模式构建并审查功能。在本练习中，将：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来，[使用自定义说明][next-lesson]，进一步了解如何让代码遵循已记录的开发实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
