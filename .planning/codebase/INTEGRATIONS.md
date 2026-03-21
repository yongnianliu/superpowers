# External Integrations and APIs - Superpowers

## APIs & Services

| Service | Purpose | Config Location |
|---------|---------|----------------|
| Claude Code Plugin System | Primary distribution -- hooks, skills, agents, commands | `.claude-plugin/plugin.json`, `hooks/hooks.json` |
| Claude Code Marketplace | Official + self-hosted marketplace distribution | `.claude-plugin/marketplace.json` |
| Cursor Plugin System | Distribution for Cursor IDE | `.cursor-plugin/plugin.json`, `hooks/hooks-cursor.json` |
| OpenCode Plugin System | Distribution for OpenCode.ai | `.opencode/plugins/superpowers.js` |
| Codex Skill Discovery | Distribution for Codex CLI (symlink-based) | `.codex/INSTALL.md` |
| Gemini CLI Extensions | Distribution for Gemini CLI | `gemini-extension.json`, `GEMINI.md` |
| GitHub | Source hosting, issues, PRs, sponsorship | `.github/FUNDING.yml`, `.github/ISSUE_TEMPLATE/config.yml`, `.github/PULL_REQUEST_TEMPLATE.md` |

## AI Coding Agent Platform Details

### Claude Code
- **Integration type:** Official plugin marketplace + self-hosted marketplace
- **Plugin manifest:** `.claude-plugin/plugin.json` (v5.0.5)
- **Marketplace manifest:** `.claude-plugin/marketplace.json`
- **Hook system:** `hooks/hooks.json` -- `SessionStart` hook on `startup|clear|compact` events
- **Hook execution:** `run-hook.cmd session-start` -> `hooks/session-start` bash script
- **Output format:** `hookSpecificOutput.additionalContext` JSON field
- **Installation:** `/plugin install superpowers@claude-plugins-official` or via self-hosted marketplace

### Cursor
- **Integration type:** Plugin marketplace
- **Plugin manifest:** `.cursor-plugin/plugin.json` (v5.0.5)
- **Hook system:** `hooks/hooks-cursor.json` -- `sessionStart` hook
- **Output format:** `additional_context` JSON field
- **Installation:** `/add-plugin superpowers` in Agent chat

### OpenCode
- **Integration type:** JavaScript ESM plugin
- **Entry point:** `.opencode/plugins/superpowers.js`
- **Config hook:** Injects skills directory path into `config.skills.paths`
- **System prompt hook:** `experimental.chat.system.transform` injects bootstrap context
- **Installation:** Add `superpowers@git+https://github.com/obra/superpowers.git` to `opencode.json` plugin array

### Codex
- **Integration type:** Symlink-based skill discovery
- **Discovery:** Skills symlinked into `~/.agents/skills/superpowers`
- **Installation:** Clone repo + create symlink (manual)

### Gemini CLI
- **Integration type:** Gemini extension
- **Manifest:** `gemini-extension.json` -- `contextFileName: "GEMINI.md"`
- **Context file:** `GEMINI.md` -- uses `@` includes to reference skill files and gemini-tools reference
- **Installation:** `gemini extensions install https://github.com/obra/superpowers`

## Authentication & Auth
- **No authentication** -- Superpowers has no auth, API keys, or secrets
- All platform integrations use the host platform's existing auth
- No tokens or session management

## Data Storage
- **No databases** -- the project is stateless
- **File-based state (brainstorm server only):**
  - `BRAINSTORM_DIR` (default `/tmp/brainstorm/`) -- temporary directory for HTML screen files
  - `.server-info` -- JSON with port, host, URL, screen_dir (written on server start)
  - `.server-stopped` -- JSON with reason + timestamp (written on shutdown)
  - `.events` -- JSONL append-only file for user choice events from WebSocket
  - `.server.pid` -- PID file for process management
  - `.server.log` -- Server stdout/stderr
- **No cache layers**
- **No persistent file storage**

## Third-Party SDKs
| SDK | Purpose | Usage Locations |
|-----|---------|----------------|
| `ws` ^8.19.0 (npm) | WebSocket client for testing | `tests/brainstorm-server/server.test.js`, `tests/brainstorm-server/ws-protocol.test.js` |
| Graphviz `dot` (optional) | Render DOT diagrams to SVG | `skills/writing-skills/render-graphs.js` |

> Production code has **zero external dependencies**. The brainstorm server implements WebSocket (RFC 6455) from scratch using Node.js `crypto` built-in.

## Network Services

### Brainstorm Companion Server
- **Protocol:** HTTP + WebSocket (RFC 6455, hand-rolled implementation)
- **Binding:** `127.0.0.1` by default (configurable via `BRAINSTORM_HOST`)
- **Port:** Random high port (49152-65534) or `BRAINSTORM_PORT` env var
- **Purpose:** Serves visual brainstorming UI to local browser; receives user interaction events (clicks, choices) via WebSocket
- **Lifecycle:** Auto-exits on 30-min idle timeout or when owner process (`BRAINSTORM_OWNER_PID`) dies
- **No external network calls** -- purely local loopback

## Environment Configuration

### Required Environment Variables
| Variable | Set By | Purpose |
|----------|--------|---------|
| `CLAUDE_PLUGIN_ROOT` | Claude Code | Plugin root directory (used by hooks to detect platform) |
| `CURSOR_PLUGIN_ROOT` | Cursor | Plugin root directory (used by hooks to detect platform) |

### Optional Environment Variables
| Variable | Purpose | Default |
|----------|---------|---------|
| `OPENCODE_CONFIG_DIR` | Override OpenCode config directory | `~/.config/opencode` |
| `CODEX_CI` | Codex CI environment detection | _(unset)_ |
| `BRAINSTORM_PORT` | Override brainstorm server port | Random 49152-65535 |
| `BRAINSTORM_HOST` | Override brainstorm server bind address | `127.0.0.1` |
| `BRAINSTORM_URL_HOST` | Override brainstorm server display hostname | `localhost` |
| `BRAINSTORM_DIR` | Override brainstorm screen directory | `/tmp/brainstorm` |
| `BRAINSTORM_OWNER_PID` | Parent process PID for auto-shutdown | _(none)_ |

### Config Files
| File | Format | Purpose |
|------|--------|---------|
| `package.json` | JSON | npm package metadata (name: superpowers, v5.0.4, ESM) |
| `.claude-plugin/plugin.json` | JSON | Claude Code plugin manifest (v5.0.5) |
| `.claude-plugin/marketplace.json` | JSON | Claude Code marketplace listing |
| `.cursor-plugin/plugin.json` | JSON | Cursor plugin manifest (v5.0.5) |
| `hooks/hooks.json` | JSON | Claude Code hook definitions (SessionStart) |
| `hooks/hooks-cursor.json` | JSON | Cursor hook definitions (sessionStart) |
| `gemini-extension.json` | JSON | Gemini CLI extension manifest (v5.0.0) |

### Feature Flags
- **No feature flags** -- no feature flag system exists
- Skills are enabled/disabled by their presence in the `skills/` directory
- Legacy migration detection: `hooks/session-start` checks for `~/.config/superpowers/skills` and warns users to migrate

## External URLs Referenced
| URL | Purpose |
|-----|---------|
| `https://github.com/obra/superpowers` | Source repository |
| `https://github.com/obra/superpowers-marketplace` | Self-hosted Claude Code marketplace |
| `https://github.com/sponsors/obra` | GitHub Sponsors funding |
| `https://blog.fsck.com/2025/10/09/superpowers/` | Project blog post |
| `https://discord.gg/Jd8Vphy9jq` | Community Discord |
| `https://primeradiant.com` | Company website (Prime Radiant) |
| `https://claude.com/plugins/superpowers` | Official Claude plugin marketplace page |
