# Formester MCP Server

Give your AI agent access to form submissions — read responses, query across forms, write insights back, and process file attachments.

**Endpoint:** `https://app.formester.com/mcp`

```
Available tools: read_submission · query_submissions · update_submission · fetch_file
```

---

## Quickstart

**Step 1: Choose your auth method**

| Method | Best for |
|--------|---------|
| **OAuth** | Interactive clients (Claude.ai, Cursor, VS Code) — authorize via browser, no token setup |
| **API Token** | Scripts, automation, or clients without OAuth support — create once in Formester → API |

→ [Full authentication guide](docs/authentication.md)

**Step 2: Connect your AI client**

---

### Claude

**Via OAuth (recommended)**

Settings → Connectors → Add custom connector → enter a name and `https://app.formester.com/mcp` as the URL. Claude will prompt you to authorize on first connection.

**Via API Token**

```json
{
  "mcpServers": {
    "formester": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://app.formester.com/mcp",
        "--header",
        "Authorization: Bearer YOUR_TOKEN_HERE"
      ]
    }
  }
}
```

Fully quit and restart Claude Desktop.
---

### VS Code (GitHub Copilot)

**Via OAuth**

Create or edit `.vscode/mcp.json`:

```json
{
  "servers": {
    "formester": {
      "type": "http",
      "url": "https://app.formester.com/mcp"
    }
  }
}
```

VS Code will handle the OAuth flow automatically. Switch Copilot Chat to **Agent mode** to use the tools.

**Via API Token**

```json
{
  "servers": {
    "formester": {
      "type": "http",
      "url": "https://app.formester.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN_HERE"
      }
    }
  }
}
```

---

### Cursor

**Via OAuth**

Settings → MCP → Add new MCP server:

```json
{
  "mcpServers": {
    "formester": {
      "url": "https://app.formester.com/mcp"
    }
  }
}
```

Cursor will prompt you to authorize via browser on first use.

**Via API Token**

```json
{
  "mcpServers": {
    "formester": {
      "url": "https://app.formester.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN_HERE"
      }
    }
  }
}
```

---

### Claude Code

**Via OAuth**

```bash
claude mcp add --transport http formester https://app.formester.com/mcp
```

**Via API Token**

```bash
claude mcp add --transport http formester https://app.formester.com/mcp \
  --header "Authorization: Bearer YOUR_TOKEN_HERE"
```

---

### Windsurf

**Via OAuth**

Edit `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "formester": {
      "type": "streamable-http",
      "url": "https://app.formester.com/mcp"
    }
  }
}
```

**Via API Token**

```json
{
  "mcpServers": {
    "formester": {
      "type": "streamable-http",
      "url": "https://app.formester.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_TOKEN_HERE"
      }
    }
  }
}
```

Restart Windsurf after saving.

---

## Example prompts

**Summarize a support ticket**
```
Read submission a1b2c3d4 and summarize the issue reported.
```

**Triage a contact form**
```
Read the last 20 submissions for form xyz, classify each one as 'sales', 'support', or 'other'
based on the message content, and save the category as a custom field called 'ai_category'.
```

**Review a job application**
```
Read submission a1b2c3d4 with files included. Fetch the resume PDF, extract the candidate's
skills and years of experience, and write a brief evaluation back as 'ai_evaluation'.
```

**Process a batch of applications**
```
Query all submissions for form xyz from the past 7 days. For each one, fetch the attached CV,
extract the applicant's name, role applied for, and top 3 skills, then save them as custom fields.
```

**Flag urgent issues**
```
Get the last 50 submissions for form xyz. Identify any that mention billing problems or account
access issues, mark them as starred, and set a custom field 'ai_priority' to 'urgent'.
```

---

## Docs

- [Authentication](docs/authentication.md) — OAuth, API tokens, scopes
- [Tools](docs/tools.md) — full reference for all 4 tools with parameters and response shapes
- [Troubleshooting](docs/troubleshooting.md) — common errors and fixes

---

## Links

- [Formester](https://formester.com)
- [Documentation](https://docs.formester.com)
- [Support](https://formester.com/contact)
