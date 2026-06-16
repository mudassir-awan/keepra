<p align="center">
  <img src="assets/logo.png" alt="Keepra" height="80" />
</p>

<p align="center">
  <a href="https://keepra.tech/app.html"><strong>Open Web App</strong></a> &nbsp;&bull;&nbsp;
  <a href="https://keepra.tech/downloads/">Download for Windows or Android</a> &nbsp;&bull;&nbsp;
  <a href="https://github.com/mudassir-awan/keepra/issues">Report a Bug</a>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.1-blue?style=flat-square" alt="Version" />
  <img src="https://img.shields.io/badge/platform-Web%20%7C%20Windows%20%7C%20Android-brightgreen?style=flat-square" alt="Platform" />
  <img src="https://img.shields.io/badge/status-public%20beta-orange?style=flat-square" alt="Beta" />
  <img src="https://img.shields.io/badge/MCP-Claude%20%7C%20Cursor%20%7C%20Windsurf-purple?style=flat-square" alt="MCP" />
  <img src="https://img.shields.io/badge/license-proprietary-lightgrey?style=flat-square" alt="License" />
</p>

---

Keepra is a private, offline-first productivity app that puts six tools in one window: a link manager, an encrypted vault, a Markdown notepad, a task manager, a contacts directory, and a file drive. Everything is encrypted locally and works without internet. Cloud sync is optional and zero-knowledge.

## Screenshots

<p align="center">
  <img src="assets/screenshot-desktop.png" alt="Keepra on Windows" width="700" />
  <br />
  <em>Keepra on Windows</em>
</p>

<p align="center">
  <img src="assets/screenshot-android.png" alt="Keepra on Android" width="300" />
  <br />
  <em>Keepra on Android</em>
</p>

See the **[full visual guide with screenshots of every section](docs/GUIDE.md)** including the MCP Connector setup.

## What is inside

| Tool | What it does |
|------|-------------|
| Links | Save Zoom, Meet, Teams, and any other URL. Click once to open. Organize with categories and tags. |
| Vault | Store passwords, API keys, SSH keys, cards, and secure notes. AES-256-GCM encrypted. Zero-knowledge. |
| Notes | Markdown notepad with live preview, auto-save, and tags. Works offline. |
| Tasks | To-do lists with My Day, priorities, due dates, subtasks, and recurring tasks. |
| Contacts | A personal directory. Each contact can have unlimited phones, emails, and links. |
| Drive | 50 MB encrypted file storage. Supports images, PDFs, and ZIPs. Synced as ciphertext. |

## Download

| Platform | Where to get it | Notes |
|----------|----------------|-------|
| Web (PWA) | [keepra.tech/app.html](https://keepra.tech/app.html) | Works in any browser. Install as a home screen app for offline use. |
| Windows | [keepra.tech/downloads/](https://keepra.tech/downloads/) | Electron desktop app. Windows 10 and above. |
| Android | [keepra.tech/downloads/](https://keepra.tech/downloads/) | APK download. Play Store listing coming soon. |
| iOS / macOS | Coming soon | - |

All features are free during the public beta. No account required to use the app locally.

## How the encryption works

Every item saved in the Vault uses **AES-256-GCM** encryption. The key is derived from your master password using **PBKDF2 with 600,000 iterations and SHA-256**. Your password is never stored anywhere, not even on your own device. It only exists in memory while the app is open.

When you enable cloud sync, Keepra derives a Firebase account and encryption key from a single **Secret Key** that you control. The server only ever receives base64-encoded ciphertext. There is no way for Keepra, Firebase, or anyone else to read your data without your Secret Key.

## Sync across devices

1. Open Settings and go to "Sync and Devices"
2. Create a vault to get your Secret Key
3. On another device, enter the same Secret Key (or scan the QR code shown on the first device)

That is it. No email address, no separate password, no account registration.

## Connect an AI assistant (MCP)

Keepra ships an MCP server that lets Claude, Cursor, Windsurf, and other AI tools read and write your tasks, notes, links, and contacts. Each connection uses a scoped API key that you create inside the app. You choose exactly which tools the AI can access.

**Step-by-step setup: open Keepra, go to Settings, then MCP, then MCP Integration.**

Quick config for Claude Desktop (replace `YOUR_KEY_HERE` with the key you create in the app):

```json
{
  "mcpServers": {
    "keepra": {
      "command": "node",
      "args": ["C:\\Keepra\\keepra-mcp.js"],
      "env": {
        "KEEPRA_KEY": "YOUR_KEY_HERE"
      }
    }
  }
}
```

Claude Code CLI:

```bash
claude mcp add keepra -e KEEPRA_KEY=YOUR_KEY_HERE -- node C:\Keepra\keepra-mcp.js
```

Full guide: [docs/MCP-SETUP.md](docs/MCP-SETUP.md)

## Frequently asked questions

**Is Keepra free?**
Yes, during the public beta (2026) every feature is free. Paid plans are planned for around Q4 2026. Beta users receive a promo code for early-adopter pricing when that happens.

**Does Keepra send my data to a server?**
No. Data lives on your device. If you enable sync, only encrypted ciphertext is stored on Firestore. The server never sees plaintext.

**Can I use Keepra without creating an account?**
Yes. Local use needs no account. Sync needs only a Secret Key, not an email or password.

**What platforms are supported?**
Web (any browser, PWA-installable), Windows 10 and above, and Android. iOS and macOS are planned.

**Is the source code open?**
No, the app is proprietary. This GitHub repository is the public community hub for bug reports, feature requests, and documentation.

**What encryption does Keepra use?**
AES-256-GCM for data at rest. Keys derived with PBKDF2 (SHA-256, 600,000 iterations). The master key is never stored.

## Roadmap

| Feature | Status |
|---------|--------|
| Web PWA | Done |
| Windows desktop (Electron) | Done |
| Android APK | Done |
| MCP integration | Done |
| Zero-knowledge cross-device sync | Done |
| QR code device linking | Done |
| Play Store listing | In progress |
| macOS and iOS | Planned |
| Biometric unlock | Planned |
| Paid plans | Q4 2026 |

Full roadmap: [docs/ROADMAP.md](docs/ROADMAP.md)

## For developers

Keepra is a single-page application with no build step. The stack is plain HTML, CSS, and vanilla JavaScript with jQuery.

- `app.html` - unified shell with all six tools
- `app-logic.js` - all application logic (vanilla JS + jQuery)
- `app.css` - design system with CSS variables, dark and light themes
- `sync.js` - zero-knowledge cloud sync (Firebase)
- `keepra-mcp.js` - MCP stdio server for AI client integration
- `electron.js` - Electron wrapper with embedded HTTP server

The same codebase ships to three targets. Web: serve `app.html` over HTTP. Desktop: Electron copies files to `C:\Keepra\` and serves over `http://localhost`. Android: Capacitor wraps the HTML and JS into a native APK.

Architecture detail: [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md)

## Issues and feedback

This repository is the public community hub. The app source is proprietary.

- [Report a bug](https://github.com/mudassir-awan/keepra/issues/new?template=bug_report.md)
- [Request a feature](https://github.com/mudassir-awan/keepra/issues/new?template=feature_request.md)
- [Ask a question](https://github.com/mudassir-awan/keepra/issues/new?template=question.md)

## Security

For security vulnerabilities, email **support@keepra.tech** instead of opening a public issue.

See [SECURITY.md](SECURITY.md) for the full policy and encryption model.

## License

Keepra is proprietary software. Free for personal use. See [LICENSE](LICENSE).

(c) 2026 Keepra / IBRANICS - [keepra.tech](https://keepra.tech)
