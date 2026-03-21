# Superpowers 中文技术文档

> Superpowers 是一个面向 AI 编程助手（AI Coding Assistant）的技能库（Skills Library），
> 通过结构化的 Markdown 文档为 AI 智能体（Agent）注入开发工作流和最佳实践。
>
> 版本：5.0.x | 作者：Jesse Vincent / Prime Radiant

## 关于本文档

本文档是对 Superpowers 项目的中文技术深度解析，旨在帮助开发者全面理解其设计理念与实现细节。文档涵盖系统架构、开发工作流、技能系统以及多平台适配等核心主题。每一章节均为独立的 `.md` 文件，既可按顺序通读以获得完整认知，也可根据需要单独查阅特定章节。本文档的目标读者是希望理解、使用或扩展 Superpowers 的开发者。

## 章节索引

| 章节 | 内容简介 | 核心概念 |
|------|---------|---------|
| [01 — 概述与设计哲学](./01-overview.md) | Superpowers 是什么、为什么需要它、四大设计哲学 | 技能即文档（Skills-as-Documents）、指令优先级（Instruction Priority） |
| [02 — 系统架构](./02-architecture.md) | 整体架构、引导注入模式、目录结构 | 引导注入（Bootstrap Injection）、会话钩子（Session Hook） |
| [03 — 核心工作流](./03-workflow.md) | 从头脑风暴到分支完成的完整开发流水线 | 工作流流水线（Workflow Pipeline）、Git 工作树（Worktree） |
| [04 — 技能系统详解](./04-skills-system.md) | 技能文档的结构、加载机制、组合方式 | YAML 前言（Frontmatter）、技能组合（Composition） |
| [05 — 反合理化设计模式](./05-anti-rationalization.md) | 防止 AI 自我合理化的铁律、红旗与硬门 | 铁律（Iron Laws）、红旗表（Red Flags）、硬门（Hard Gates） |
| [06 — 子智能体编排](./06-subagent-orchestration.md) | 主智能体如何拆分任务并协调子智能体 | 子智能体驱动开发（Subagent-Driven Development） |
| [07 — 平台适配层](./07-platform-adapters.md) | 五大平台的适配机制与差异 | 平台适配器（Platform Adapter）、钩子注入（Hook Injection） |
| [08 — Brainstorm Companion Server](./08-brainstorm-server.md) | 内置 WebSocket 服务器的设计与实现 | WebSocket、零依赖（Zero Dependencies） |
| [09 — 技能开发指南](./09-writing-skills.md) | 如何编写、测试和发布自定义技能 | 技能模板（Skill Template）、精神条款（Spirit-over-Letter） |
| [10 — 对比分析](./10-comparison.md) | Superpowers 与其他 AI 工作流方案的对比 | 差异化优势（Differentiators） |
| [11 — 附录](./11-appendix.md) | 术语表、技能清单、参考链接 | — |

## 推荐阅读路径

- **快速了解**（10 分钟）：[01 概述](./01-overview.md) → [03 核心工作流](./03-workflow.md)
- **深入理解**（30 分钟）：[01 概述](./01-overview.md) → [02 架构](./02-architecture.md) → [04 技能系统](./04-skills-system.md) → [05 反合理化](./05-anti-rationalization.md)
- **完整阅读**：按顺序 01 → 11，建议预留 1–2 小时

## 约定说明

- **术语处理**：技术术语在首次出现时以"中文（English Original）"格式标注，后续直接使用中文表述。
- **图表**：文档中的架构图与流程图使用 Mermaid 语法绘制，可在 GitHub 及 VS Code 中直接渲染。
- **代码示例**：所有代码片段均来自代码库中的实际文件，并在代码块上方标注对应的文件路径。
- **导航**：每篇文章末尾提供"上一篇 / 下一篇"链接，方便顺序阅读时快速跳转。

---
本文档基于 [Superpowers](https://github.com/obra/superpowers) 代码库分析撰写。
