---
title: "练习 5 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能，自定义报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是写代码。前面已手动验证代码是否正常运行，并使用说明文件确保代码符合标准。但测试、lint，以及持续集成 (CI) 中的其他工作又该如何处理？

这类任务最适合使用**智能体技能**！技能可以帮助 Copilot 了解如何正确执行这些操作。

在本练习中，将：

- 查看现有的 `quality-checks` 技能；
- 自定义结果报告格式；
- 重新加载并运行该技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 说明、脚本和资源

智能体技能封装可复用的任务说明、可执行脚本和辅助资源，供智能体按需加载。技能的核心是一个以技能名称命名的文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件包含说明技能名称和用途的 frontmatter、功能概述，以及何时调用技能的指导。文件夹还可以包含脚本和其他资源子目录，供技能执行时使用。

> [!NOTE]
> 技能不一定需要其他文件夹或文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有的命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 查看技能

现在来查看 Tailspin Toys 团队为运行单元测试、lint 和类型检查而创建的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述有助于 Copilot 判断何时调用该技能。
3. 阅读说明，了解其如何引导 Copilot 执行测试和 lint 流程。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 修改前运行技能

可以通过 Copilot CLI 直接调用技能，也可以使用自然语言提出请求。现在请 Copilot 运行技能中的三项检查。

1. 返回处于 Interactive 模式的筛选功能会话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

现在改进报告，让它说明运行了什么、是否成功，以及工具实际报告的内容。更新技能以生成此类报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改完成后，看看效果如何！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一份报告比较。

## 总结和后续步骤

已经自定义并使用了现有智能体技能。在本练习中：

- 查看了用于单元测试、lint 和类型检查的 `quality-checks` 技能；
- 自定义了结果报告格式；
- 重新加载并运行了该技能。

这项更改会与筛选功能一起包含在功能 PR 中。接下来使用 [Playwright MCP 服务器][next-lesson]，让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例供参考，不是本练习的必做任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和捆绑的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
