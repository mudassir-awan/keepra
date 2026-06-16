# Changelog

All notable changes to Keepra are documented here.

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
