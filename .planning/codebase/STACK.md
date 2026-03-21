# Technology Stack - Superpowers

## Overview
Superpowers (v5.0.4/5.0.5) is an AI coding agent skills/workflow framework. It is primarily a **content-driven** project -- the core "product" is a library of Markdown skill documents that guide AI agent behavior, supported by lightweight tooling in JavaScript and Bash.

## Core Technologies
- **Language(s):** JavaScript (ES Modules + CommonJS), Bash, Batch/CMD, Markdown
- **Runtime:** Node.js (no specific version pinned; uses modern features like `BigInt`, `import.meta.url`, `??=`)
- **Package manager:** npm (package.json at root and `tests/brainstorm-server/`; no lockfile at root)
- **Module system:** ESM (`"type": "module"` in root package.json); brainstorm server uses CommonJS (`.cjs` extension)

## Language Breakdown

| Language | Files | ~Lines | Est. % of codebase |
|----------|-------|--------|---------------------|
| Markdown | ~70 | ~14,500 | ~72% |
| Bash/Shell | ~25 | ~3,300 | ~17% |
| JavaScript (JS/CJS) | 6 | ~1,500 | ~8% |
| JSON | 9 | ~135 | ~1% |
| HTML | 1 | -- | <1% |
| Python | 1 | -- | <1% (example only) |
| TypeScript | 1 | -- | <1% (example only) |
| Windows Batch (.cmd) | 1 | -- | <1% |

## Key Dependencies
| Dependency | Version | Purpose |
|-----------|---------|---------|
| Node.js built-ins (`http`, `fs`, `path`, `crypto`, `os`, `url`, `child_process`) | -- | Runtime for brainstorm server and OpenCode plugin |
| `ws` | ^8.19.0 | WebSocket client for brainstorm server tests only (`tests/brainstorm-server/`) |

> The project has **zero external runtime dependencies**. The brainstorm server (`server.cjs`) implements RFC 6455 WebSocket from scratch using Node.js built-ins. The `ws` package is used solely in the test suite.

## Dev Dependencies
| Dependency | Version | Purpose |
|-----------|---------|---------|
| _(none declared)_ | -- | No dev dependencies in any package.json |

> Testing is done via plain Node.js (`node server.test.js`) and Bash scripts -- no test framework (Jest, Mocha, etc.) is used.

### Optional External Tools
- **Graphviz** (`dot`) -- Used by `skills/writing-skills/render-graphs.js` to render DOT diagrams from skill files to SVG

## Build & Tooling
- **Build system:** None -- no compilation, bundling, or transpilation step
- **Linting / formatting:** None configured (no ESLint, Prettier, etc.)
- **Type checking:** None (no TypeScript, no JSDoc types)
- **Bundling:** None
- **Version management:** Manual bumps across `package.json`, `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json`, `.cursor-plugin/plugin.json`, and `gemini-extension.json`

## Infrastructure
- **Deployment target:** Distributed as a plugin via multiple AI coding agent ecosystems:
  - Claude Code plugin marketplace (official + custom marketplace at `obra/superpowers-marketplace`)
  - Cursor plugin marketplace
  - OpenCode plugin system (git-based install via `opencode.json`)
  - Codex (git clone + symlink to `~/.agents/skills/`)
  - Gemini CLI extensions (`gemini extensions install`)
- **CI/CD:** None -- no GitHub Actions workflows, no automated testing pipeline
- **Hosting:** GitHub (source, issue tracker, sponsorship via GitHub Sponsors)
- **No Docker, no cloud deployment** -- this is a purely client-side plugin

## Key Source Files

| File | Purpose |
|------|---------|
| `hooks/session-start` | Bash -- SessionStart hook that injects `using-superpowers` skill into AI agent context (platform-aware JSON output) |
| `hooks/run-hook.cmd` | Cross-platform polyglot (batch+bash) wrapper for running hooks on Windows/Unix |
| `.opencode/plugins/superpowers.js` | ESM plugin for OpenCode -- injects skills paths and bootstrap context via system prompt transform |
| `skills/brainstorming/scripts/server.cjs` | Zero-dependency HTTP+WebSocket server for interactive brainstorming UI (~338 lines, Node.js built-ins only) |
| `skills/brainstorming/scripts/helper.js` | Browser-side JS injected into brainstorm UI for WebSocket communication and UI interaction |
| `skills/brainstorming/scripts/frame-template.html` | HTML template for wrapping brainstorm screen content |
| `skills/writing-skills/render-graphs.js` | Utility for rendering DOT graph notation in skill files to SVG |
| `skills/*/SKILL.md` | Markdown skill definitions with YAML frontmatter (the core product) |

## Platform Integration Files

| File | Platform | Purpose |
|------|----------|---------|
| `.claude-plugin/plugin.json` | Claude Code | Plugin manifest (v5.0.5) |
| `.claude-plugin/marketplace.json` | Claude Code | Marketplace listing |
| `.cursor-plugin/plugin.json` | Cursor | Plugin manifest (references `skills/`, `agents/`, `commands/`, hooks) |
| `hooks/hooks.json` | Claude Code | Hook registration (SessionStart, matcher: `startup\|clear\|compact`) |
| `hooks/hooks-cursor.json` | Cursor | Hook registration (sessionStart) |
| `.opencode/plugins/superpowers.js` | OpenCode | Plugin entry point (ESM) |
| `.opencode/INSTALL.md` | OpenCode | Installation instructions |
| `.codex/INSTALL.md` | Codex | Installation instructions |
| `gemini-extension.json` | Gemini CLI | Extension manifest (contextFileName: `GEMINI.md`) |
| `GEMINI.md` | Gemini CLI | Context file with `@` includes to skill files |

## Project Structure
```
superpowers/
  skills/            # 16 skill directories (core product - Markdown)
  agents/            # Agent persona definitions (code-reviewer.md)
  commands/          # Slash commands (brainstorm, write-plan, execute-plan)
  hooks/             # Session lifecycle hooks (Bash + JSON configs)
  tests/             # Test suites (Bash scripts, Node.js tests)
    brainstorm-server/  # WebSocket server tests (uses ws npm package)
    claude-code/        # Claude Code integration tests (Bash)
    explicit-skill-requests/  # Skill triggering tests (Bash)
    opencode/           # OpenCode plugin tests (Bash)
    skill-triggering/   # Skill auto-triggering tests (Bash)
    subagent-driven-dev/  # Subagent workflow tests (Bash)
  docs/              # Documentation, design specs, plans
  .claude-plugin/    # Claude Code plugin manifests
  .cursor-plugin/    # Cursor plugin manifest
  .opencode/         # OpenCode plugin (JS entry point)
  .codex/            # Codex installation guide
```
