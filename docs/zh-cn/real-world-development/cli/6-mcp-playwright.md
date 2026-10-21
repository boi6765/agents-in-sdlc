---
title: "练习 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并通过浏览器探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面已经提到，编写代码不只是写代码。我们还需要处理数据和外部服务，并让 Copilot 可以使用更多自动化工具。MCP 服务器能够扩展 Copilot CLI 的内置功能，为它提供更多工具和服务。

在本练习中，将：

- 了解 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加它。
- 请智能体驱动浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际操作 UI。允许 Copilot 像用户一样使用正在开发的网站，可以进一步自动化更改验证，让我们更有信心确认更新符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 智能体与外部工具和服务通信提供了途径。借助 MCP，AI 智能体可以实时访问外部工具和服务，获取最新信息并代为执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁。每个 MCP 服务器都代表智能体可以访问的一组工具和资源。

以下是几个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器可供使用。GitHub 托管了 [MCP registry][mcp-registry]，帮助用户发现生态系统中的资源并为其做出贡献。

> [!CAUTION]
> 使用 MCP 服务器时应像使用项目中的其他依赖项一样谨慎。使用前请审查源代码、验证发布者并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果已在 Copilot CLI 中配置 Playwright MCP，请跳过这些设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 将以下内容粘贴到 **Command** 对话框中：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 通过 Playwright 探索功能

之前已手动确认功能可以正常运行。现在使用 Copilot 中的 Playwright MCP 服务器完成相同验证！

1. 使用以下提示，请 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常无需特意告诉 Copilot 使用 MCP 服务器，因为它一般会自行判断。但既然已经知道要用什么工具，明确指出也无妨！这样有助于保持结果一致，还能节省一些令牌。

2. 观察 Copilot 如何列出在浏览器中执行的步骤，以确认功能是否正常。
3. 阅读报告并确认一切符合预期。

Copilot 会启动服务器，使用 Playwright 操作网站，停止服务器，然后提供报告。

## 总结和后续步骤

已从 Copilot CLI 中使用 Playwright MCP 服务器，在真实浏览器中探索功能。在本练习中：

- 了解了 Model Context Protocol (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加了它。
- 请智能体驱动浏览器并探索筛选功能。

下一步，[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专门角色中。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
