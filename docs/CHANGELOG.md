# Changelog

All notable changes to Keepra are documented here.

---

## [1.0.3] — 2026-06-20

### Added
- **In-app back navigation** — full browser history stack via `pushState`. Pressing back within the app navigates between sections instead of leaving the page.
- **Exit confirmation modal** — pressing back on the home section (or Android hardware back) shows a "Leave Keepra?" dialog instead of immediately navigating away.
- **Claude.ai Remote MCP** — HTTP+SSE bridge (`keepra-mcp-http.js`) so Claude.ai web can connect via a Cloudflare tunnel without installing anything locally.
- **Per-item MCP key button** — a `🔑 Key` button on every item modal (Links, Vault, Tasks, Contacts, Notes) creates a dedicated scoped key for that specific item in one click.
- **Node.js prerequisite detection** — Keepra Desktop detects whether Node.js is installed at startup and shows a yellow banner when it is missing (required to run the MCP server).

### Changed
- **Electron X button now closes correctly** — `beforeunload` was silently blocking the window close in Electron. Fixed by detecting the Electron context via user agent and skipping the handler there.
- **Link Open button** no longer stretches full-width on a maximised window.
- **Landing page hero** overhauled: stat tiles use icon → label → value layout with six equal-width tiles; sidebar bottom items aligned correctly.

### SEO
- **Blog post URLs** cleaned: numeric prefixes removed from all 10 posts (`01-why-keepra-exists` → `why-keepra-exists`, etc.). Old URLs 301-redirect via `.htaccess`.
- **All page `<title>` tags** shortened to under 55 characters. MCP keyword added to homepage, blog, and help titles.
- **MCP Connector H2** on the landing page updated to "MCP Connector — Let Claude & AI access your private data".
- **`blog.html`** `<meta name="keywords">` added.
- **Homepage** canonical, Open Graph, schema.org JSON-LD completed (was missing entirely).

---

## [1.0.2] — 2026-06-19

### Added
- **Per-item MCP grants** — each link, vault credential, task, contact, and note can individually grant AI access via a robot checkbox in its edit modal. Only items you explicitly unlock are visible to the AI.
- **MCP key auto-rotation** — keys now support automatic rotation on a schedule: Never / 1 h / 6 h / 12 h / 24 h / 7 d / 30 d. Set it in Settings → MCP → edit a key.
- **`run_command` tool** — opt-in shell command execution with vault secrets injected as env vars. The AI never sees secret values; Keepra injects them into the child process. Enabled per key with an explicit checkbox.
- **FTP tools** — six new MCP tools (`ftp_list`, `ftp_upload`, `ftp_download`, `ftp_delete`, `ftp_mkdir`, `ftp_rename`) that operate using FTP credentials stored in your vault. No credentials are ever exposed to the AI.
- **`run_command` audit log** — every shell command executed through MCP is logged to `%APPDATA%\keepra\keepra-mcp-audit.log` with key prefix, command, directory, and exit code.
- **ChatGPT (Codex CLI) and Perplexity** setup instructions added to the MCP settings view.

### Changed
- **`run_command` confirmation dialog** now uses an in-app styled overlay instead of a native OS dialog. Includes a "Trust this key for the session" checkbox — approve once and subsequent commands from the same key run silently until Keepra restarts.
- **MCP rate limit** capped at 60 requests per minute per key.
- **Sync passphrase minimum** raised to 8 characters.

### Security
- **XSS fix** — note tag `onclick` handlers now use HTML-escaped values.
- **Markdown RCE fix** — HTML inside code fences is no longer un-escaped.
- **UNC path block** — `run_command` rejects UNC paths (`\\server\share`) to prevent NTLM hash leaks.
- **`javascript:` href block** — Markdown link renderer blocks `javascript:` URLs.
- **Security headers** — Electron HTTP responses now include `X-Frame-Options`, `X-Content-Type-Options`, `Referrer-Policy`, and `COOP`.
- **Coupon generation** uses `crypto.getRandomValues` (removed hardcoded fallback).

---

## [1.0.1] — 2026-06-16

### Added
- **MCP Integration guide** — tabbed setup guide inside Settings → MCP for Claude Desktop, Claude Code, Cursor, Windsurf, and other MCP-compatible clients
- **AI key rename** — pencil icon on each key card lets you rename keys without revoking them
- **Update notification** — a pill in the header alerts you when a new version is available on keepra.tech
- **Version display** — current app version shown in Settings → About

### Changed
- Vault credential form: "Add field" button moved to the modal footer alongside Cancel/Save
- Custom field labels now use the correct text color (white in dark mode)
- "Setup Guide" sidebar label renamed to "MCP Integration"

### Fixed
- Custom vault field label was rendering in gray instead of white
- Unwanted dashed bottom border removed from custom field labels

---

## [1.0.0] — 2026-06-09

### Initial release

- **Links** — bookmark manager with categories, tags, pin, grid/list views
- **Vault** — AES-256-GCM encrypted credential store (passwords, SSH keys, API keys, cards, secure notes)
- **Notes** — Markdown notepad with live preview, tags, auto-save
- **Tasks** — to-do lists with My Day, Important, Planned smart lists, subtasks, due dates, recurrence
- **Contacts** — people directory with unlimited phone/email/link rows per contact
- **Drive** — 50 MB encrypted file storage with IndexedDB + Firestore chunked sync
- **Dashboard** — home overview with stat tiles and widget cards
- **Cross-device sync** — zero-knowledge E2E via a single Secret Key (QR link or manual entry)
- **MCP AI Access** — scoped API keys for Claude Desktop, Claude Code, Cursor, and any MCP client
- **5 themes** — system, light, dark, ocean, forest
- **PWA** — installable on any platform with offline support
- **Electron desktop app** — Windows
- **Android APK** — sideloadable; Play Store submission in progress
- **Public beta** — all features unlocked during beta period (target Q4 2026 for paid plans)
