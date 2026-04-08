# Tools

Formester MCP exposes four tools. All tools require a valid token — see [Authentication](authentication.md).

---

## read_submission

Fetches a single submission by UUID, including form field responses and any custom fields previously written by AI agents.

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `submission_id` | string | Yes | Submission UUID |
| `include_files` | boolean | No | Include attachment metadata (id, filename, url). Default: false. Call `fetch_file` to get actual content. |

**Example prompts**
```
Read submission a1b2c3d4 and summarize the support issue reported.
```
```
Read submission a1b2c3d4 with files included, then fetch the resume PDF and extract the candidate's skills and experience.
```

**Required scope:** `submission:read`

---

## query_submissions

Searches submissions across a form with optional filters. Returns paginated results.

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `form_id` | string | Yes | Form UUID |
| `filters` | object | No | Filter conditions (see below) |
| `limit` | integer | No | Max results to return (1–100). Default: 10 |
| `offset` | integer | No | Number of results to skip. Default: 0 |

**Filter options**

| Filter | Type | Description |
|--------|------|-------------|
| `created_after` | string | ISO8601 datetime with timezone, e.g. `2024-01-01T00:00:00Z` |
| `created_before` | string | ISO8601 datetime with timezone |
| `starred` | boolean | Only starred submissions |
| `spam` | boolean | Include spam submissions |
| `custom_field` | string | Custom field name to filter on |
| `custom_field_value` | string | Value to match for the custom field |

**Example prompts**
```
Get the last 30 support tickets from form xyz, classify each as 'billing', 'technical', or 'other', and categorize each by urgency.
```
```
Find all starred submissions from the past week for form xyz and summarize the common themes.
```
```
Get all job applications from form xyz where 'ai_status' is 'shortlisted' and draft interview questions for each.
```

**Required scope:** `submission:read`

---

## update_submission

Saves AI-generated values back to a submission as custom fields. Creates the column automatically if it doesn't exist yet.

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `submission_id` | string | Yes | Submission UUID |
| `fields` | array | Yes | Array of field objects (see below) |

**Field object**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `field_name` | string | Yes | Name for the custom field |
| `field_value` | any | Yes | Value to store |
| `field_type` | string | No | Column type. Default: `shorttext`. Options: `shorttext`, `longtext`, `number`, `date`, `time`, `radio`, `multiple-checkbox`, `checkbox` |
| `field_options` | array | Conditional | Required for `radio` and `multiple-checkbox`. Array of choice strings. |

**Example prompts**
```
Read submission a1b2c3d4 and save a one-sentence summary back as 'ai_summary'.
```
```
Review the contact form submission a1b2c3d4 and tag it as 'sales', 'support', or 'other' using a field called 'ai_category'.
```

**Required scope:** `submission:write`

---

## fetch_file

Downloads a file attachment and returns its content for AI processing. Call `read_submission` with `include_files: true` first to get attachment IDs.

**Parameters**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `attachment_id` | integer | Yes | Attachment ID from `read_submission` response |
| `extract_text` | boolean | No | Extract text from PDFs instead of returning raw base64. Default: true for PDFs. |
| `include_metadata` | boolean | No | Include file dimensions, page count, etc. Default: true. |

**Supported file types**

| Type | How it's returned |
|------|------------------|
| Images (PNG, JPG, WebP, GIF) | Base64 — vision-capable models can analyze directly |
| PDFs | Extracted text (or base64 if `extract_text: false`) |
| Text files (TXT, CSV, JSON, MD) | Plain text |
| Other documents | Base64 |

**File size limit:** 10 MB

**Example prompts**
```
Read submission a1b2c3d4 with files included, then fetch the attached PDF and extract the applicant's name, contact details, and work experience.
```
```
Fetch image attachment 789 from submission a1b2c3d4 and describe what's shown in it.
```

**Required scope:** `submission:read`
