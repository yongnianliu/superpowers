# Project: Superpowers 中文技术文档

## Vision
为 Superpowers 项目编写一份全面、深入的中文技术文档，完整描述其工作原理、系统架构和实现细节。文档面向三类读者（用户、开发者、技术社区），采用多文件结构化 Markdown 形式，存放在 `docs/zh/` 目录下。

## Context
Superpowers 是一个为 AI 编程助手（Claude Code、Cursor、Gemini CLI、Codex、OpenCode）提供结构化开发工作流的技能库。核心产品是约 70 个 Markdown 技能文档，通过 prompt injection 机制让 AI agent 遵循规范化的开发流程（头脑风暴→设计→计划→实现→完成）。

项目特点：
- 零外部运行时依赖，仅使用 Node.js 内置模块
- 主要"代码"是 Markdown 文档（约 72% 的代码库）
- 支持 5 个 AI 编程平台，每个平台有独立适配层
- 创新的"反合理化"设计模式，防止 AI 跳过关键步骤
- 唯一实际代码是 brainstorm companion server（零依赖 HTTP+WebSocket 服务器）

## Constraints
- 语言：全中文（技术术语保留英文原文并在首次出现时给出中文解释）
- 格式：多文件 Markdown，存放于 `docs/zh/`
- 深度：原理 + 架构 + 实现，三层全面覆盖
- 扩展内容：包含与其他 AI 编程工具/框架的对比分析
- 风格：技术准确但不失可读性，适合技术博客和文档站使用

## Audience
1. **AI 工具用户** — 想要使用 Superpowers 的开发者
2. **AI 工具开发者** — 想要开发类似插件或贡献代码的工程师
3. **技术社区** — 对 AI 编程助手感兴趣的技术人群

## Success Criteria
- [ ] 完整覆盖 Superpowers 所有核心概念和工作原理
- [ ] 架构图和流程图清晰表达系统设计
- [ ] 代码实现细节有具体示例和注释
- [ ] 与同类工具的对比分析客观准确
- [ ] 各章节逻辑连贯，可独立阅读也可串联理解
- [ ] 技术术语处理得当（英文原文+中文解释）

## Tech Stack
- Output: Markdown (.md) files
- Diagrams: Mermaid or ASCII art within Markdown
- Location: `docs/zh/` directory

## Brownfield Context
基于 `.planning/codebase/` 中的 7 份代码库分析文档（ARCHITECTURE.md, STRUCTURE.md, STACK.md, INTEGRATIONS.md, CONVENTIONS.md, TESTING.md, CONCERNS.md）提供的全面代码库理解。
