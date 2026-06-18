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
        "KEEPRA_KEY": "YOUR_KEY_HERE",
        "KEEPRA_URL": "http://127.0.0.1:47615"
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

Open `~/.claude/settings.json` (create it if it doesn't exist) and add under `"mcpServers"`:

```json
{
  "mcpServers": {
    "keepra": {
      "command": "node",
      "args": ["C:\\Keepra\\keepra-mcp.js"],
      "env": {
        "KEEPRA_KEY": "YOUR_KEY_HERE",
        "KEEPRA_URL": "http://127.0.0.1:47615"
      }
    }
  }
}
```

Save the file — Claude Code picks it up automatically on the next session.

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

| Scope | Tools | Notes |
|-------|-------|-------|
| `tasks:read` | `list_tasks` | — |
| `tasks:write` | `add_task`, `complete_task` | — |
| `notes:read` | `list_notes`, `read_note` | — |
| `notes:write` | `create_note`, `update_note` | — |
| `links:read` | `list_links` | — |
| `links:write` | `add_link` | — |
| `contacts:read` | `list_contacts`, `get_contact` | — |
| `contacts:write` | `add_contact` | — |
| Vault item grant | `list_credentials`, `get_credential` | Per-item, enabled in each item's edit modal |
| Vault item grant + run_command opt-in | `run_command` | Executes shell commands with vault secrets injected as env vars. Requires explicit opt-in when creating the key. Keepra asks for confirmation on first use. |
| Vault item grant | `ftp_list`, `ftp_upload`, `ftp_download`, `ftp_delete`, `ftp_mkdir`, `ftp_rename` | FTP operations using credentials stored in the vault. The AI never sees the password. |

Grant only the scopes your AI needs — scope down to individual vault items for maximum security.

---

## Security model

- Keys are **device-local** and never synced to the cloud
- Each key enforces its scope on every call — the AI cannot access data beyond what the key allows
- The Keepra desktop app must be running; there is no always-on cloud endpoint
- Revoke any key at any time in Settings → MCP → My Keys → Trash icon
