---
title: "练习 9 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话以及可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多斜杠命令。它们可以访问高级功能、后台信息和其他配置选项。此前已经使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr` 等命令。下面来了解其他实用命令。

在本练习中，将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给云端智能体的可选命令。

## 场景

核心 CLI 工作流已完成。现在了解一些其他功能：管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理较大或较复杂的任务时，可能会遇到模型的最大上下文窗口限制。Copilot CLI 会在需要时自动压缩对话，也可以使用斜杠命令自行查看或压缩上下文。

1. 返回 Codespace。如果尚未启动 Copilot CLI，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前令牌用量，以及上下文如何分配给系统说明、工具、消息和可用空间。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不大。

> [!NOTE]
> 上下文窗口逐渐填满时，Copilot CLI 会自动压缩上下文。想自行选择压缩时机时，可以使用 `/compact`。如果要切换到无关任务并开始全新对话，请使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有所长，开发者也有各自的偏好。Copilot CLI 可以列出可用模型并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队协作和共享经验有助于所有人更好地使用 AI 工具。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看已安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择适合的目标位置，例如使用 `/share file` 导出本地 Markdown 文件。
3. 发送或发布之前先审查导出的内容。会话导出可能包含提示、响应和项目详细信息。

发布链接或 gist 是可选操作。不要发布团队不打算共享的存储库或对话内容。

## 可选：扩展工作或委派任务

核心工作坊已完成。Copilot CLI 还提供了用于处理较大任务的命令：

- `/fleet` 可将独立子任务分配给子智能体并行执行。
- `/worktree` 可为独立任务创建隔离的 Git 工作树。
- `/delegate` 可将任务发送给 Copilot 云智能体；它会异步工作，并可能创建拉取请求。

这些命令是可选的，因为它们可能会创建其他工作树或远程工作。尝试前先创建一个范围明确的新任务，并按常规工作流审查结果。如果想深入了解异步智能体工作，请继续学习[云智能体工作坊][cloud-workshop]。

## 总结和后续步骤

通过 Copilot CLI 斜杠命令，可以进行配置、共享会话，并查看后台运行情况。在本练习中，将：

- 使用 `/context` 和 `/compact` 了解 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解 `/share` 如何导出或共享会话。
- 探索用于并行工作、工作树和委派给云端智能体的可选命令。

Copilot CLI 还有许多其他斜杠命令值得探索！接下来，[回顾所学内容][next-lesson]，并了解后续学习方式。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [Copilot CLI 中的上下文管理][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
