# Troubleshooting

---

## Authentication errors

### "Missing authorization token"
The `Authorization` header is not reaching the server.
- Check the format: `Authorization: Bearer YOUR_TOKEN_HERE` — the space after `Bearer` is required
- In `mcp-remote`, the header goes directly in args: `"--header", "Authorization: Bearer YOUR_TOKEN_HERE"`

### "Invalid or expired token"
The token was received but not recognized.
- Verify the token is active — go to **Formester → API** and check it hasn't been revoked
- Make sure you copied the full token with no extra spaces

### "API token has expired"
Your API token has passed its expiry date. Go to **Formester → API** and create a new token.

### "API token signature is invalid" / "API token is malformed"
The token value is corrupted or from a different environment. Make sure you copied it exactly as shown with no truncation.

---

## Authorization errors

### "Insufficient permissions. Required scope: ..."
The token doesn't have the required permission for the tool being called.

| Tool | Required permission |
|------|---------------------|
| `read_submission`, `query_submissions`, `fetch_file` | View Submissions |
| `update_submission` | Update Submissions |
| `query_submissions` (form lookup) | View Forms |

Go to **Formester → API**, revoke the current token and create a new one with the correct permissions.

### "Submission not found or access denied"
The submission UUID doesn't belong to your organization, or the token doesn't have access to it. Verify the UUID is correct.

### "Form not found or access denied"
The form UUID doesn't exist in your organization or the token doesn't have access to it.

### "Token is restricted to a different form"
You're using a form-scoped token but calling a tool with a submission or form outside its scope. [Contact support](https://formester.com/contact) to get a token for the correct form.

---

## Tool-specific errors

### "Invalid argument: Invalid date format for created_after / created_before"
Date filters require ISO8601 with an explicit timezone:
- ✅ `2024-01-01T00:00:00Z`
- ✅ `2024-06-15T17:00:00+05:30`
- ❌ `yesterday`, `5PM`, `2024-01-01` (no timezone)

### "'fields' must be an array" (`update_submission`)
The `fields` parameter must be an array of objects, not a string or a single object.

### "No valid custom fields to update" (`update_submission`)
All fields passed are system fields that cannot be modified via MCP. Only custom fields created by agents can be written.

### "File too large" (`fetch_file`)
The attachment exceeds the 10 MB limit. Use the attachment URL from `read_submission` to process the file externally.

### "File download failed" (`fetch_file`)
The attachment URL expired. Call `read_submission` again to get a fresh URL, then retry `fetch_file`.

### "PDF parsing failed" (`fetch_file`)
The PDF is corrupted or malformed. Retry with `extract_text: false` to get the raw base64 content instead.

---

## Tools not appearing in Claude Desktop
- Fully quit Claude Desktop (not just close the window) and reopen it after editing the config
- Check the config file is valid JSON — a missing comma or bracket silently prevents loading
- Verify connectivity: `curl https://app.formester.com/mcp/sse`

---

## Rate limit errors (429)
- 100 requests per minute per token
- 1000 requests per hour per token

Reduce the frequency of `query_submissions` calls or lower the `limit` parameter.

---

## Still stuck?

[Contact support](https://formester.com/contact) with the error message and the tool you were calling.
