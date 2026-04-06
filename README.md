# Formester MCP Server

Give your AI agent access to form submissions — read responses, query across forms, write insights back, and process file attachments.

**Endpoint:** `https://app.formester.com/mcp/sse`

```
Available tools: read_submission · query_submissions · update_submission · fetch_file
```

---

## Quickstart

**1. Get a token**

In Formester: **API → Create Token**

Select the permissions your agent needs — View Submissions, Update Submissions, View Forms.

→ [Full authentication guide](docs/authentication.md)

**2. Connect your AI client**

### Claude Desktop

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "formester": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://app.formester.com/mcp/sse",
        "--header",
        "Authorization: Bearer YOUR_TOKEN_HERE"
      ]
    }
  }
}
```

Fully quit and restart Claude Desktop.

### VS Code (GitHub Copilot)

Create or edit `.vscode/mcp.json`:

```json
{
  "servers": {
    "formester": {
      "type": "stdio",
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://app.formester.com/mcp/sse",
        "--header",
        "Authorization: Bearer YOUR_TOKEN_HERE"
      ]
    }
  }
}
```

Switch Copilot Chat to **Agent mode** to use the tools.

### Cursor

**Settings → MCP → Add new MCP server:**

```json
{
  "formester": {
    "command": "npx",
    "args": [
      "mcp-remote",
      "https://app.formester.com/mcp/sse",
      "--header",
      "Authorization: Bearer YOUR_TOKEN_HERE"
    ]
  }
}
```

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

- [Authentication](docs/authentication.md) — token types, scopes, creating and revoking tokens
- [Tools](docs/tools.md) — full reference for all 4 tools with parameters and response shapes
- [Troubleshooting](docs/troubleshooting.md) — common errors and fixes

---

## Links

- [Formester](https://formester.com)
- [Documentation](https://docs.formester.com)
- [Support](https://formester.com/contact)
