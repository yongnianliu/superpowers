# Code Conventions

## Project Identity

Superpowers is a skills/workflows library for AI coding assistants (Claude Code, Cursor, Codex, OpenCode, Gemini CLI). It is **not** a traditional software project — it is primarily **structured markdown documentation** that shapes agent behavior, plus a small amount of supporting infrastructure code.

## Language & File Types

| Content Type | Format | Notes |
|---|---|---|
| Skills (core product) | Markdown (`.md`) | YAML frontmatter + markdown body |
| Commands | Markdown (`.md`) | YAML frontmatter + instructions |
| Agents | Markdown (`.md`) | Agent persona definitions |
| Hook scripts | Bash (`.sh`), polyglot CMD/Bash (`.cmd`) | Cross-platform, LF line endings enforced |
| Brainstorm server | CommonJS (`.cjs`) | Zero-dependency Node.js server |
| Helper scripts | JavaScript (`.js`) | Client-side browser helpers |
| Plugin manifests | JSON | Per-platform plugin descriptors |
| Plans/specs | Markdown (`.md`) | Design docs with date-prefixed names |
| Tests | Bash (`.sh`), JavaScript (`.js`) | Integration and unit tests |
| OpenCode plugin | ES module (`.js`) | `import`/`export` syntax with `"type": "module"` in root package.json |

## Style Guide

### Formatting Rules
- **Indentation**: 2 spaces for JSON, JavaScript; standard indentation for shell scripts
- **Line endings**: LF enforced for ALL text files via `.gitattributes` — `*.sh`, `*.md`, `*.json`, `*.js`, `*.mjs`, `*.ts`, `*.cmd`, plus extensionless hooks
- **No trailing whitespace** in committed files
- **No linting tools configured** — no ESLint, Prettier, or shellcheck in CI. Style is enforced by convention and human review

### Naming Conventions

| Context | Convention | Examples |
|---|---|---|
| Skill names | kebab-case, verb-first gerunds | `brainstorming`, `writing-plans`, `requesting-code-review` |
| Skill directories | kebab-case matching skill name | `skills/test-driven-development/`, `skills/subagent-driven-development/` |
| Test directories | kebab-case grouped by scope | `tests/brainstorm-server/`, `tests/claude-code/` |
| Shell functions | `snake_case` | `create_test_project`, `cleanup_test_project`, `wait_for_server_info` |
| Shell constants/env vars | `UPPER_SNAKE_CASE` | `SCRIPT_DIR`, `TEST_PORT`, `BRAINSTORM_DIR` |
| Shell local vars | `lower_snake_case` | `passed`, `failed`, `output` |
| JS functions/variables | `camelCase` | `computeAcceptKey`, `encodeFrame`, `payloadLen` |
| JS constants | `UPPER_SNAKE_CASE` | `OPCODES`, `WS_MAGIC`, `SERVER_PATH`, `TEST_PORT` |
| Platform dirs | dot-prefixed | `.claude-plugin/`, `.cursor-plugin/`, `.codex/`, `.opencode/` |
| Main skill file | `SKILL.md` (always uppercase) | `skills/brainstorming/SKILL.md` |
| Root docs | `UPPERCASE.md` | `README.md`, `CHANGELOG.md`, `RELEASE-NOTES.md` |
| Plan documents | `YYYY-MM-DD-<topic>.md` | `2026-01-22-document-review-system.md` |
| Spec documents | `YYYY-MM-DD-<topic>-design.md` | `2026-02-19-visual-brainstorming-refactor-design.md` |
| Test files (JS) | `<name>.test.js` | `server.test.js`, `ws-protocol.test.js` |
| Test files (Bash) | `test-<name>.sh` | `test-subagent-driven-development.sh` |
| Supporting skill docs | `kebab-case.md` | `visual-companion.md`, `defense-in-depth.md` |
| Prompt templates | `kebab-case-prompt.md` | `implementer-prompt.md`, `spec-reviewer-prompt.md` |

### Import Ordering
Not formally enforced. JavaScript files use Node.js built-ins only:
```javascript
// Typical order in .cjs files:
const crypto = require('crypto');
const http = require('http');
const fs = require('fs');
const path = require('path');

// Typical order in .js (ESM) files:
import path from 'path';
import fs from 'fs';
import os from 'os';
import { fileURLToPath } from 'url';
```

## Code Patterns

### Common Patterns Used

#### Pattern 1: Custom Inline Test Runner (no framework)
Both JS and Bash tests use a hand-rolled test runner instead of Jest/Mocha/tap.

**JS version** (`tests/brainstorm-server/ws-protocol.test.js`):
```javascript
function test(name, fn) {
  try {
    fn();
    console.log(`  PASS: ${name}`);
    passed++;
  } catch (e) {
    console.log(`  FAIL: ${name}`);
    console.log(`    ${e.message}`);
    failed++;
  }
}
```

**Bash version** (`tests/brainstorm-server/windows-lifecycle.test.sh`):
```bash
pass() { echo "  PASS: $1"; passed=$((passed + 1)); }
fail() { echo "  FAIL: $1"; echo "    $2"; failed=$((failed + 1)); }
```

#### Pattern 2: YAML Frontmatter on Markdown Files
All skills, commands, and agents use YAML frontmatter for metadata. Only two fields: `name` and `description`.

**Location**: Every `SKILL.md`, `commands/*.md`, `agents/*.md`
```yaml
---
name: skill-name-with-hyphens
description: "Use when [specific triggering conditions]."
---
```

#### Pattern 3: Script Directory Resolution
Every shell script resolves its own directory first for portable path construction.

**Location**: All `.sh` files
```bash
SCRIPT_DIR="$(cd "$(dirname "$0")" && pwd)"
```

#### Pattern 4: Trap-Based Cleanup
Test scripts and server launchers use `trap` for guaranteed cleanup of temp dirs and child processes.

**Location**: `tests/brainstorm-server/windows-lifecycle.test.sh`, `tests/claude-code/test-helpers.sh`
```bash
trap "cleanup_function" EXIT
```

#### Pattern 5: Zero External Dependencies in Production
The brainstorm server (`server.cjs`) implements WebSocket from scratch (RFC 6455) using only Node.js built-ins (`crypto`, `http`, `fs`, `path`). The `ws` npm package is test-only.

**Location**: `skills/brainstorming/scripts/server.cjs`

#### Pattern 6: Polyglot CMD/Bash Wrapper
The hook system uses a single file that works as both a Windows CMD batch script and a Unix bash script.

**Location**: `hooks/run-hook.cmd`
```bash
: << 'CMDBLOCK'
@echo off
REM Windows batch code here...
exit /b 0
CMDBLOCK
# Unix bash code here...
exec bash "${SCRIPT_DIR}/${SCRIPT_NAME}" "$@"
```

#### Pattern 7: Platform-Aware JSON Output
Hooks detect the platform via environment variables and emit different JSON structures for each.

**Location**: `hooks/session-start`
```bash
if [ -n "${CURSOR_PLUGIN_ROOT:-}" ]; then
  # Cursor format: { "additional_context": "..." }
elif [ -n "${CLAUDE_PLUGIN_ROOT:-}" ]; then
  # Claude Code format: { "hookSpecificOutput": { ... } }
else
  # Fallback: { "additional_context": "..." }
fi
```

#### Pattern 8: Graphviz DOT Flowcharts in Skills
Skills use DOT syntax in fenced code blocks for process diagrams — only for non-obvious decision points, never for linear sequences.

**Location**: Most `SKILL.md` files (brainstorming, subagent-driven-development, etc.)

### Anti-Patterns to Avoid

| Anti-Pattern | Why | What to Do Instead |
|---|---|---|
| External dependencies in production | Plugin must work with zero npm install | Use Node.js built-ins only |
| Test frameworks (Jest/Mocha) | Adds dependencies, overkill for this codebase | Use `assert` module + custom test runner |
| `#!/bin/bash` shebang | Not portable (NixOS, FreeBSD) | Use `#!/usr/bin/env bash` |
| Skill description summarizing workflow | Agents follow the shortcut instead of reading the full body | Description must ONLY describe triggering conditions |
| `@` file links in skills | Force-loads files, burns context | Use `superpowers:<skill-name>` cross-references |
| File path references to skills | Fragile, platform-dependent | Use `superpowers:<skill-name>` cross-references |
| Putting secrets in committed files | Security risk | `.gitignore` covers `.claude/`, `node_modules/`, etc. |
| Bundled unrelated changes in PRs | Violates PR template requirements | One logical change per PR |
| Modifying behavior-shaping content without evals | Skill wording is "code" — changes have side effects | Run adversarial pressure tests, show before/after |
| CommonJS `.js` files with `require()` | Root `package.json` has `"type": "module"` — Node.js 22+ fails | Use `.cjs` extension for CommonJS files |

## Error Handling

### Shell Scripts
- `set -euo pipefail` (or `set -e` minimum) at top of all scripts
- Trap cleanup on EXIT to handle errors gracefully
- Exit code 1 on test failure, 0 on success
- `2>/dev/null` for commands that may fail in normal operation (e.g., `kill`, `wait`)
- `|| true` suffix when failure is acceptable (e.g., cleanup operations)
- Timeout wrapping for long-running operations: `timeout 300 command ...`

### JavaScript
- Server code: graceful error handling without crashing (e.g., malformed WebSocket JSON)
- Tests: `assert` module throws on failure, caught by test runner
- Module loading: try/catch with helpful error message for TDD (tests before implementation)
```javascript
try {
  ws = require(SERVER_PATH);
} catch (e) {
  console.error(`Cannot load ${SERVER_PATH}: ${e.message}`);
  console.error('This is expected if running tests before implementation.');
  process.exit(1);
}
```

### Logging Patterns
- Test output format: `[PASS]` / `[FAIL]` (bash) or `PASS:` / `FAIL:` (JS) prefixes
- Server: JSON-structured stdout for machine-readable events (`{"type": "server-started", ...}`)
- Structured stdout events: `screen-added`, `screen-updated`, `user-event`
- Summary line at end of test runs: `Results: X passed, Y failed`
- Exit code communicates success/failure to callers

## Documentation

### Skill Documentation
Every skill has a `SKILL.md` with consistent structure:
1. YAML frontmatter (`name`, `description`)
2. `# Skill Name` heading
3. `## Overview` — core principle in 1-2 sentences
4. `## When to Use` — triggering conditions, symptoms
5. `## Core Pattern / The Process` — main content
6. `## Common Mistakes / Red Flags` — what goes wrong
7. `## Common Rationalizations` (for discipline skills) — excuse/reality table

### Inline Comment Style
- Shell: `# Comment` with explanation of non-obvious logic
- JavaScript: `// Comment` for single-line, `/** JSDoc */` for file headers and function docs
- Markdown: `<!-- HTML comments -->` for template instructions (PR template, issue templates)

### File-Level Documentation
- JS test files have JSDoc-style block comments at top explaining what's tested and what APIs are exercised
- Shell test files have header comments explaining purpose, requirements, and usage

### Changelog & Release Notes
- `CHANGELOG.md`: Brief, `## [version] - date` format with `### Fixed/Changed/Added` sections
- `RELEASE-NOTES.md`: Detailed per-version narrative with PR/issue links
- Both reference PR numbers (`[PR #NNN]`) and issue numbers (`[#NNN]`)

### Plan/Spec Documents
- Plans: `docs/superpowers/plans/YYYY-MM-DD-<topic>.md`
- Specs: `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
- Committed to git after creation

### Token Efficiency
Skills target word counts to minimize context consumption:
- Getting-started skills: <150 words
- Frequently-loaded skills: <200 words
- Other skills: <500 words
- Move details to tool help rather than inline
- Use cross-references instead of repeating content

## Git Conventions

### Commit Message Format
Descriptive messages, no strict conventional-commits format. Common patterns:
```
fix: Windows brainstorm server lifecycle, restore execution choice
fix : resolve ESM/CommonJS module conflict in brainstorming server
Add PR template to filter low-quality submissions
Credit @user for Windows brainstorm docs (PR #768)
Release v5.0.5: brainstorm server ESM fix, Windows PID fix, stop-server reliability
```
- Some use `fix:` / `feat:` prefixes, but not consistently enforced
- Reference issue/PR numbers when applicable
- Release commits summarize all changes

### Branch Strategy
- `main` branch for releases
- `dev` branch for integration
- Feature branches merged into `dev`, then `dev` merged into `main` for releases
- Worktrees recommended for parallel development (skill: `using-git-worktrees`)

### PR Process
Rigorous PR template with mandatory sections:
1. Problem statement (not just "improving")
2. What changed (1-3 sentences)
3. Appropriateness for core library
4. Alternatives considered
5. No bundled unrelated changes
6. Duplicate/prior-art check (open AND closed PRs)
7. Environment tested (harness, version, model)
8. Evaluation evidence (before/after across sessions)
9. Skills changes require `writing-skills` with adversarial pressure testing
10. Human must review complete diff before submission

PRs closed without review if they: show no human involvement, bundle unrelated changes, promote third-party services, submit project-specific config, leave sections blank, or modify behavior-shaping content without eval evidence.

## Cross-Platform Considerations

### Line Endings
All text files enforce LF via `.gitattributes`:
- `*.sh`, `*.md`, `*.json`, `*.js`, `*.mjs`, `*.ts`, `*.cmd` -> `text eol=lf`
- `hooks/session-start` (extensionless) -> `text eol=lf`
- Binary files (`.png`, `.jpg`, `.gif`) -> `binary`

### Hook Execution
- `run-hook.cmd` is a polyglot CMD/Bash wrapper for cross-platform hook execution
- Hooks use extensionless filenames (`session-start` not `session-start.sh`) to avoid Claude Code's Windows auto-detection interference
- Windows: tries Git for Windows bash, then PATH bash, then exits silently
- Unix: direct bash execution via `exec`
- Uses `printf` instead of heredoc for large content to work around bash 5.3+ heredoc bug

### Platform Detection
- Environment variables: `CLAUDE_PLUGIN_ROOT`, `CURSOR_PLUGIN_ROOT`, `OSTYPE`, `MSYSTEM`
- Windows/MSYS2 detection: `case "${OSTYPE:-}" in msys*|cygwin*|mingw*)`
- Different JSON output format per platform (Claude Code vs Cursor)
- OWNER_PID lifecycle monitoring disabled on Windows (MSYS2 PID namespace invisible to Node.js)

### Module System
- Root `package.json` declares `"type": "module"` (ESM)
- Server code uses `.cjs` extension to opt into CommonJS (`require()`)
- OpenCode plugin uses `.js` with `import`/`export` (ESM)
- This split was a bug fix: `.js` files with `require()` broke on Node.js 22+
