---
title: "练习 8 - 创建并合并功能 PR"
description: "一起审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一个分支上。现在可以一起审查这些更改并创建拉取请求。之前已亲自合并星级评分拉取请求（PR）；这一次将让 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分为几个独立的 PR。为了精简本工作坊，筛选功能和质量工作流都保存在同一会话和分支中，并通过此 PR 一起提交。

在本练习中，将：

- 了解 Agent Merge 及其如何自动管理合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 审查完成后启用 Agent Merge，并确认 PR 已合并。

## 场景

在整个筛选工作流中，已使用 Copilot 规划、实现并验证功能。Tailspin Toys 现在希望自动处理剩余的 PR 工作，同时由开发者控制合并授权。

## 认识 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时进行变基；如果存储库允许，随后会启用 GitHub 自动合并。

到目前为止，都是自行选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用前，先审查更改并确认可以合并。

## 使用 Agent Merge 管理 PR

所有代码都已生成，现在一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监视 PR 的持续集成流程。所有检查通过后，它会执行合并。
7. 确认收到类似“PR #14 was squash-merged successfully.”的 Copilot 消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果操作受阻，请阅读报告的原因，并在存储库允许时手动完成已审查的合并。

## 总结和后续步骤

已经自动化了开发流程中的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。在本练习中：

- 了解了 Agent Merge 及其如何自动管理合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

下一步，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云端委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
