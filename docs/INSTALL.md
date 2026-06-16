# Installation Guide

## Web (recommended — no install needed)

1. Open [keepra.tech/app.html](https://keepra.tech/app.html) in any modern browser
2. To install as a PWA (for offline use and a native feel):
   - **Chrome/Edge**: click the install icon in the address bar or go to Menu → Install Keepra
   - **Safari (iOS)**: tap Share → Add to Home Screen
   - **Firefox**: Menu → Install

---

## Windows Desktop App

Requirements: Windows 10 or later

1. Download the `.exe` installer from [keepra.tech/downloads/](https://keepra.tech/downloads/)
2. Run the installer — Windows may show a SmartScreen warning; click "More info → Run anyway" (the app is not code-signed yet)
3. Keepra launches from the Start Menu or desktop shortcut
4. Data is stored locally; sync is optional

---

## Android APK (sideload)

Requirements: Android 8.0+

1. Download `keepra.apk` from [keepra.tech/downloads/](https://keepra.tech/downloads/)
2. On your Android device: Settings → Security → Install unknown apps → allow your browser
3. Tap the downloaded APK to install
4. Open Keepra from the app drawer

> Play Store version is in progress. The sideload APK is the same build.

---

## MCP Integration (AI clients)

The Keepra desktop app includes an MCP server for connecting AI assistants.

See the full setup guide inside the app: **Settings → MCP → MCP Integration**

Quick reference:

### Claude Desktop

Edit `%APPDATA%\Claude\claude_desktop_config.json`:

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

### Claude Code (CLI)

```bash
claude mcp add keepra -e KEEPRA_KEY=YOUR_KEY_HERE -- node C:\Keepra\keepra-mcp.js
```

### Cursor / Windsurf / Other

Use the same JSON config block above in your client's MCP server settings.

**Create your key in Keepra:** Settings → MCP → My Keys → New Key

---

## Troubleshooting

**Sync doesn't work on the file:// URL?**
Use `http://localhost` instead (open via the desktop app or `node dev-server.js` for local dev).

**"Windows protected your PC" on install?**
The app is not yet code-signed. Click "More info → Run anyway" — the binary is safe to run.

**Android APK install blocked?**
Enable "Install unknown apps" for your browser in Android settings.

**MCP server not connecting?**
The Keepra desktop app must be running for the MCP bridge to work.
