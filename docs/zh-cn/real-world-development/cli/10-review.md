---
title: "练习 10 - 总结和后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义配置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

通过 GitHub Copilot CLI，完成了 Tailspin Toys 的完整工作流。在本工作坊中：

- 准备了 Codespace、安装 Copilot CLI、了解项目并找到预置的筛选功能议题。
- 添加星级评分，在转发的浏览器中审查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选功能议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中构建，并在 Interactive 模式中审查。
- 使用自定义说明引导智能体，然后自定义现有的 `quality-checks` 技能，并用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中检查筛选功能。
- 创建并选择质量保证（QA）自定义智能体，以评估需求、覆盖范围、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于管理上下文、模型、共享以及可选云端委派的斜杠命令。

## 完成的工作

本工作坊包含两个 PR 阶段，每个阶段都从更新后的 `main` 分支创建独立分支：

1. **星级评分：**在游戏卡片上显示已有的 `starRating`，并明确显示尚无评分的状态。
2. **筛选功能和质量工作流：**实现筛选功能、更新说明并将其应用于该功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，都使用同一会话和分支。为精简本工作坊，这些工作合并为一个 PR。

## 不同类型的验证

通过多种方式检查了代码：自动化测试、亲自检查浏览器，以及 Copilot 通过 MCP 探索浏览器。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 智能体在创建 PR 前，将这些结果与需求和测试覆盖范围的审查汇总在一起。

添加的测试应弥补实际存在的缺口；如果 QA 检查后不需要新增测试，也可能是正确结果。授权合并前，应审查代码和证据；更改完成后，应更新受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在不同会话中复用这些自定义设置，并根据团队需求调整。说明用于设置标准，技能用于描述可重复的任务，自定义智能体用于定义专门角色，MCP 服务器用于连接外部工具。应审查实际更改和工具结果，而不只是智能体的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 在构建前思考方案，使用 **Interactive** 持续参与聚焦的更改，并使用 **Autopilot** 处理范围明确的任务。常规编辑可选择响应更快的模型，复杂工作则选择能力更强的模型。

上下文与基础设施同样重要。清楚说明要构建**什么**、**为什么**以及**如何**构建，会显著影响输出。

## 更多探索

核心工作流已经完成。还可以了解以下 CLI 功能：

- 使用 `/review` 请求代码审查智能体分析更改。
- 使用 `/rubber-duck` 讨论问题并获得不同视角。
- 使用 `/fleet` 并行协调独立子任务。
- 使用 `/worktree` 隔离独立任务。
- 使用 `/delegate` 将任务发送给 Copilot 云智能体。

## 后续步骤

熟练使用任何工具的最佳方式就是持续使用它！可以将它用于生产代码、业余项目，或多年来一直想做却没时间开发的小应用。与团队分享经验，也向团队成员学习。别忘了探索文档。

如果想从终端扩展 Tailspin Toys，请继续学习可选的 [Foundry Backer Concierge 系列课程][foundry]。如需比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot 云智能体工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
