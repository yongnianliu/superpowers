# Concerns & Technical Debt

## Critical Issues
Issues that could cause failures or security problems.

| Issue | Severity | Location | Impact |
|-------|----------|----------|--------|
| Stale `server.js` reference in test | High | `tests/brainstorm-server/windows-lifecycle.test.sh:23` | Windows lifecycle test references `server.js` but actual file is `server.cjs` — test will always fail |
| Version mismatch across manifests | Medium | `package.json` (5.0.4), `gemini-extension.json` (5.0.0), plugins (5.0.5) | Users on different platforms get different version signals; could cause confusion with bug reports |
| No authentication on brainstorm server | Medium | `skills/brainstorming/scripts/server.cjs` | Any local process can connect/read/write; `--host 0.0.0.0` exposes to network with zero protection |
| `innerHTML` with user-derived content | Low | `skills/brainstorming/scripts/helper.js:57,59` | Minor XSS surface; `textContent` source is safe but pattern could be copied unsafely |

## Technical Debt
Known shortcuts or suboptimal implementations.

| Debt Item | Priority | Location | Effort to Fix |
|-----------|----------|----------|--------------|
| Deprecated commands with no removal timeline | Medium | `commands/brainstorm.md`, `execute-plan.md`, `write-plan.md` | Low — delete 3 files in next major |
| Synchronous file I/O in server request handlers | Low | `server.cjs` — `readFileSync` in `handleRequest`, `getNewestScreen` | Medium — convert to async/await |
| Hand-rolled test framework | Low | `tests/brainstorm-server/server.test.js`, `ws-protocol.test.js` | Medium — migrate to node:test or vitest |
| `sleep()`-based timing in tests | Medium | `tests/brainstorm-server/server.test.js` (many `await sleep(300-500)`) | Medium — replace with event-driven waits |
| Hardcoded test port 3334 | Low | `tests/brainstorm-server/server.test.js:19` | Low — use port 0 with dynamic allocation |
| No `engines` field in `package.json` | Low | `package.json` | Low — add `"engines": { "node": ">=14" }` |
| Stale plan documents reference `server.js` | Low | `docs/superpowers/plans/2026-03-11-*.md`, `docs/superpowers/specs/2026-03-11-*.md` | Low — update references to `server.cjs` |
| Module system inconsistency | Low | `render-graphs.js` uses CJS `require()` despite ESM `package.json` | Low — rename to `.cjs` or convert to ESM |

## Security Concerns

### Authentication/Authorization Gaps
- **Brainstorm server has zero auth.** Binds to `127.0.0.1` by default (good), but the `--host 0.0.0.0` flag opens it to the network with no auth, no origin checking on WebSocket upgrades, and no CORS restrictions. Any local process can read served content and inject WebSocket messages.
- **No CSP headers** on served HTML pages. If malicious content is written to the session directory, it executes freely in the browser context.

### Input Validation Issues
- **`/files/*` endpoint** uses `path.basename()` to prevent directory traversal — adequate but minimal. No allowlist of file extensions or size limits.
- **WebSocket message handler** (`handleMessage`) parses arbitrary JSON from clients and logs it to stdout with no sanitization. The `choice` field is appended directly to `.events` file.
- **Random port selection** uses `Math.random()` (not cryptographically secure) without checking port availability — theoretical collision risk.

### Secrets Management
- No secrets are stored or managed by this project (it's a skills/prompt library). No `.env` files, no credentials. This is clean.

### Dependency Vulnerabilities
- Zero runtime dependencies (by design). Only test dependency is `ws@^8.19.0` — well-maintained, no known vulnerabilities.

## Performance Concerns

### Known Bottlenecks
- **Synchronous I/O in HTTP handler:** `server.cjs` uses `readFileSync`, `readdirSync`, `statSync` in the request path. Under concurrent load this blocks the Node.js event loop. Acceptable for single-user local tool.
- **Session-start hook** reads a skill file from disk and performs multiple string substitutions on every session start/clear/compact event. Adds latency to session initialization.

### Scaling Limitations
- **Single-file server architecture** (338 lines) handles HTTP, WebSocket, file watching, and lifecycle in one file. Adding HTTPS, auth, or multi-session support would strain this design.
- **Session directory accumulation:** Persistent sessions under `.superpowers/brainstorm/` have no cleanup mechanism. Over time these grow unbounded.
- **`fs.watch` reliability:** Has well-documented cross-platform inconsistencies (especially on NFS, Docker volumes, some Linux configs). No fallback to polling is implemented. The 100ms debounce mitigates some issues.

### Resource Usage Issues
- **No memory limits** on WebSocket message handling. A malicious client could send large frames (up to 2^63 bytes per the protocol handler) and exhaust memory.
- **Buffer.concat in WebSocket** data handler creates new buffers on every chunk, which fragments memory under high throughput.

## Maintainability Concerns

### Complex/Fragile Code Areas
- **`hooks/run-hook.cmd`** — Polyglot batch/bash script. Fragile by nature; changes must be tested on both Windows CMD and Unix shells.
- **`hooks/session-start`** — Uses `printf` instead of heredoc due to a bash 5.3+ bug (documented in comments). This workaround makes the JSON output fragile; any unescaped character in skill content breaks the JSON.
- **WebSocket protocol implementation** in `server.cjs` — Custom RFC 6455 implementation. Correct per tests, but any edge case (fragmented frames, extensions, subprotocols) is unhandled.

### Missing Documentation
- No API documentation for the brainstorm server HTTP/WebSocket protocol
- No `CONTRIBUTING.md`
- `package.json` has no `scripts` section — no documented way to run tests
- Hook file distinction (`hooks.json` vs `hooks-cursor.json`) is undocumented
- No changelog entries linking version bumps across manifests

### Tight Coupling
- **Platform API dependencies:** The plugin depends on undocumented/experimental platform APIs:
  - Claude Code: `hookSpecificOutput.additionalContext` pattern
  - Cursor: `additional_context` pattern
  - OpenCode: `experimental.chat.system.transform` (literally prefixed "experimental")
  - Gemini: `contextFileName` in extension manifest
  - Changes to any platform API break the plugin silently.
- **Session-start hook** must understand the JSON output format expected by both Claude Code and Cursor, with platform detection via environment variables.

### Missing Tests
- No unit tests for `hooks/session-start` (bash script)
- No tests for `hooks/run-hook.cmd` polyglot behavior
- No tests for `.opencode/plugins/superpowers.js` plugin logic
- No tests for `skills/writing-skills/render-graphs.js`
- Skill markdown content is tested only via LLM-in-the-loop integration tests (non-deterministic)
- No CI pipeline definition visible in the repo (no `.github/workflows/`)

## Dependency Risks

### Minimal Dependencies (Strength)
Zero runtime dependencies. The only dependency is `ws@^8.19.0` in test-only `tests/brainstorm-server/package.json`. This is intentional and a significant strength — no supply chain risk.

### Node.js Version Requirements
No `.nvmrc` or `engines` field. The code uses features requiring Node 14+ minimum:
- `import.meta.url` (ESM, Node 12+)
- Optional chaining `?.` (Node 14+)
- `fs.rmSync` in tests (Node 14.14+)
- `BigInt` / `BigUInt64` (Node 10.3+)

### External Tool Dependencies
- `render-graphs.js` requires `graphviz` (system package) — fails with helpful error if missing
- Bash 4+ required for `hooks/session-start` (associative arrays, parameter substitution)
- Windows support requires Git for Windows bash — degrades silently without it

## Migration Needs

### Deprecated Commands
Three commands in `commands/` say "will be removed in the next major release" but have persisted through the v5.x series. Should be removed in v6.0 or the deprecation message updated.

### Stale File References
The `windows-lifecycle.test.sh` script references `server.js` (line 23) but the file was renamed to `server.cjs`. Multiple plan/spec documents in `docs/superpowers/` also reference the old name. These are documentation artifacts but the test reference is a functional bug.

### Version Synchronization
Need a mechanism (script or CI check) to keep version numbers in sync across:
- `package.json`
- `.claude-plugin/plugin.json`
- `.claude-plugin/marketplace.json`
- `.cursor-plugin/plugin.json`
- `gemini-extension.json`

### Platform API Stability
All four platform integrations depend on APIs that could change:
- Claude Code's hook system and `Skill` tool
- Cursor's plugin/hook format
- OpenCode's `experimental.*` plugin APIs (high risk — experimental by name)
- Gemini CLI's extension format

No abstraction layer exists — each platform integration is a separate bespoke implementation. A breaking change in any platform requires manual updates.
