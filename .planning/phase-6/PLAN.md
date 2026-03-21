# Phase 6: 附录与整合 — Executable Plan

**Goal:** 完成附录内容，统一术语，检查导航链接，最终审阅
**Deliverables:** `docs/zh/11-appendix.md`, updated `docs/zh/README.md`
**Requirements:** FR-12 (12.1–12.3), NFR-1 through NFR-4

---

## Task 1: Write `docs/zh/11-appendix.md` — 附录

**File:** `docs/zh/11-appendix.md`
**Requirements:** FR-12.1, FR-12.2, FR-12.3

### What to write

#### Header

```markdown
# 附录

> 本附录汇总了 Superpowers 的完整技能清单、中英文术语对照表和参考链接。
```

#### Section 1: 完整技能清单（FR-12.1）

Table with ALL 14 skills:

| # | 技能名称 | 中文名 | 触发条件 | 分类 |
|---|---------|-------|---------|------|
| 1 | using-superpowers | 根技能 | 会话启动时自动加载 | 基础设施 |
| 2 | brainstorming | 头脑风暴 | 任何创造性工作——创建功能、构建组件、添加功能或修改行为之前 | 核心工作流 |
| 3 | writing-plans | 编写计划 | 有规格或需求文档，需要拆分为多步任务时 | 核心工作流 |
| 4 | subagent-driven-development | 子智能体驱动开发 | 执行包含独立任务的实现计划时 | 核心工作流 |
| 5 | executing-plans | 直接执行计划 | 在独立会话中执行实现计划时 | 核心工作流 |
| 6 | test-driven-development | 测试驱动开发 | 实现任何功能或修复 bug 之前 | 横切关注点 |
| 7 | systematic-debugging | 系统化调试 | 遇到任何 bug、测试失败或意外行为时 | 横切关注点 |
| 8 | verification-before-completion | 完成前验证 | 即将声称工作完成、已修复或已通过时 | 横切关注点 |
| 9 | requesting-code-review | 请求代码审查 | 完成任务、实现主要功能或合并前 | 横切关注点 |
| 10 | receiving-code-review | 接收代码审查 | 收到代码审查反馈时 | 横切关注点 |
| 11 | using-git-worktrees | Git 工作树 | 开始需要隔离的功能开发或执行实现计划之前 | 基础设施 |
| 12 | finishing-a-development-branch | 完成开发分支 | 实现完成、所有测试通过后 | 核心工作流 |
| 13 | dispatching-parallel-agents | 派遣并行智能体 | 面对 2+ 个可并行的独立任务时 | 基础设施 |
| 14 | writing-skills | 编写技能 | 创建、编辑或验证技能时 | 元技能 |

#### Section 2: 术语表（FR-12.2）

Alphabetical by English term. Include ALL terms introduced across chapters 01-10:

| 英文术语 | 中文翻译 | 首次出现 |
|---------|---------|---------|
| Agent | 智能体 | 01 |
| Anti-Rationalization | 反合理化 | 01/05 |
| Bootstrap Injection | 引导注入 | 01/02 |
| Brainstorming | 头脑风暴 | 03 |
| Claude Search Optimization (CSO) | Claude 搜索优化 | 05/09 |
| Composable | 可组合 | 04 |
| Context Injection | 上下文注入 | 02 |
| Context Window | 上下文窗口 | 04 |
| Controller-Worker | 控制器-工作者 | 06 |
| Core Architectural Concern | 核心架构关注点 | 05 |
| Cross-Cutting Concerns | 横切关注点 | 03 |
| Development Workflow | 开发工作流 | 01 |
| Environment Variable | 环境变量 | 02 |
| Evidence over Claims | 证据优于声明 | 01 |
| Frame | 帧 | 08 |
| Frontmatter | 前言/元数据 | 04 |
| Gate Condition | 门控条件 | 03 |
| Hard Gate | 硬门控 | 04/05 |
| Handshake | 握手 | 08 |
| Hot Reload | 热重载 | 08 |
| Independently Loadable | 独立可加载 | 04 |
| Iron Law | 铁律 | 04/05 |
| Masking | 掩码 | 08 |
| Meta-Cognition | 元认知 | 05 |
| Pipeline | 流水线 | 03 |
| Platform Adapter | 平台适配器 | 07 |
| Platform-Agnostic | 平台无关 | 04 |
| Polyglot | 多语言/跨平台 | 07 |
| Pressure Testing | 压力测试 | 09 |
| Prompt Injection Plugin | 提示注入插件 | 02 |
| Rationalization | 合理化 | 05 |
| Rationalization Prevention Table | 合理化防御表 | 05 |
| Red Flags | 红旗 | 04/05 |
| Red-Green-Refactor | 红-绿-重构 | 01/03 |
| Self-Contained Document | 自包含文档 | 04 |
| Session Hook | 会话钩子 | 02 |
| Skill | 技能 | 01 |
| Skills Library | 技能库 | 01 |
| Skills-as-Documents | 技能即文档 | 01 |
| Spirit-over-Letter | 精神条款 | 05 |
| Subagent-Driven Development (SDD) | 子智能体驱动开发 | 06 |
| Task Isolation | 任务隔离 | 06 |
| Triggering Conditions | 触发条件 | 04 |
| Worktree | 工作树 | 03 |
| Zero Dependencies | 零依赖 | 08 |

(This is a representative list — the writer should include ALL terms that appeared with English annotations across chapters)

#### Section 3: 参考链接（FR-12.3）

| 资源 | 链接 |
|------|------|
| GitHub 仓库 | https://github.com/obra/superpowers |
| 自托管 Marketplace | https://github.com/obra/superpowers-marketplace |
| 项目博客 | https://blog.fsck.com/2025/10/09/superpowers/ |
| Discord 社区 | https://discord.gg/Jd8Vphy9jq |
| GitHub Sponsors | https://github.com/sponsors/obra |
| Prime Radiant | https://primeradiant.com |
| Claude 插件市场 | https://claude.com/plugins/superpowers |

#### Footer navigation

```markdown
---

[⬅️ 上一篇：对比分析](./10-comparison.md) | [返回目录](./README.md)
```

### Key writing guidelines
1. Chinese with English in parentheses in the glossary table
2. Skills table: use the ACTUAL description text from each SKILL.md frontmatter (translated to Chinese)
3. Glossary: alphabetical by English, include chapter of first appearance
4. Links: verify all URLs are correct (use the ones from project context)

### Acceptance criteria
- [ ] File exists at `docs/zh/11-appendix.md`
- [ ] Complete skill catalog with all 14 skills (name, Chinese name, trigger, category)
- [ ] Glossary with ≥30 bilingual term entries
- [ ] Reference links section with ≥5 links
- [ ] Footer navigation back to chapter 10 and README
- [ ] No Mermaid needed (tables are the primary format)

### Verification
```bash
test -f docs/zh/11-appendix.md && echo "PASS: exists" || echo "FAIL"
for skill in "using-superpowers" "brainstorming" "writing-plans" "subagent-driven-development" "test-driven-development" "systematic-debugging" "verification-before-completion" "writing-skills"; do
  grep -q "$skill" docs/zh/11-appendix.md && echo "PASS: $skill" || echo "FAIL: $skill"
done
grep -q "术语" docs/zh/11-appendix.md && echo "PASS: glossary" || echo "FAIL: glossary"
grep -q "github.com/obra/superpowers" docs/zh/11-appendix.md && echo "PASS: links" || echo "FAIL: links"
grep -q "10-comparison.md" docs/zh/11-appendix.md && echo "PASS: nav" || echo "FAIL: nav"
```

---

## Task 2: Update `docs/zh/README.md` — 最终版索引

**File:** `docs/zh/README.md` (update existing)
**Requirements:** FR-1.1 (final update)

### What to do

Read the existing `docs/zh/README.md` and verify:
1. All 11 chapter links are correct and point to existing files
2. Chapter descriptions match actual content
3. Reading paths are still valid
4. No broken links

If everything is correct, no changes needed. If any descriptions need updating based on actual chapter content, update them.

### Acceptance criteria
- [ ] All 11 chapter file links resolve to existing files
- [ ] No broken navigation

### Verification
```bash
for f in 01-overview.md 02-architecture.md 03-workflow.md 04-skills-system.md 05-anti-rationalization.md 06-subagent-orchestration.md 07-platform-adapters.md 08-brainstorm-server.md 09-writing-skills.md 10-comparison.md 11-appendix.md; do
  test -f "docs/zh/$f" && echo "PASS: $f exists" || echo "FAIL: $f missing"
  grep -q "$f" docs/zh/README.md && echo "PASS: $f linked" || echo "FAIL: $f not linked in README"
done
```

---

## Execution Order

```
Task 1 (11-appendix.md)  ←→  Task 2 (README.md check)   [parallel]
```

---

## Phase 6 Completion Checklist

- [ ] `docs/zh/11-appendix.md` — complete appendix with skill catalog, glossary, links
- [ ] `docs/zh/README.md` — all links verified, descriptions accurate
- [ ] All 12 files exist in `docs/zh/`
- [ ] Navigation chain complete: README ↔ 01 ↔ 02 ↔ ... ↔ 11
- [ ] Project complete! 🎉
