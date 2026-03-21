# Requirements: Superpowers 中文技术文档

## Scope
为 Superpowers 项目编写全面的中文技术文档系列，以多文件 Markdown 形式存放在 `docs/zh/` 目录下。覆盖原理、架构、实现三个深度层次，面向用户、开发者和技术社区三类读者。包含与其他 AI 编程工具的对比分析。

## Functional Requirements

### FR-1: 文档结构与导航
- FR-1.1: 提供 `docs/zh/README.md` 作为文档索引/目录页，包含所有章节的链接和简介
- FR-1.2: 每个章节为独立 `.md` 文件，可单独阅读也可串联理解
- FR-1.3: 章节间提供"上一篇/下一篇"导航链接
- FR-1.4: 技术术语在首次出现时提供英文原文+中文解释，后续使用中文或约定俗成的英文

### FR-2: 概述与设计哲学（01-overview.md）
- FR-2.1: 说明 Superpowers 是什么——一个让 AI 编程助手拥有结构化开发工作流的技能库
- FR-2.2: 解释核心洞察——AI agent 在有结构化流程指导时表现远优于随意指令
- FR-2.3: 阐述四大设计哲学（TDD、系统化优于随意化、复杂度削减、证据优于声明）
- FR-2.4: 介绍"技能即文档"的核心概念——Markdown 文档就是代码
- FR-2.5: 说明指令优先级系统（用户指令 > Superpowers 技能 > 默认系统提示）

### FR-3: 系统架构（02-architecture.md）
- FR-3.1: 描述 Prompt Injection Plugin 架构模式——Superpowers 通过上下文注入控制 AI 行为
- FR-3.2: 用图表展示系统组件关系（技能层、平台适配层、钩子系统、brainstorm 服务器）
- FR-3.3: 解释会话启动生命周期（平台启动→钩子触发→引导注入→技能发现→按需加载）
- FR-3.4: 描述技能依赖图——从 using-superpowers 根技能到各个子技能的引用关系
- FR-3.5: 说明数据/工件流向（用户想法→设计文档→实现计划→代码提交）
- FR-3.6: 描述系统边界——Superpowers 控制什么、不控制什么

### FR-4: 核心工作流（03-workflow.md）
- FR-4.1: 详细描述完整开发工作流管线（brainstorming→worktree→plan→implement→finish）
- FR-4.2: 每个阶段说明输入/输出/门控条件/产出物
- FR-4.3: 解释横切关注点技能（TDD、调试、验证、代码审查）如何贯穿整个流程
- FR-4.4: 提供工作流流程图（Mermaid 格式）

### FR-5: 技能系统详解（04-skills-system.md）
- FR-5.1: 解释技能（Skill）的定义——YAML frontmatter + Markdown body 的自包含文档
- FR-5.2: 详述技能的三个特性：独立可加载、可组合、平台无关
- FR-5.3: 说明技能触发机制——1% 可能性规则、红旗表、合理化防御
- FR-5.4: 描述技能分类（流程技能 vs 实现技能、刚性 vs 弹性）
- FR-5.5: 列出所有现有技能并分类说明其功能

### FR-6: 反合理化设计模式（05-anti-rationalization.md）
- FR-6.1: 解释为什么需要反合理化——AI agent 倾向于合理化跳过步骤
- FR-6.2: 详述五种反合理化手段（铁律、红旗表、合理化表、硬门控、精神条款）
- FR-6.3: 给出具体示例（从 TDD 技能、brainstorming 技能等提取）
- FR-6.4: 解释 CSO（Claude Search Optimization）——技能描述为何不能总结工作流
- FR-6.5: 分析这一设计模式的独特性——这不是偶然的，是核心架构关注点

### FR-7: 子智能体编排（06-subagent-orchestration.md）
- FR-7.1: 描述 Controller-Worker 模式——主智能体协调、子智能体执行
- FR-7.2: 解释"新鲜上下文"原则——每个子智能体不继承会话历史
- FR-7.3: 详述两阶段审查机制（规格合规→代码质量）
- FR-7.4: 说明 Prompt 模板系统（implementer、spec-reviewer、code-quality-reviewer）
- FR-7.5: 描述状态报告协议（DONE | DONE_WITH_CONCERNS | BLOCKED | NEEDS_CONTEXT）
- FR-7.6: 对比 subagent-driven-development 和 executing-plans 两种执行模式

### FR-8: 平台适配层（07-platform-adapters.md）
- FR-8.1: 逐一说明 5 个平台的集成方式（Claude Code、Cursor、Gemini CLI、Codex、OpenCode）
- FR-8.2: 比较各平台的钩子格式、JSON 输出结构、安装方式
- FR-8.3: 解释 Windows 跨平台支持（polyglot CMD/Bash 包装器、LF 行尾强制）
- FR-8.4: 描述平台检测机制（环境变量、条件 JSON 输出）
- FR-8.5: 说明模块系统处理（ESM vs CJS、`.cjs` 扩展名的由来）

### FR-9: Brainstorm Companion Server（08-brainstorm-server.md）
- FR-9.1: 描述这是系统中唯一的"真正代码"——零依赖 HTTP+WebSocket 服务器
- FR-9.2: 解释手工实现 RFC 6455 WebSocket 协议的技术细节
- FR-9.3: 说明服务器生命周期管理（启动、空闲超时、进程监控、关闭）
- FR-9.4: 描述文件监视与浏览器实时通信机制
- FR-9.5: 记录 Windows 平台适配（MSYS2 PID 命名空间问题）

### FR-10: 技能开发指南（09-writing-skills.md）
- FR-10.1: 解释技能创建流程——TDD 应用于流程文档
- FR-10.2: 说明压力测试方法论（RED-GREEN-REFACTOR 用于文档）
- FR-10.3: 描述技能目录结构和命名规范
- FR-10.4: 列出技能编写最佳实践（token 效率、反合理化加固、CSO）
- FR-10.5: 提供创建新技能的步骤指南

### FR-11: 对比分析（10-comparison.md）
- FR-11.1: 与其他 AI 编程工具增强方案对比（如 Cursor Rules、Aider conventions、Continue.dev 等）
- FR-11.2: 从多个维度进行对比（架构方式、工作流覆盖、平台支持、可扩展性、社区生态）
- FR-11.3: 分析 Superpowers 的独特优势（反合理化、子智能体编排、TDD 强制、多平台支持）
- FR-11.4: 客观说明局限性（无 CI/CD、平台 API 依赖、版本同步问题）

### FR-12: 附录
- FR-12.1: 完整技能清单附录（名称、触发条件、分类）
- FR-12.2: 术语表（中英文对照）
- FR-12.3: 参考链接汇总

## Non-Functional Requirements

### NFR-1: 语言质量
- 全中文撰写，技术术语处理得当
- 语言准确但不失可读性，避免翻译腔
- 适合技术博客和文档站发布

### NFR-2: 图表与可视化
- 使用 Mermaid 语法绘制架构图和流程图
- 重要概念用表格对比展示
- ASCII 图表作为备选方案

### NFR-3: 准确性
- 所有技术描述基于代码库实际分析，不凭印象
- 代码示例从实际文件中提取
- 版本号和文件路径引用准确

### NFR-4: 可维护性
- 文档结构模块化，单个章节更新不影响其他章节
- 避免跨章节硬编码引用特定行号

## Out of Scope
- 翻译 README.md（这是独立的中文技术文档，不是翻译）
- 修改任何源代码
- 创建中文版技能文档
- 搭建文档站点基础设施（只生成 Markdown 文件）
