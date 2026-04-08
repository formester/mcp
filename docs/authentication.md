# Authentication

The Formester MCP server supports two authentication methods: **OAuth** (recommended for interactive clients) and **API tokens** (for scripts and automation).

---

## OAuth (Recommended)

Most modern MCP clients (Claude.ai, Cursor, VS Code) handle OAuth automatically — just add the server URL and your browser will open a Formester authorization page.

**How it works:**

1. Add `https://app.formester.com/mcp` to your MCP client
2. Your browser opens the Formester authorization page
3. You approve the requested permissions
4. The client connects — no token needed

---

## API Tokens

For scripts, automation, or clients that don't support OAuth.

### Creating a token

1. Log in to [Formester](https://app.formester.com)
2. Click **API** in the left sidebar
3. Click **Create Token**
4. Enter a name (e.g. "My Script")
5. **Forms Access** — leave empty to access all forms in your organization, or select specific forms to restrict access
6. **Permissions** — select what the token is allowed to do:
   - **View Submissions** — read submission data and attachment metadata
   - **Update Submissions** — write custom fields back to submissions
   - **View Forms** — read form metadata
7. Click **Create** and copy the token — it won't be shown again

To revoke a token, click **Revoke** next to it on the same page.

### Choosing permissions

Select only what your agent needs:

| If your agent... | Select |
|-----------------|--------|
| Only reads submissions | View Submissions |
| Reads and writes insights back | View Submissions + Update Submissions |
| Also needs form structure | Add View Forms |

### Using the token

Pass the token in the `Authorization` header:

```
Authorization: Bearer YOUR_TOKEN_HERE
```

See the [README](../README.md) for per-client configuration examples.
