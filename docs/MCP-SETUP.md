# MCP Integration Guide

Connect any MCP-compatible AI assistant to your Keepra data with scoped, revocable API keys.

> **Prerequisites:** Keepra desktop app installed and running on your PC.

---

## Step 1 — Create an API Key

1. Open Keepra on your PC
2. Go to **Settings → MCP → My Keys**
3. Click **New Key**
4. Name the key (e.g. "Claude Desktop"), select the tools the AI can access, then click Save
5. Click the 👁 eye icon to reveal and copy the key

---

## Claude Desktop

1. Open (or create) `%APPDATA%\Claude\claude_desktop_config.json`
2. Add the Keepra server under `"mcpServers"`:

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

3. Replace `YOUR_KEY_HERE` with the key you copied
4. Fully quit Claude Desktop (system tray → Quit) and relaunch
5. Test: type *"List my Keepra tasks"*

---

## Claude Code (CLI)

```bash
claude mcp add keepra -e KEEPRA_KEY=YOUR_KEY_HERE -- node C:\Keepra\keepra-mcp.js
```

Verify: `claude mcp list` should show `keepra`.

Test: start a session and type *"Show my Keepra notes"*.

---

## Cursor

1. Press `Ctrl+,` → search "MCP" → open MCP Servers panel
2. Click **Add MCP Server**
3. Paste the config JSON (same format as Claude Desktop above)
4. Replace `YOUR_KEY_HERE` with your key
5. `Ctrl+Shift+P → Reload Window`

---

## Windsurf

1. Open (or create) `~/.codeium/windsurf/mcp_config.json`
2. Add the same JSON config block (same format as Claude Desktop)
3. Restart Windsurf

---

## Other MCP clients

Any client that supports the MCP standard accepts the same `{"mcpServers": {"keepra": {...}}}` config.

---

## Available MCP tools

| Tool | Read | Write |
|------|------|-------|
| `tasks:read` / `tasks:write` | ✅ | ✅ |
| `notes:read` / `notes:write` | ✅ | ✅ |
| `links:read` / `links:write` | ✅ | ✅ |
| `contacts:read` / `contacts:write` | ✅ | ✅ |
| Vault items | Per-item grant | — |

Grant only the scopes your AI needs — scope down to individual vault items for maximum security.

---

## Security model

- Keys are **device-local** and never synced to the cloud
- Each key enforces its scope on every call — the AI cannot access data beyond what the key allows
- The Keepra desktop app must be running; there is no always-on cloud endpoint
- Revoke any key at any time in Settings → MCP → My Keys → Trash icon
