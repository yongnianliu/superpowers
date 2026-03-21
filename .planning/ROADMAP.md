# Roadmap: Superpowers 中文技术文档

## Overview
| Field | Value |
|-------|-------|
| Project | Superpowers 中文技术文档 |
| Phases | 6 |
| Est. Total | ~3 sessions |
| Output | `docs/zh/` (11 Markdown files) |

## Phase 1: 文档框架与概述
**Goal:** 建立文档结构，完成索引页和概述章节

**Deliverables:**
- `docs/zh/README.md` — 文档索引/目录页
- `docs/zh/01-overview.md` — 概述与设计哲学

**Requirements covered:** FR-1, FR-2

**Success criteria:**
- [ ] `docs/zh/` 目录创建
- [ ] README.md 包含所有章节链接和简介
- [ ] 概述章节覆盖核心概念、设计哲学、指令优先级
- [ ] 术语首次出现有中英文对照

---

## Phase 2: 架构与工作流
**Goal:** 完成系统架构和核心工作流两个技术核心章节

**Deliverables:**
- `docs/zh/02-architecture.md` — 系统架构
- `docs/zh/03-workflow.md` — 核心工作流

**Requirements covered:** FR-3, FR-4

**Success criteria:**
- [ ] 架构图使用 Mermaid 清晰展示组件关系
- [ ] 会话生命周期完整描述
- [ ] 工作流管线每个阶段有输入/输出/门控说明
- [ ] 包含横切关注点技能的描述

---

## Phase 3: 技能系统与反合理化
**Goal:** 完成技能系统详解和反合理化设计模式两个特色章节

**Deliverables:**
- `docs/zh/04-skills-system.md` — 技能系统详解
- `docs/zh/05-anti-rationalization.md` — 反合理化设计模式

**Requirements covered:** FR-5, FR-6

**Success criteria:**
- [ ] 技能定义、触发机制、分类体系完整
- [ ] 所有现有技能列表及分类
- [ ] 五种反合理化手段有具体示例
- [ ] CSO 概念解释清楚

---

## Phase 4: 子智能体与平台适配
**Goal:** 完成子智能体编排和平台适配层两个实现细节章节

**Deliverables:**
- `docs/zh/06-subagent-orchestration.md` — 子智能体编排
- `docs/zh/07-platform-adapters.md` — 平台适配层

**Requirements covered:** FR-7, FR-8

**Success criteria:**
- [ ] Controller-Worker 模式图解
- [ ] 两阶段审查流程清晰
- [ ] 5 个平台集成方式逐一说明
- [ ] 跨平台支持机制描述完整

---

## Phase 5: 服务器、技能开发与对比
**Goal:** 完成 brainstorm 服务器、技能开发指南和对比分析三个章节

**Deliverables:**
- `docs/zh/08-brainstorm-server.md` — Brainstorm Companion Server
- `docs/zh/09-writing-skills.md` — 技能开发指南
- `docs/zh/10-comparison.md` — 对比分析

**Requirements covered:** FR-9, FR-10, FR-11

**Success criteria:**
- [ ] WebSocket 协议实现有技术深度
- [ ] 技能开发 TDD 方法论清晰
- [ ] 对比分析客观，多维度覆盖
- [ ] 既说明优势也承认局限性

---

## Phase 6: 附录与整合
**Goal:** 完成附录内容，统一术语，检查导航链接，最终审阅

**Deliverables:**
- `docs/zh/11-appendix.md` — 附录（技能清单、术语表、参考链接）
- 更新 `docs/zh/README.md` — 最终版索引

**Requirements covered:** FR-12, NFR-1 through NFR-4

**Success criteria:**
- [ ] 完整技能清单（名称、触发条件、分类）
- [ ] 中英文术语表
- [ ] 所有章节间导航链接正确
- [ ] 术语使用一致
- [ ] 整体可读性审阅通过
