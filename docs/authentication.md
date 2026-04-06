# Authentication

Formester MCP uses Bearer token authentication. Pass your token in the `Authorization` header with every request.

---

## Creating a token

1. Log in to [Formester](https://app.formester.com)
2. Click **API** in the left sidebar
3. Click **Create Token**
4. Enter a name (e.g. "Claude Desktop")
5. **Forms Access** — leave empty to access all forms in your organization, or select specific forms to restrict access to those only
6. **Permissions** — select what the agent is allowed to do:
   - **View Submissions** — read submission data and attachment metadata
   - **Update Submissions** — write custom fields back to submissions
   - **View Forms** — read form metadata
7. Click **Create** and copy the token — it won't be shown again

To revoke access, click **Revoke** next to any token from the same page.

---

## Choosing permissions

Select only what your agent needs:

| If your agent... | Select |
|-----------------|--------|
| Only reads submissions | View Submissions |
| Reads and writes insights back | View Submissions + Update Submissions |
| Also needs form details | Add View Forms |
