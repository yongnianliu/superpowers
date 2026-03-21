# Structure - Superpowers

## Directory Layout

```
superpowers/                          # Project root (v5.0.4/5.0.5)
│
├── skills/                           # Core product — skill documents (Markdown)
│   ├── brainstorming/                #   Socratic design refinement
│   │   ├── SKILL.md                  #     Main skill document
│   │   ├── visual-companion.md       #     Browser-based mockup companion guide
│   │   ├── spec-document-reviewer-prompt.md  # Subagent prompt template
│   │   └── scripts/                  #     Brainstorm companion server
│   │       ├── server.cjs            #       Zero-dep HTTP+WS server (CJS)
│   │       ├── start-server.sh       #       Server launch script
│   │       ├── stop-server.sh        #       Server shutdown script
│   │       ├── frame-template.html   #       HTML frame for content fragments
│   │       └── helper.js             #       Client-side helper script
│   ├── subagent-driven-development/  #   Primary plan execution engine
│   │   ├── SKILL.md                  #     Main skill document
│   │   ├── implementer-prompt.md     #     Subagent prompt: implementer
│   │   ├── spec-reviewer-prompt.md   #     Subagent prompt: spec compliance
│   │   └── code-quality-reviewer-prompt.md  # Subagent prompt: quality review
│   ├── writing-plans/                #   Creates implementation plans
│   │   ├── SKILL.md
│   │   └── plan-document-reviewer-prompt.md
│   ├── executing-plans/              #   Alternative plan execution (no subagents)
│   │   └── SKILL.md
│   ├── test-driven-development/      #   TDD enforcement
│   │   ├── SKILL.md
│   │   └── testing-anti-patterns.md
│   ├── systematic-debugging/         #   4-phase root cause debugging
│   │   ├── SKILL.md
│   │   ├── root-cause-tracing.md
│   │   ├── defense-in-depth.md
│   │   ├── condition-based-waiting.md
│   │   ├── condition-based-waiting-example.ts
│   │   ├── find-polluter.sh
│   │   ├── CREATION-LOG.md
│   │   └── test-*.md                 #   Pressure test scenarios (3 files)
│   ├── using-superpowers/            #   Root skill — loaded at session start
│   │   ├── SKILL.md
│   │   └── references/
│   │       ├── gemini-tools.md       #     Tool mapping for Gemini CLI
│   │       └── codex-tools.md        #     Tool mapping for Codex
│   ├── using-git-worktrees/          #   Isolated workspace setup
│   │   └── SKILL.md
│   ├── requesting-code-review/       #   Pre-review dispatch
│   │   ├── SKILL.md
│   │   └── code-reviewer.md          #   Reviewer subagent prompt template
│   ├── receiving-code-review/        #   Responding to feedback
│   │   └── SKILL.md
│   ├── finishing-a-development-branch/  # Merge/PR/cleanup workflow
│   │   └── SKILL.md
│   ├── dispatching-parallel-agents/  #   Concurrent subagent dispatch
│   │   └── SKILL.md
│   ├── verification-before-completion/  # Evidence-before-claims enforcement
│   │   └── SKILL.md
│   ├── writing-skills/               #   Meta-skill: creating new skills (TDD-based)
│   │   ├── SKILL.md
│   │   ├── anthropic-best-practices.md
│   │   ├── persuasion-principles.md
│   │   ├── testing-skills-with-subagents.md
│   │   ├── graphviz-conventions.dot
│   │   ├── render-graphs.js
│   │   └── examples/
│   │       └── CLAUDE_MD_TESTING.md
│
├── agents/                           # Reusable agent persona definitions
│   └── code-reviewer.md             #   Senior code reviewer agent
│
├── commands/                         # Deprecated slash commands (redirect to skills)
│   ├── brainstorm.md
│   ├── write-plan.md
│   └── execute-plan.md
│
├── hooks/                            # Session lifecycle hooks
│   ├── hooks.json                    #   Claude Code hook config (SessionStart)
│   ├── hooks-cursor.json             #   Cursor hook config (sessionStart)
│   ├── session-start                 #   Bootstrap injection script (Bash, extensionless)
│   └── run-hook.cmd                  #   Polyglot wrapper (Batch+Bash) for Windows
│
├── docs/                             # Documentation and project artifacts
│   ├── README.codex.md               #   Codex-specific docs
│   ├── README.opencode.md            #   OpenCode-specific docs
│   ├── testing.md                    #   Testing documentation
│   ├── windows/
│   │   └── polyglot-hooks.md         #   Windows hook implementation docs
│   ├── plans/                        #   Historical/legacy plans
│   │   ├── 2025-11-22-opencode-support-*.md
│   │   ├── 2025-11-28-skills-improvements-from-user-feedback.md
│   │   └── 2026-01-17-visual-brainstorming.md
│   └── superpowers/                  #   Active specs and plans (convention for user projects too)
│       ├── specs/                    #     Design specifications
│       │   ├── 2026-01-22-document-review-system-design.md
│       │   ├── 2026-02-19-visual-brainstorming-refactor-design.md
│       │   └── 2026-03-11-zero-dep-brainstorm-server-design.md
│       └── plans/                    #     Implementation plans
│           ├── 2026-01-22-document-review-system.md
│           ├── 2026-02-19-visual-brainstorming-refactor.md
│           └── 2026-03-11-zero-dep-brainstorm-server.md
│
├── tests/                            # Test suites
│   ├── brainstorm-server/            #   Server unit tests (Node.js + ws)
│   │   ├── server.test.js
│   │   ├── ws-protocol.test.js
│   │   ├── windows-lifecycle.test.sh
│   │   ├── package.json              #   Test-only dependency: ws ^8.19.0
│   │   └── package-lock.json
│   ├── claude-code/                  #   Claude Code integration tests
│   │   ├── run-skill-tests.sh
│   │   ├── test-document-review-system.sh
│   │   ├── test-subagent-driven-development*.sh
│   │   ├── test-helpers.sh
│   │   ├── analyze-token-usage.py
│   │   └── README.md
│   ├── explicit-skill-requests/      #   Tests for explicit skill invocation
│   │   ├── run-test.sh / run-all.sh
│   │   ├── run-*-test.sh             #   Specialized test runners
│   │   └── prompts/                  #     Test prompt files (.txt)
│   ├── skill-triggering/             #   Tests for automatic skill triggering
│   │   ├── run-test.sh / run-all.sh
│   │   └── prompts/                  #     Test prompt files (.txt)
│   ├── subagent-driven-dev/          #   SDD integration tests
│   │   ├── run-test.sh
│   │   ├── go-fractals/              #     Test project: Go fractals
│   │   └── svelte-todo/              #     Test project: Svelte todo app
│   └── opencode/                     #   OpenCode integration tests
│       ├── run-tests.sh / setup.sh
│       └── test-*.sh
│
├── .claude-plugin/                   # Claude Code plugin integration
│   ├── plugin.json                   #   Plugin manifest (name, version, author, license)
│   └── marketplace.json              #   Dev marketplace manifest
│
├── .cursor-plugin/                   # Cursor plugin integration
│   └── plugin.json                   #   Plugin manifest (skills, agents, commands, hooks refs)
│
├── .opencode/                        # OpenCode plugin integration
│   ├── INSTALL.md                    #   Installation guide
│   └── plugins/
│       └── superpowers.js            #   ESM plugin (config hook + system prompt transform)
│
├── .codex/                           # Codex integration
│   └── INSTALL.md                    #   Installation guide (symlink-based)
│
├── .github/                          # GitHub configuration
│   ├── FUNDING.yml
│   ├── PULL_REQUEST_TEMPLATE.md
│   └── ISSUE_TEMPLATE/
│       ├── config.yml
│       ├── bug_report.md
│       ├── feature_request.md
│       └── platform_support.md
│
├── .planning/                        # Internal planning documents
│   └── codebase/
│       ├── ARCHITECTURE.md           #   This project's architecture
│       ├── STRUCTURE.md              #   This project's structure
│       ├── STACK.md                  #   Technology stack analysis
│       ├── CONCERNS.md               #   Known concerns
│       └── INTEGRATIONS.md           #   Integration details
│
├── package.json                      # Root package (name, version, ESM type, opencode main)
├── gemini-extension.json             # Gemini CLI extension manifest
├── GEMINI.md                         # Gemini CLI context file (@-includes skills)
├── README.md                         # Main documentation
├── CHANGELOG.md                      # Version changelog
├── RELEASE-NOTES.md                  # Detailed release notes
├── CODE_OF_CONDUCT.md
├── LICENSE                           # MIT
├── .gitignore
└── .gitattributes
```

## Naming Conventions

### Skills
- **Directory names**: lowercase, hyphenated, verb-gerund or descriptive (`brainstorming`, `test-driven-development`, `using-git-worktrees`)
- **Main file**: always `SKILL.md` (uppercase, required)
- **Supporting files**: lowercase-hyphenated descriptive names (`visual-companion.md`, `implementer-prompt.md`)
- **Prompt templates**: `*-prompt.md` suffix for subagent dispatch templates
- **References**: stored in `references/` subdirectory within a skill

### Frontmatter
- `name`: hyphenated, matches directory name
- `description`: starts with "Use when..." — triggering conditions only, never workflow summary

### Documents
- **Specs**: `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
- **Plans**: `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`
- **Date prefix**: ISO 8601 date for chronological ordering

### Platform Integration
- Hidden directories with platform name prefix (`.claude-plugin/`, `.cursor-plugin/`, `.opencode/`, `.codex/`)
- Plugin manifests always `plugin.json`
- Platform-specific install docs as `INSTALL.md`

### Hooks
- Hook scripts are **extensionless** (`session-start`, not `session-start.sh`) — prevents Claude Code's Windows auto-detection from prepending `bash`
- Hook configs: `hooks.json` (Claude Code), `hooks-cursor.json` (Cursor)

### Tests
- Test directories named by what they test (`brainstorm-server/`, `skill-triggering/`, `explicit-skill-requests/`)
- Test runners: `run-test.sh`, `run-all.sh`, `run-tests.sh`
- Test prompts: `.txt` files in `prompts/` subdirectories

## Module Boundaries

### Core Skills Module (`skills/`)
The primary module. Each skill directory is a self-contained unit:
- `SKILL.md` is the only required file
- Supporting files are private to the skill (not shared across skills)
- Skills reference each other by name (`superpowers:<skill-name>`), never by file path in cross-skill references
- Exception: `requesting-code-review/code-reviewer.md` is a template used by the `code-reviewer` agent

### Platform Integration Module (`.claude-plugin/`, `.cursor-plugin/`, `.opencode/`, `.codex/`, `hooks/`)
Handles per-platform concerns:
- Plugin manifests declare what the platform should load
- Hooks inject bootstrap context at session start
- Each platform has its own discovery mechanism:
  - **Claude Code**: native plugin system reads `skills/`, `agents/`, `commands/`, `hooks/hooks.json`
  - **Cursor**: reads `skills/`, `agents/`, `commands/`, `hooks/hooks-cursor.json`
  - **Gemini CLI**: `gemini-extension.json` + `GEMINI.md` with `@` includes
  - **Codex**: symlink `skills/` into `~/.agents/skills/superpowers`
  - **OpenCode**: JS plugin registers skills path via config hook

### Agent Definitions Module (`agents/`)
Reusable agent personas. Currently only `code-reviewer.md`. Referenced by skills via `superpowers:code-reviewer` type in Task tool dispatches.

### Commands Module (`commands/`)
Deprecated slash commands. All redirect to equivalent skills. Will be removed in next major release.

### Documentation Module (`docs/`)
- `docs/superpowers/specs/` — design specifications produced by brainstorming skill
- `docs/superpowers/plans/` — implementation plans produced by writing-plans skill
- `docs/plans/` — historical/legacy plans (older convention)
- Platform-specific READMEs for Codex and OpenCode

### Test Module (`tests/`)
- **Unit tests** (`brainstorm-server/`) — Node.js tests for the companion server, only module with npm dependencies (`ws`)
- **Integration tests** (`claude-code/`, `opencode/`) — shell scripts that test end-to-end flows
- **Behavioral tests** (`skill-triggering/`, `explicit-skill-requests/`) — test prompt files that verify agents trigger and follow skills correctly
- **Project tests** (`subagent-driven-dev/`) — scaffold test projects with design docs and plans for SDD testing

## File Organization Principles

1. **Skills are flat** — no nesting of skills within skills. All skills at `skills/<name>/SKILL.md`
2. **Supporting files stay with their skill** — prompt templates, references, scripts colocate with the skill that uses them
3. **Platform code is isolated** — each platform's integration lives in its own dotfile directory
4. **Convention over configuration** — `SKILL.md` is always the main file, `references/` is always the reference subdirectory
5. **No build step** — everything runs directly (Markdown is read as-is, JS runs directly, Bash scripts are executable)
6. **Artifact locations are prescriptive** — skills define where output goes (`docs/superpowers/specs/`, `docs/superpowers/plans/`) with user override allowed
