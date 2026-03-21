# Testing

## Overview

Superpowers has a multi-layered testing approach that reflects its unique nature as an AI-agent behavior-shaping system. Testing spans from traditional unit/integration tests for infrastructure code to novel "pressure testing" methodologies for validating that skills actually change agent behavior.

**No CI/CD pipeline exists** — there are no GitHub Actions workflows. All tests are run manually. The project relies on human review, adversarial testing, and the PR template's rigor requirements as quality gates.

## Test Framework

- **No test framework** — no Jest, Mocha, tap, or any test library
- **Node.js `assert` module** for JavaScript test assertions
- **Custom inline test runners** in both JavaScript and Bash
- **`ws` npm package** — sole test-only dependency (WebSocket client for brainstorm server tests)
- **Claude Code CLI** (`claude -p`) — used as a test harness for skill behavior tests
- **Python 3** — token usage analysis tool only

## Test Organization

```
tests/
├── brainstorm-server/                  # Unit + integration tests for brainstorm server
│   ├── package.json                    # Test-only dependency: ws (WebSocket client)
│   ├── server.test.js                  # HTTP/WebSocket/file-watching integration tests
│   ├── ws-protocol.test.js             # WebSocket frame encoding/decoding unit tests
│   └── windows-lifecycle.test.sh       # Windows-specific server lifecycle tests
├── claude-code/                        # Skill behavior tests via Claude Code CLI
│   ├── README.md                       # Test documentation
│   ├── test-helpers.sh                 # Shared assertion library (assert_contains, assert_order, etc.)
│   ├── run-skill-tests.sh             # Test runner with --verbose, --integration, --test flags
│   ├── test-subagent-driven-development.sh          # Fast skill content tests
│   ├── test-subagent-driven-development-integration.sh  # Full workflow (10-30 min)
│   ├── test-document-review-system.sh  # Document review skill tests
│   └── analyze-token-usage.py          # Token cost analysis for sessions
├── explicit-skill-requests/            # Tests that agents invoke skills when named
│   ├── prompts/                        # Natural language test prompts (9 txt files)
│   ├── run-test.sh                     # Single test runner with premature-action detection
│   ├── run-all.sh                      # Batch runner
│   ├── run-multiturn-test.sh           # Multi-turn conversation tests
│   ├── run-extended-multiturn-test.sh  # Extended multi-turn tests
│   └── run-haiku-test.sh              # Model-specific tests
├── skill-triggering/                   # Tests that skills trigger from naive prompts
│   ├── prompts/                        # Prompts that don't name skills explicitly (6 txt files)
│   ├── run-test.sh                     # Single test runner
│   └── run-all.sh                      # Batch runner for all skills
├── subagent-driven-dev/                # End-to-end SDD workflow test fixtures
│   ├── go-fractals/                    # Go test project (design, plan, scaffold)
│   ├── svelte-todo/                    # Svelte test project (design, plan, scaffold)
│   └── run-test.sh                     # Test runner
└── opencode/                           # OpenCode plugin integration tests
    ├── setup.sh                        # Environment setup
    ├── run-tests.sh                    # Test runner with --verbose, --integration, --test flags
    ├── test-plugin-loading.sh          # Plugin structure verification (fast)
    ├── test-tools.sh                   # use_skill/find_skills tool tests (integration)
    └── test-priority.sh                # Skill priority resolution tests (integration)
```

## Test Types

| Type | Location | Runner | Technology | Duration | Coverage |
|------|----------|--------|------------|----------|----------|
| Unit | `tests/brainstorm-server/ws-protocol.test.js` | `node` | `assert` module | ~1s | WebSocket frame encode/decode, RFC 6455 handshake |
| Integration (server) | `tests/brainstorm-server/server.test.js` | `node` | `assert` + `ws` | ~10s | HTTP serving, WebSocket relay, file watching |
| Platform lifecycle | `tests/brainstorm-server/windows-lifecycle.test.sh` | `bash` | Process management | ~2.5 min | Windows PID handling, server survival, shutdown |
| Skill content (fast) | `tests/claude-code/test-*.sh` | `bash` + `claude -p` | Headless Claude CLI | ~2 min | Skill loading, content verification |
| Skill workflow (slow) | `tests/claude-code/*-integration.sh` | `bash` + `claude -p` | Headless Claude CLI + JSONL parsing | 10-30 min | Full end-to-end SDD workflow |
| Skill triggering | `tests/skill-triggering/` | `bash` + `claude -p` | `stream-json` output grep | ~5 min each | 6 skills auto-trigger from naive prompts |
| Explicit requests | `tests/explicit-skill-requests/` | `bash` + `claude -p` | `stream-json` output grep | ~5 min each | Skill invocation when named + no premature action |
| Plugin loading | `tests/opencode/test-plugin-loading.sh` | `bash` | File/structure checks | ~1s | OpenCode plugin structure |
| Plugin integration | `tests/opencode/test-{tools,priority}.sh` | `bash` | OpenCode CLI | ~5 min each | Tool and priority resolution |
| Pressure testing | Manual (in-workflow) | Human + subagent | Adversarial prompting | Variable | Skill behavioral compliance |

## Running Tests

```bash
# === Brainstorm Server Tests ===

# Unit tests (WebSocket protocol)
cd tests/brainstorm-server && npm install && node ws-protocol.test.js

# Integration tests (full server behavior)
cd tests/brainstorm-server && npm install && node server.test.js

# Windows lifecycle tests (on Windows/MSYS2, ~2.5 min)
bash tests/brainstorm-server/windows-lifecycle.test.sh

# === Skill Behavior Tests (requires Claude CLI + plugin) ===

# Fast tests only (~2 min)
cd tests/claude-code && ./run-skill-tests.sh

# With integration tests (~10-30 min)
cd tests/claude-code && ./run-skill-tests.sh --integration

# Verbose output
cd tests/claude-code && ./run-skill-tests.sh --verbose

# Specific test only
cd tests/claude-code && ./run-skill-tests.sh --test test-subagent-driven-development.sh

# === Skill Triggering Tests (requires Claude CLI + plugin) ===

# All skills
cd tests/skill-triggering && ./run-all.sh

# Single skill
cd tests/skill-triggering && ./run-test.sh systematic-debugging prompts/systematic-debugging.txt

# === Explicit Skill Request Tests ===

cd tests/explicit-skill-requests && ./run-test.sh subagent-driven-development prompts/subagent-driven-development-please.txt

# === OpenCode Plugin Tests ===

# Fast tests only
cd tests/opencode && ./run-tests.sh

# With integration tests (requires OpenCode)
cd tests/opencode && ./run-tests.sh --integration

# === Token Usage Analysis ===

python3 tests/claude-code/analyze-token-usage.py ~/.claude/projects/<dir>/<session>.jsonl
```

## Test Patterns

### Custom Test Runner Pattern

All test files use a hand-rolled test runner. No test framework.

**JavaScript pattern** (async-capable):
```javascript
function test(name, fn) {
  return fn().then(() => {
    console.log(`  PASS: ${name}`);
    passed++;
  }).catch(e => {
    console.log(`  FAIL: ${name}`);
    console.log(`    ${e.message}`);
    failed++;
  });
}
```

**JavaScript pattern** (synchronous):
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

**Bash pattern**:
```bash
pass() { echo "  PASS: $1"; passed=$((passed + 1)); }
fail() { echo "  FAIL: $1"; echo "    $2"; failed=$((failed + 1)); }
skip() { echo "  SKIP: $1 ($2)"; skipped=$((skipped + 1)); }
```

All test files end with a summary and exit code:
```javascript
console.log(`\n--- Results: ${passed} passed, ${failed} failed ---`);
if (failed > 0) process.exit(1);
```

### Assertion Library (Bash)

Shared helpers in `tests/claude-code/test-helpers.sh`:

| Function | Purpose |
|---|---|
| `run_claude "prompt" [timeout] [tools]` | Run Claude Code in headless mode |
| `assert_contains output pattern name` | Verify pattern is present |
| `assert_not_contains output pattern name` | Verify pattern is absent |
| `assert_count output pattern expected name` | Verify match count |
| `assert_order output pattern_a pattern_b name` | Verify A appears before B |
| `create_test_project` | Create temp directory |
| `cleanup_test_project dir` | Remove temp directory |
| `create_test_plan dir [name]` | Generate sample plan file |

### Mocking Strategy

- **No mocking framework** — tests use real components
- Server tests spawn actual Node.js server processes
- Claude CLI tests run actual Claude sessions in headless mode
- Windows tests use fake `node` scripts (simple bash scripts that capture env vars) to test argument passing without running the real server
- Test isolation via temp directories (`mktemp -d`, `/tmp/brainstorm-test`)

### Fixture Management

- **Test prompts**: Plain text files in `prompts/` subdirectories (one per test scenario)
- **Test projects**: Created dynamically via `create_test_plan` helper or inline `cat > file << 'EOF'`
- **Test project scaffolds**: Pre-built in `tests/subagent-driven-dev/{go-fractals,svelte-todo}/` with `design.md`, `plan.md`, `scaffold.sh`
- **Cleanup**: `trap cleanup EXIT` pattern ensures temp directories are removed

### Test Data Approach

- Brainstorm server tests: HTML files written to temp dirs during test execution
- WebSocket tests: Frames constructed programmatically with `makeClientFrame()` helper
- Skill tests: Minimal implementation plans with 2-3 simple tasks (e.g., "Create Hello Function", "Create Goodbye Function")
- Skill triggering tests: Natural language prompts stored as `.txt` files

### Session Transcript Parsing

Integration tests parse Claude Code's JSONL session transcripts to verify behavior:

```bash
# Check if a specific skill was invoked
grep -q '"name":"Skill".*"skill":"your-skill-name"' "$SESSION_FILE"

# Check stream-json output for skill invocation
SKILL_PATTERN='"skill":"([^"]*:)?'"${SKILL_NAME}"'"'
grep -q '"name":"Skill"' "$LOG_FILE" && grep -qE "$SKILL_PATTERN" "$LOG_FILE"

# Count subagent dispatches
grep -c '"name":"Task"' "$SESSION_FILE"
```

## Coverage

### Current Coverage (Conceptual — No Coverage Tooling)

There is no code coverage tool (Istanbul/c8/etc). Coverage is assessed conceptually:

| Layer | What's Tested | How | Gap Assessment |
|---|---|---|---|
| WebSocket protocol | Frame encode/decode, handshake, boundaries | Unit tests (`ws-protocol.test.js`) | **Good** — extensive boundary testing |
| Brainstorm server HTTP | Serving, fragments, frame wrapping, mtime ordering | Integration tests (`server.test.js`) | **Good** — 20+ test cases |
| Brainstorm server WebSocket | Connection, relay, events, multi-client, malformed input | Integration tests (`server.test.js`) | **Good** — error cases covered |
| Server lifecycle | PID handling, 60s survival, clean shutdown | Platform tests (`windows-lifecycle.test.sh`) | **Good** — Windows + control tests |
| Skill loading | Skills trigger from prompts | CLI headless tests | **Partial** — 6 of 14 skills have triggering tests |
| Skill compliance | Agents follow instructions | Full integration tests | **Limited** — only SDD has full integration test |
| Skill quality | Behavior under pressure | Manual subagent pressure testing | **By convention** — not automated |
| Hook execution | Session start context injection | No tests | **Gap** — manual testing only |
| OpenCode plugin | Plugin loading, tool dispatch, priority | Bash tests (fast + integration) | **Partial** — structure test is fast, tool tests need OpenCode |
| Cursor plugin | Plugin loading | No tests | **Gap** — manual testing only |
| Cross-platform hooks | CMD/Bash polyglot wrapper | No tests | **Gap** — manual testing only |
| `helper.js` (client-side) | WebSocket connection, event dispatch | Static content checks only | **Gap** — no browser tests |
| `start-server.sh` | Argument parsing, background/foreground modes | Partial (Windows fake-node test) | **Gap** — limited coverage of argument combinations |

### Coverage Requirements

No formal coverage thresholds. Quality gates are:

1. **PR template**: Human reviews complete diff, evaluation evidence required
2. **Skills changes**: Must use `writing-skills` with adversarial pressure testing results
3. **Infrastructure changes**: Must demonstrate before/after behavior across multiple sessions
4. **The Iron Law** (from `writing-skills`): No skill without a failing test first

### Gaps in Coverage

1. **No CI/CD** — tests are never run automatically; relies entirely on manual discipline
2. **No browser testing** — `helper.js` client-side code has no automated tests
3. **No hook tests** — `session-start` and `run-hook.cmd` are tested manually only
4. **Limited skill integration tests** — only `subagent-driven-development` has full end-to-end workflow coverage
5. **8 of 14 skills** lack automated triggering tests (skill-triggering covers 6)
6. **No cross-platform CI** — Windows lifecycle test exists but must be run manually on Windows
7. **No regression testing** — no mechanism to detect if a skill change breaks behavior in prior scenarios
8. **Pressure testing is manual** — the most important quality signal (skill behavioral compliance) depends on human discipline to execute

## Quality Gates

### PR-Level Gates (Manual)
1. Human reviews complete diff before submission
2. Skills changes require `writing-skills` skill usage with adversarial pressure testing results
3. Multi-session evaluation required (before/after comparison)
4. Environment must be documented (harness, version, model)
5. No bundled unrelated changes
6. Duplicate/prior-art check on open AND closed PRs/issues

### Skill-Level Quality (from `writing-skills` skill)
- **The Iron Law**: No skill without a failing test first (RED-GREEN-REFACTOR for docs)
- **Rationalization table**: Every agent excuse captured and countered
- **Red flags list**: Self-check triggers for common violations
- **CSO (Claude Search Optimization)**: Description must be triggering-conditions-only
- **Token efficiency**: Word count targets enforced
- **Deployment checklist**: Mandatory per-skill verification before deployment

### Pressure Testing Methodology

The most novel aspect of this project's testing. Defined in `writing-skills` skill:

**RED phase** (baseline): Run pressure scenarios WITHOUT the skill, document agent failures and rationalizations verbatim.

**GREEN phase**: Write skill addressing specific failures, verify agents now comply.

**REFACTOR phase**: Find new rationalizations, add explicit counters, re-test until bulletproof.

**Pressure types** (best tests combine 3+):

| Type | Example |
|---|---|
| Time | Emergency, deadline, deploy window |
| Sunk cost | Hours of work already done |
| Authority | Senior says skip it |
| Economic | Job/promotion at stake |
| Exhaustion | End of day, tired |
| Social | Looking dogmatic |
| Pragmatic | "Being pragmatic vs dogmatic" |

## CI/CD

**There is no automated CI/CD pipeline.** The `.github/` directory contains:
- `FUNDING.yml` — GitHub Sponsors config (obra)
- `ISSUE_TEMPLATE/` — Bug report, feature request, platform support templates (blank issues disabled)
- `PULL_REQUEST_TEMPLATE.md` — Rigorous PR checklist

No GitHub Actions, no automated test runs on push/PR, no deployment pipeline. Quality enforcement is through the PR template requirements, manual testing, and the skill-based workflow discipline.

## Key Dependencies for Testing

| Dependency | Purpose | Scope |
|---|---|---|
| Node.js | Run brainstorm server tests | Unit + integration |
| `ws` npm package | WebSocket test client | `tests/brainstorm-server/` only |
| Claude Code CLI (`claude`) | Headless skill testing | `tests/claude-code/`, `tests/skill-triggering/`, `tests/explicit-skill-requests/` |
| `jq` | JSON parsing in test scripts | Optional, for debug output |
| Python 3 | Token usage analysis | `tests/claude-code/analyze-token-usage.py` |
| bash | All test scripts | Universal |
| OpenCode | OpenCode integration tests | `tests/opencode/` (integration only) |
