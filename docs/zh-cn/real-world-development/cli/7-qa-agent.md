---
title: "练习 7 - 创建并使用 QA 智能体"
description: "创建一个 QA 自定义智能体，整合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

已经使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到自定义智能体中，并为它定义清晰的 QA 流程。

在本练习中，将：

- 了解自定义智能体如何使用说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题审查其结果。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，以一致的方式审查需求、代码质量、自动化检查、测试覆盖范围和浏览器行为。自定义智能体可以协调 QA 流程并提供可复用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述智能体的用途、说明和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

已使用的自定义配置各有用途。存储库说明用于描述团队标准。`quality-checks` 技能封装可重复的检查。Playwright MCP 提供浏览器工具。QA 配置告诉 Copilot 如何使用这些能力评估需求并报告结果。它不会取代这些功能，也不需要另开一个会话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。该配置会定义 QA 要执行的检查以及必须遵循的边界。

1. 返回 Codespace，并确认筛选功能会话处于 Interactive 模式。
2. 请默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，先审查其配置，确认 Copilot 捕捉到了预期的 QA 工作流和边界。这样可以避免配置不完整或范围过宽的智能体在你只想验证功能时修改代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 查看 frontmatter。`description` 是必需项；`name` 是可选项，但可以作为清晰的显示名称。
3. 阅读配置说明，确认 QA 会从需求开始，遵循存储库说明，运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会报告支持证据，在修改实现代码前先询问，并且不会提交更改或创建拉取请求。
5. 如果生成的配置遗漏任何职责或边界，请让默认智能体先修改，再继续操作。

## 根据议题运行 QA

Copilot CLI 会在会话开始时加载项目智能体。创建配置后，恢复同一个筛选功能会话并选择 QA，以便它可以使用会话中已有的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能不会出现在可用智能体列表中，但它已经存在，以上命令会将其启用。

2. 使用以下提示，请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体会开始工作。
4. 完成后，仔细阅读它提供的报告。

## 总结和后续步骤

已经为工作流添加可复用的专用角色，并审查了其结果。在本练习中：

- 了解了自定义智能体如何使用说明、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置。
- 选择了 QA 智能体，并根据筛选议题审查了其结果。

现在实现、技能更新、QA 配置、测试和验证报告都已准备就绪，可以进行审查。下一步，[汇总这些内容并创建功能 PR，然后使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
