# Architecture - Superpowers

## System Overview

Superpowers is a **content-driven framework** that provides AI coding assistants (Claude Code, Cursor, Gemini CLI, Codex, OpenCode) with structured development workflows via a library of Markdown "skill" documents. The core product is ~70 Markdown files that shape agent behavior — not traditional application code.

The system operates on a fundamental insight: AI coding agents perform dramatically better when given structured processes (brainstorm before coding, plan before implementing, test before shipping) rather than ad-hoc instructions. Skills are not suggestions — they are mandatory workflows that the agent must follow.

## Architecture Pattern: Prompt-Injection Plugin

Superpowers uses a **bootstrap injection pattern**:

1. **Session start** — A platform-specific hook fires when the AI agent begins a session
2. **Bootstrap injection** — The `using-superpowers` skill content is injected into the agent's context, establishing the skill system's rules
3. **Skill discovery** — The agent learns it has skills available and must check them before any action
4. **On-demand loading** — Individual skills are loaded via the platform's skill/plugin tool when triggered by conversation context

This is not a traditional client-server or MVC architecture. The "runtime" is the AI agent itself — Superpowers configures its behavior through injected context.

## Core Design Patterns

### 1. Skill-as-Document Pattern

Each skill is a self-contained Markdown document with:
- **YAML frontmatter** (`name`, `description`) — used for discovery and triggering
- **Body content** — the actual instructions the agent follows
- **Optional supporting files** — prompt templates, scripts, references

Skills are the atomic unit of the system. They are designed to be:
- **Independently loadable** — each skill works on its own
- **Composable** — skills reference other skills by name (e.g., `superpowers:writing-plans`)
- **Platform-agnostic** — written for Claude Code tool names, with tool mapping references for other platforms

### 2. Workflow Pipeline Pattern

Skills chain into a prescribed development workflow:

```
brainstorming → using-git-worktrees → writing-plans → [subagent-driven-development | executing-plans] → finishing-a-development-branch
```

Each stage gates the next:
- **brainstorming** produces a design spec (saved to `docs/superpowers/specs/`)
- **writing-plans** produces an implementation plan (saved to `docs/superpowers/plans/`)
- **subagent-driven-development** or **executing-plans** consumes the plan task-by-task
- **finishing-a-development-branch** handles merge/PR/cleanup

Cross-cutting skills apply throughout:
- **test-driven-development** — enforced during all implementation
- **systematic-debugging** — triggered by any bug or test failure
- **verification-before-completion** — required before any success claim
- **requesting-code-review** / **receiving-code-review** — between tasks and at completion

### 3. Subagent Orchestration Pattern

The `subagent-driven-development` skill implements a **controller-worker** pattern:

```
Controller (main agent session)
  ├── Dispatch implementer subagent (per task)
  │     └── Implements, tests, commits, self-reviews
  ├── Dispatch spec-reviewer subagent
  │     └── Verifies implementation matches spec
  ├── Dispatch code-quality-reviewer subagent
  │     └── Reviews code quality and practices
  └── Repeat for each task
```

Key design decisions:
- **Fresh context per subagent** — no inherited session history, prevents context pollution
- **Controller curates context** — provides exactly what each subagent needs (full task text, not file references)
- **Two-stage review** — spec compliance first, then code quality (never skip or reorder)
- **Prompt templates** — standardized templates for each subagent role (`implementer-prompt.md`, `spec-reviewer-prompt.md`, `code-quality-reviewer-prompt.md`)
- **Status protocol** — implementers report `DONE | DONE_WITH_CONCERNS | BLOCKED | NEEDS_CONTEXT`

### 4. Platform Abstraction Pattern

Superpowers supports 5+ platforms through a layered abstraction:

```
Platform-Specific Integration Layer
├── .claude-plugin/    → plugin.json, marketplace.json (Claude Code native plugin)
├── .cursor-plugin/    → plugin.json (Cursor native plugin, skills/agents/commands/hooks refs)
├── .opencode/         → superpowers.js (ESM plugin with config + system prompt hooks)
├── .codex/            → INSTALL.md (symlink-based skill discovery)
├── GEMINI.md          → Context file with @-includes (Gemini extension)
└── hooks/             → Platform-aware session-start hook (Bash + JSON)

Skill Layer (platform-agnostic)
├── skills/            → All SKILL.md files use Claude Code tool names
├── references/        → Tool mapping docs (gemini-tools.md, codex-tools.md)
└── agents/            → Agent definitions (code-reviewer.md)
```

The session-start hook (`hooks/session-start`) detects the platform via environment variables (`CURSOR_PLUGIN_ROOT`, `CLAUDE_PLUGIN_ROOT`) and emits platform-appropriate JSON:
- **Claude Code**: `hookSpecificOutput.additionalContext`
- **Cursor**: `additional_context`
- **Fallback**: `additional_context`

Windows support uses a polyglot wrapper (`hooks/run-hook.cmd`) — a file that is simultaneously valid batch script and bash script.

### 5. Anti-Rationalization Pattern

A distinctive architectural pattern: skills are designed to resist the AI agent's tendency to rationalize skipping steps. This manifests as:

- **Iron Laws** — non-negotiable rules stated in absolute terms
- **Red Flags tables** — lists of specific thoughts that signal the agent is about to deviate
- **Rationalization tables** — pre-addressed excuses with rebuttals
- **Hard Gates** — explicit `<HARD-GATE>` blocks that prevent proceeding without prerequisites
- **Spirit-over-letter clauses** — "Violating the letter of the rules is violating the spirit of the rules"

This is not incidental — it is a core architectural concern. The `writing-skills` skill dedicates significant content to "bulletproofing skills against rationalization" and the CSO (Claude Search Optimization) section explains why skill descriptions must never summarize workflows (to prevent the agent from shortcutting).

## Data Flow

### Session Lifecycle

```
1. Platform launches AI agent session
2. SessionStart hook fires → reads using-superpowers/SKILL.md
3. Hook outputs JSON with bootstrap context
4. Agent context now includes skill system rules
5. User sends message
6. Agent checks: "Does any skill apply?" (even 1% chance → load it)
7. Agent invokes platform's Skill tool to load relevant SKILL.md
8. Agent follows skill instructions exactly
9. Skill may trigger other skills (e.g., brainstorming → writing-plans)
10. Subagents may be dispatched with curated context
11. Artifacts saved to docs/superpowers/{specs,plans}/ and committed to git
```

### Artifact Flow

```
User idea
  → brainstorming
    → docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md (committed)
      → spec-document-reviewer subagent validates
        → writing-plans
          → docs/superpowers/plans/YYYY-MM-DD-<feature>.md (committed)
            → plan-document-reviewer subagent validates
              → subagent-driven-development
                → per-task: implement → spec-review → quality-review → commit
                  → finishing-a-development-branch
                    → merge / PR / keep / discard
```

## Component Relationships

### Skill Dependency Graph

```
using-superpowers (root — loaded at session start)
  ├── brainstorming (entry point for creative work)
  │     ├── visual-companion.md (optional browser-based mockups)
  │     ├── spec-document-reviewer-prompt.md (subagent template)
  │     └── → writing-plans (next step after design)
  ├── writing-plans (creates implementation plans)
  │     ├── plan-document-reviewer-prompt.md (subagent template)
  │     └── → subagent-driven-development OR executing-plans
  ├── subagent-driven-development (primary execution engine)
  │     ├── implementer-prompt.md (subagent template)
  │     ├── spec-reviewer-prompt.md (subagent template)
  │     ├── code-quality-reviewer-prompt.md (subagent template)
  │     ├── → test-driven-development (used by implementers)
  │     ├── → requesting-code-review (review between tasks)
  │     └── → finishing-a-development-branch (after all tasks)
  ├── executing-plans (alternative execution — no subagents)
  │     └── → finishing-a-development-branch
  ├── using-git-worktrees (isolation before implementation)
  ├── test-driven-development (cross-cutting, always active)
  │     └── testing-anti-patterns.md (reference)
  ├── systematic-debugging (triggered by any bug)
  │     ├── root-cause-tracing.md
  │     ├── defense-in-depth.md
  │     ├── condition-based-waiting.md (+example)
  │     └── → verification-before-completion
  ├── verification-before-completion (cross-cutting)
  ├── requesting-code-review → code-reviewer.md (agent/subagent template)
  ├── receiving-code-review
  ├── dispatching-parallel-agents (for independent concurrent tasks)
  ├── finishing-a-development-branch
  └── writing-skills (meta — for creating new skills)
        ├── anthropic-best-practices.md
        ├── persuasion-principles.md
        ├── testing-skills-with-subagents.md
        ├── graphviz-conventions.dot
        └── render-graphs.js
```

### Agents

The `agents/` directory contains reusable agent persona definitions:
- **code-reviewer** — Senior code reviewer agent dispatched by `requesting-code-review` skill

### Commands (Deprecated)

The `commands/` directory contains deprecated slash commands that redirect to skills:
- `brainstorm.md` → use `brainstorming` skill
- `write-plan.md` → use `writing-plans` skill
- `execute-plan.md` → use `executing-plans` skill

## System Boundaries

### What Superpowers Controls
- Agent behavior through injected context and skill documents
- Development workflow progression (brainstorm → plan → implement → finish)
- Quality gates (TDD, code review, verification)
- Subagent orchestration and prompt templates

### What Superpowers Does Not Control
- The AI agent's core capabilities or model
- Platform-specific tool implementations
- The user's project code or structure
- Git operations (skills instruct the agent to perform them)

### Extension Points
- **New skills** — add `skills/<name>/SKILL.md` following the `writing-skills` guide
- **New platforms** — add integration layer (plugin manifest + tool mapping reference)
- **Custom agents** — add to `agents/` directory
- **Hooks** — extend `hooks/` for additional lifecycle events

## Key Technical Details

### Brainstorm Companion Server
The only "real code" in the system — a zero-dependency Node.js HTTP+WebSocket server (`skills/brainstorming/scripts/server.cjs`) that:
- Watches a directory for HTML files, serves newest to browser
- Hand-rolled RFC 6455 WebSocket implementation
- Records user click events to `.events` file for agent to read
- 30-minute idle auto-shutdown with owner-process monitoring
- Cross-platform support (macOS, Linux, Windows, Codex CI)

### OpenCode Plugin
The only traditional plugin code — an ESM module (`.opencode/plugins/superpowers.js`) that:
- Injects bootstrap context via system prompt transform
- Auto-registers skills directory via config hook (no manual symlinks)
- Extracts YAML frontmatter from skill files

### Priority System
1. **User's explicit instructions** (CLAUDE.md, GEMINI.md, AGENTS.md) — highest
2. **Superpowers skills** — override default agent behavior
3. **Default system prompt** — lowest
