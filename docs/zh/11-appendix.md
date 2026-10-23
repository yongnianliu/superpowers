# 附录

> 本附录汇总了 Superpowers 的完整技能清单、中英文术语对照表、参考链接，以及从 5.0 到 7.0 的主要变化。

---

## A. 完整技能清单

下表列出 Superpowers 技能库中全部 15 项技能及其触发条件（取自各 `SKILL.md` 的 `description`，7.0.1）与分类。

| # | 技能名称 | 中文名 | 触发条件 | 分类 |
|---|---------|-------|---------|------|
| 1 | using-superpowers | 根技能 | 开始任何会话时使用——建立技能发现与加载机制，要求在**任何**响应（包括澄清问题）之前先调用技能 | 基础设施 |
| 2 | brainstorming | 头脑风暴 | 任何创造性工作之前必须使用——创建功能、构建组件、添加功能、修改行为，或规划任何新事物（软件或非软件，如一场演讲、一门生意、一次装修） | 核心工作流 |
| 3 | writing-plans | 编写计划 | 有多步任务的规格或需求，在动手写代码之前使用 | 核心工作流 |
| 4 | subagent-driven-development | 子智能体驱动开发 | 在当前会话中执行包含独立任务的实现计划时使用 | 核心工作流 |
| 5 | executing-plans | 执行计划（Native） | 在当前会话中由自己担任实现者执行计划——用户选择了内联执行，或平台没有子智能体工具 | 核心工作流 |
| 6 | test-driven-development | 测试驱动开发 | 实现任何功能或修复 bug 之前，在编写实现代码之前使用 | 横切关注点 |
| 7 | systematic-debugging | 系统化调试 | 遇到任何 bug、测试失败或意外行为时，在提出修复方案之前使用 | 横切关注点 |
| 8 | verification-before-completion | 完成前验证 | 即将声称工作已完成、已修复或已通过时，在提交或创建 PR 之前使用 | 横切关注点 |
| 9 | requesting-code-review | 请求代码审查 | 完成任务、实现主要功能或合并前，用于验证工作是否符合要求 | 横切关注点 |
| 10 | receiving-code-review | 接收代码审查 | 收到代码审查反馈后，在实施建议之前使用，尤其是反馈不明确或技术上存疑时 | 横切关注点 |
| 11 | using-git-worktrees | Git 工作树 | 开始需要与当前工作区隔离的功能开发时，或在执行实现计划之前使用 | 基础设施 |
| 12 | finishing-a-development-branch | 完成开发分支 | 实现完成、所有测试通过，需要决定如何集成工作时使用 | 核心工作流 |
| 13 | dispatching-parallel-agents | 派遣并行智能体 | 面对 2 个或更多独立任务，且彼此之间没有共享状态或顺序依赖时使用 | 基础设施 |
| 14 | writing-skills | 编写技能 | 创建新技能、编辑现有技能或在部署前验证技能时使用 | 元技能 |
| 15 | diagnosing-superpowers | 诊断 Superpowers | 一次 Superpowers 会话出了问题而用户想知道原因（重复劳动、忽略计划、技能没触发、"太久了""太贵了"），或要为维护者构建 bug 报告 | 元技能 |

**分类说明：**

- **基础设施** — 提供运行环境和工具支撑的底层技能
- **核心工作流** — 驱动日常开发流程的主干技能
- **横切关注点** — 贯穿所有工作流、保障质量的技能
- **元技能** — 用于创建、维护和诊断技能本身的技能

---

## B. 术语表

按英文术语字母顺序排列。标 **（新）** 的是 5.0 之后新增的概念。

| 英文术语 | 中文翻译 | 首次出现章节 |
|---------|---------|------------|
| Agent | 智能体 | [第1章 总览](./01-overview.md) |
| Anti-Rationalization | 反合理化 | [第1章 总览](./01-overview.md) |
| Bootstrap Injection | 引导注入 | [第2章 架构](./02-architecture.md) |
| Brainstorming | 头脑风暴 | [第3章 工作流](./03-workflow.md) |
| Builder Check **（新）** | 构建者检查 | [第3章 工作流](./03-workflow.md) |
| Circuit Breaker **（新）** | 断路器（五轮修复上限） | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Composable | 可组合 | [第4章 技能系统](./04-skills-system.md) |
| Context Injection | 上下文注入 | [第2章 架构](./02-architecture.md) |
| Context Window | 上下文窗口 | [第4章 技能系统](./04-skills-system.md) |
| Controller-Worker | 控制器-工作者 | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Core Architectural Concern | 核心架构关注点 | [第5章 反合理化](./05-anti-rationalization.md) |
| Cross-Cutting Concerns | 横切关注点 | [第3章 工作流](./03-workflow.md) |
| Declined to Judge **（新）** | 拒绝裁决清单 | [第3章 工作流](./03-workflow.md) |
| Development Workflow | 开发工作流 | [第1章 总览](./01-overview.md) |
| Environment Variable | 环境变量 | [第2章 架构](./02-architecture.md) |
| Evidence over Claims | 证据优于声明 | [第1章 总览](./01-overview.md) |
| Frame | 帧 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Frontmatter | 前言/元数据 | [第4章 技能系统](./04-skills-system.md) |
| Gate Condition | 门控条件 | [第3章 工作流](./03-workflow.md) |
| Handshake | 握手 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Hard Gate | 硬门 | [第5章 反合理化](./05-anti-rationalization.md) |
| Hot Reload | 热重载 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Independently Loadable | 独立可加载 | [第4章 技能系统](./04-skills-system.md) |
| Iron Law | 铁律 | [第5章 反合理化](./05-anti-rationalization.md) |
| Ledger **（新）** | 账本（`progress.md`） | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Look Before You Ask **（新）** | 先读后问 | [第3章 工作流](./03-workflow.md) |
| Masking | 掩码 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Match the Form to the Failure **（新）** | 选对形式 | [第5章 反合理化](./05-anti-rationalization.md) |
| Meta-Cognition | 元认知 | [第5章 反合理化](./05-anti-rationalization.md) |
| Micro-Test **（新）** | 微测试 | [第9章 编写技能](./09-writing-skills.md) |
| Native Execution **（新）** | Native（内联）执行 | [第3章 工作流](./03-workflow.md) |
| Pipeline | 流水线 | [第3章 工作流](./03-workflow.md) |
| Platform Adapter | 平台适配器 | [第7章 平台适配器](./07-platform-adapters.md) |
| Platform-Agnostic | 平台无关 | [第4章 技能系统](./04-skills-system.md) |
| Polyglot | 多语言脚本 | [第7章 平台适配器](./07-platform-adapters.md) |
| Preflight **（新）** | 预检（冲突扫描） | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Pressure Testing | 压力测试 | [第9章 编写技能](./09-writing-skills.md) |
| Prompt Injection Plugin | 提示注入插件 | [第2章 架构](./02-architecture.md) |
| Rationalization | 合理化 | [第5章 反合理化](./05-anti-rationalization.md) |
| Rationalization Prevention Table | 合理化防御表 | [第5章 反合理化](./05-anti-rationalization.md) |
| Red Flags | 红旗 | [第4章 技能系统](./04-skills-system.md) |
| Red-Green-Refactor | 红-绿-重构 | [第3章 工作流](./03-workflow.md) |
| Review Package **（新）** | 评审包 | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Ruling **（新）** | 裁决（账本中的 `Ruling:` 行） | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Self-Contained Document | 自包含文档 | [第4章 技能系统](./04-skills-system.md) |
| Session Hook | 会话钩子 | [第2章 架构](./02-architecture.md) |
| Session Key **（新）** | 会话密钥 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |
| Skill | 技能 | [第1章 总览](./01-overview.md) |
| Skill Discovery Optimization (SDO) **（新，原 CSO）** | 技能发现优化 | [第5章 反合理化](./05-anti-rationalization.md) |
| Skills Library | 技能库 | [第1章 总览](./01-overview.md) |
| Skills-as-Documents | 技能即文档 | [第1章 总览](./01-overview.md) |
| Spirit-over-Letter | 精神条款 | [第5章 反合理化](./05-anti-rationalization.md) |
| Subagent-Driven Development (SDD) | 子智能体驱动开发 | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Task Isolation | 任务隔离 | [第6章 子智能体编排](./06-subagent-orchestration.md) |
| Triggering Conditions | 触发条件 | [第4章 技能系统](./04-skills-system.md) |
| Visual Companion **（新）** | 可视化伴随 | [第3章 工作流](./03-workflow.md) |
| Worktree | 工作树 | [第3章 工作流](./03-workflow.md) |
| Zero Dependencies | 零依赖 | [第8章 Brainstorm Server](./08-brainstorm-server.md) |

> 注意："SDD"在本项目中指 **Subagent-Driven Development**（子智能体驱动开发），而不是 Spec-Driven Development。

---

## C. 参考链接

| 资源 | 链接 |
|------|------|
| GitHub 仓库 | https://github.com/obra/superpowers |
| 自托管 Marketplace | https://github.com/obra/superpowers-marketplace |
| Issues | https://github.com/obra/superpowers/issues |
| 项目博客（首发公告） | https://blog.fsck.com/2025/10/09/superpowers/ |
| Discord 社区 | https://discord.gg/35wsABTejz |
| 发布通知订阅 | https://primeradiant.com/superpowers/ |
| GitHub Sponsors | https://github.com/sponsors/obra |
| Prime Radiant | https://primeradiant.com |
| Claude 官方插件市场 | https://claude.com/plugins/superpowers |
| Codex 官方插件市场 | https://github.com/openai/plugins |
| Grok 官方插件市场 | https://github.com/xai-org/plugin-marketplace |
| 技能评测套件（Quorum） | https://github.com/prime-radiant-inc/superpowers-evals/ |

仓库内的相关文档：`RELEASE-NOTES.md`（逐版本变化）、`AGENTS.md`（贡献指南）、`docs/porting-to-a-new-harness.md`（接入新平台）、`docs/testing.md`（测试体系）、`docs/windows/polyglot-hooks.md`（Windows 钩子）、`docs/README.kimi.md`、`docs/README.opencode.md`。

---

## D. 从 5.0 到 7.0 的主要变化

本文档最初基于 5.0.x 撰写。下表按主题归纳 7.0.1 之前的变化，细节以 `RELEASE-NOTES.md` 为准。

### D.1 版本时间线

| 版本 | 日期 | 要点 |
|------|------|------|
| 5.0.5–5.0.7 | 2026-03 | Brainstorm 服务器改名 `server.cjs`、会话目录分 `content/` 与 `state/`；子智能体审查循环换成内联自检；加入 Copilot CLI；OpenCode bootstrap 改为用户消息 |
| 5.1.0 | 2026-04-30 | 删除遗留斜杠命令和具名 `code-reviewer` agent；重写工作树技能（环境检测、征得同意、原生工具优先、来源感知清理）；AGENTS.md 贡献指南 |
| 6.0.0 | 2026-06-16 | SDD 重写（单审查者双裁决、文件交接、必须指定模型）；计划新增 Global Constraints / Interfaces；Brainstorm 服务器安全模型；新增 Kimi / Pi / Antigravity；技能词汇去 Claude 化；CSO 更名 SDO |
| 6.0.1–6.0.3 | 2026-06 | Codex 版本显示、移除 evals 子模块、SDD 工作区移出 `.git/` |
| 6.1.0–6.1.1 | 2026-06/07 | bootstrap 与参考文档瘦身；Codex 从市场安装、取消 Codex 钩子；（短暂）移除 Gemini CLI；Codex 清单显式 `hooks: {}` |
| 6.2.0 | 2026-07-23 | 工作区按计划隔离；修复循环恢复原实现者 + 五轮断路器；全库压缩；`testing-anti-patterns.md` → `writing-good-tests.md`；"丢弃"移出收尾菜单；Windows 钩子走 Git Bash；恢复 Gemini CLI |
| 6.3.0 | 2026-08-12 | Devin CLI、Hermes Agent、Grok Build CLI；头脑风暴按任务规模分级；SDD 不再因计划冲突卡住；批处理小任务；工作树移除不再毁掉未跟踪文件 |
| 6.4.1 | 2026-09-18 | 新增 `diagnosing-superpowers`；`executing-plans` 重建为 Native 执行；OpenCode 2.0、Muse、Qwen Code；脚本改用解释器调用 |
| 6.4.2 | 2026-09-25 | `writing-plans` 改为记录决定而不是转录代码，计划更短更快 |
| 7.0.0 | 2026-10-09 | 头脑风暴整体重写（回放、规模判断、构建者检查、"怎么做"的归属）；Antigravity 原生插件；`PATH` 损坏容错；Windows `run-hook.cmd` 加固；内容注入 bug 修复 |
| 7.0.1 | 2026-10-21 | 头脑风暴先读上下文、提问更精准、先给方案再写设计文档；三个长技能拆分 `references/` 以抗压缩截断；Issue / PR 模板要求人写摘要 |

### D.2 旧版文档中不再成立的说法

| 旧说法（5.0.x） | 7.0.1 实际情况 |
|----------------|--------------|
| 支持五个平台 | 15 个平台 |
| 约 70 个技能文档；16 个技能目录 / 14 个技能 | 15 个技能，约 70 个 Markdown 文件 |
| 有 `agents/` 与 `commands/` 层 | 两个目录均已不存在 |
| 平台检测靠 `CLAUDE_CODE_HOOK_EVENT` / `CURSOR_HOOK_EVENT` | 靠 `CURSOR_PLUGIN_ROOT`、`CLAUDE_PLUGIN_ROOT`、`COPILOT_CLI`、`MUSE_PLUGIN_ROOT` |
| Codex 用符号链接到 `~/.agents/skills/superpowers` | 通过官方 Codex 插件市场；清单显式 `hooks: {}` |
| Cursor 的 `plugin.json` 含 `agents`、`commands` | 只含 `skills` 与 `hooks` |
| 头脑风暴产出规格文档并由 `spec-document-reviewer` 审查 | 无该审查者；项目级设计由"构建者检查"把关，小改动直送 |
| 计划拆成 2–5 分钟的任务，由 `plan-document-reviewer` 审查 | 每步"一个动作、一个可检查结果"；该审查者已删除，作者自检 + 用户评审 |
| SDD 两阶段审查（`spec-reviewer` + `code-quality-reviewer`） | 单个 `task-reviewer-prompt.md`，一次读 diff 给出两个裁决 |
| SDD 的 "Spec-Driven Development" | Subagent-Driven Development |
| `executing-plans` 在并行独立会话里带人工检查点 | Native 内联执行，任务间不停，末尾一次整分支评审 |
| 收尾四选项：合并 / PR / 保留 / 丢弃 | 三选项：合并 / PR / 保持现状；detached HEAD 两选项；"丢弃"只在明确要求时出现 |
| 系统化调试四阶段：观察 / 假设 / 实验 / 验证 | Root Cause Investigation / Pattern Analysis / Hypothesis and Testing / Implementation |
| CSO（Claude Search Optimization） | SDO（Skill Discovery Optimization） |
| TDD 参考文档 `testing-anti-patterns.md` | `writing-good-tests.md` |
| Brainstorm 服务器：约 338 行、无认证、30 分钟空闲超时 | 约 723 行、有会话密钥与沙箱、默认 4 小时空闲超时 |
| `render-graphs.js` 使用 CJS `require()`（技术债务） | 已是 ESM `import` |
| 无自动化的版本同步工具 | `scripts/bump-version.sh` + `.version-bump.json`（12 个清单） |
| Discord 邀请链接 `Jd8Vphy9jq` | `https://discord.gg/35wsABTejz` |

---

[⬅️ 上一篇：对比分析](./10-comparison.md) | [返回目录](./README.md)
