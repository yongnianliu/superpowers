# 附录

> 本附录汇总了 Superpowers 的完整技能清单、中英文术语对照表和参考链接。

---

## A. 完整技能清单

下表列出 Superpowers 技能库中全部 14 项技能及其触发条件与分类。

| # | 技能名称 | 中文名 | 触发条件 | 分类 |
|---|---------|-------|---------|------|
| 1 | using-superpowers | 根技能 | 开始任何会话时使用 | 基础设施 |
| 2 | brainstorming | 头脑风暴 | 在任何创造性工作之前必须使用——创建功能、构建组件、添加功能或修改行为 | 核心工作流 |
| 3 | writing-plans | 编写计划 | 当你有规格说明或需求文档，需要拆分为多步任务时，在动手写代码之前使用 | 核心工作流 |
| 4 | subagent-driven-development | 子智能体驱动开发 | 在当前会话中执行包含独立任务的实现计划时使用 | 核心工作流 |
| 5 | executing-plans | 直接执行计划 | 当你有书面实现计划，需要在独立会话中执行并设有审查检查点时使用 | 核心工作流 |
| 6 | test-driven-development | 测试驱动开发 | 在实现任何功能或修复 bug 之前，在编写实现代码之前使用 | 横切关注点 |
| 7 | systematic-debugging | 系统化调试 | 遇到任何 bug、测试失败或意外行为时，在提出修复方案之前使用 | 横切关注点 |
| 8 | verification-before-completion | 完成前验证 | 即将声称工作已完成、已修复或已通过时，在提交或创建 PR 之前使用 | 横切关注点 |
| 9 | requesting-code-review | 请求代码审查 | 完成任务、实现主要功能或合并前，用于验证工作是否符合要求 | 横切关注点 |
| 10 | receiving-code-review | 接收代码审查 | 收到代码审查反馈后，在实施建议之前使用 | 横切关注点 |
| 11 | using-git-worktrees | Git 工作树 | 开始需要与当前工作区隔离的功能开发时，或在执行实现计划之前使用 | 基础设施 |
| 12 | finishing-a-development-branch | 完成开发分支 | 实现完成、所有测试通过，需要决定如何集成工作时使用 | 核心工作流 |
| 13 | dispatching-parallel-agents | 派遣并行智能体 | 面对 2 个或更多独立任务，且彼此之间没有共享状态或顺序依赖时使用 | 基础设施 |
| 14 | writing-skills | 编写技能 | 创建新技能、编辑现有技能或在部署前验证技能时使用 | 元技能 |

**分类说明：**

- **基础设施** — 提供运行环境和工具支撑的底层技能
- **核心工作流** — 驱动日常开发流程的主干技能
- **横切关注点** — 贯穿所有工作流、保障质量的技能
- **元技能** — 用于创建和维护技能本身的技能

---

## B. 术语表

按英文术语字母顺序排列。

| 英文术语 | 中文翻译 | 首次出现章节 |
|---------|---------|------------|
| Agent | 智能体 | [第1章 总览](./01-overview.md) |
| Anti-Rationalization | 反合理化 | [第1章 总览](./01-overview.md) |
| Bootstrap Injection | 引导注入 | [第2章 架构](./02-architecture.md) |
| Brainstorming | 头脑风暴 | [第3章 工作流](./03-workflow.md) |
| Claude Search Optimization (CSO) | Claude 搜索优化 | [第5章 反合理化](./05-anti-rationalization.md) |
| Composable | 可组合 | [第4章 技能系统](./04-skills-system.md) |
| Context Injection | 上下文注入 | [第2章 架构](./02-architecture.md) |
| Context Window | 上下文窗口 | [第4章 技能系统](./04-skills-system.md) |
| Controller-Worker | 控制器-工作者 | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Core Architectural Concern | 核心架构关注点 | [第5章 反合理化](./05-anti-rationalization.md) |
| Cross-Cutting Concerns | 横切关注点 | [第3章 工作流](./03-workflow.md) |
| Development Workflow | 开发工作流 | [第1章 总览](./01-overview.md) |
| Environment Variable | 环境变量 | [第2章 架构](./02-architecture.md) |
| Evidence over Claims | 证据优于声明 | [第1章 总览](./01-overview.md) |
| Frame | 帧 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Frontmatter | 前言/元数据 | [第4章 技能系统](./04-skills-system.md) |
| Gate Condition | 门控条件 | [第3章 工作流](./03-workflow.md) |
| Handshake | 握手 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Hard Gate | 硬门控 | [第4章 技能系统](./04-skills-system.md) |
| Hot Reload | 热重载 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Independently Loadable | 独立可加载 | [第4章 技能系统](./04-skills-system.md) |
| Iron Law | 铁律 | [第4章 技能系统](./04-skills-system.md) |
| Masking | 掩码 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Meta-Cognition | 元认知 | [第5章 反合理化](./05-anti-rationalization.md) |
| Pipeline | 流水线 | [第3章 工作流](./03-workflow.md) |
| Platform Adapter | 平台适配器 | [第7章 平台适配器](./07-platform-adapters.md) |
| Platform-Agnostic | 平台无关 | [第4章 技能系统](./04-skills-system.md) |
| Polyglot | 多语言脚本 | [第7章 平台适配器](./07-platform-adapters.md) |
| Pressure Testing | 压力测试 | [第9章 编写技能](./09-writing-skills.md) |
| Prompt Injection Plugin | 提示注入插件 | [第2章 架构](./02-architecture.md) |
| Rationalization | 合理化 | [第5章 反合理化](./05-anti-rationalization.md) |
| Rationalization Prevention Table | 合理化防御表 | [第5章 反合理化](./05-anti-rationalization.md) |
| Red Flags | 红旗 | [第4章 技能系统](./04-skills-system.md) |
| Red-Green-Refactor | 红-绿-重构 | [第3章 工作流](./03-workflow.md) |
| Self-Contained Document | 自包含文档 | [第4章 技能系统](./04-skills-system.md) |
| Session Hook | 会话钩子 | [第2章 架构](./02-architecture.md) |
| Skill | 技能 | [第1章 总览](./01-overview.md) |
| Skills Library | 技能库 | [第1章 总览](./01-overview.md) |
| Skills-as-Documents | 技能即文档 | [第1章 总览](./01-overview.md) |
| Spirit-over-Letter | 精神条款 | [第5章 反合理化](./05-anti-rationalization.md) |
| Subagent-Driven Development (SDD) | 子智能体驱动开发 | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Task Isolation | 任务隔离 | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Triggering Conditions | 触发条件 | [第4章 技能系统](./04-skills-system.md) |
| Worktree | 工作树 | [第3章 工作流](./03-workflow.md) |
| Zero Dependencies | 零依赖 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |

---

## C. 参考链接

| 资源 | 链接 |
|------|------|
| GitHub 仓库 | https://github.com/obra/superpowers |
| 自托管 Marketplace | https://github.com/obra/superpowers-marketplace |
| 项目博客 | https://blog.fsck.com/2025/10/09/superpowers/ |
| Discord 社区 | https://discord.gg/Jd8Vphy9jq |
| GitHub Sponsors | https://github.com/sponsors/obra |
| Prime Radiant | https://primeradiant.com |
| Claude 官方插件市场 | https://claude.com/plugins/superpowers |

---

[⬅️ 上一篇：对比分析](./10-comparison.md) | [返回目录](./README.md)
