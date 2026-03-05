---
name: casedelta-gmail
description: "Search and read emails from the lawyer's Gmail account. Use when the lawyer asks about emails, messages, correspondence, client communications, or anything related to their inbox."
metadata: { "openclaw": { "emoji": "📧", "requires": { "env": ["GOOGLE_ACCESS_TOKEN"] } } }
---

# Gmail Integration

Connects to Gmail API v1 to search and read the lawyer's emails.

## When to use

- "Do I have any emails from John Smith?" → search Gmail
- "What did opposing counsel say in their last email?" → search Gmail
- "Show me recent emails about the Martinez matter" → search Gmail
- "Any emails about the lease renewal?" → search Gmail
- "What's in my inbox about the Henderson case?" → search Gmail

## Setup

Required environment variables:

- `GOOGLE_ACCESS_TOKEN` — OAuth 2.0 access token from the lawyer's Google account

## Search emails

```bash
node skills/casedelta-gmail/scripts/search.js "lease renewal Henderson"
```

## Get a specific email by ID

```bash
node skills/casedelta-gmail/scripts/search.js --id <message_id>
```

## Output format

Returns a list of messages with:

- `id` — Gmail message ID
- `subject` — Email subject
- `from` — Sender name and address
- `to` — Recipients
- `date` — Date sent
- `snippet` — Short preview of the body
- `body` — Full plain-text body (truncated to 2000 chars)

## Tips for the agent

- Always include sender, date, and subject in your answer so the lawyer can find it
- Use specific legal terms in the query (e.g. "deposition" not just "meeting")
- If no results, try broader terms before asking the lawyer to clarify
- Snippets are short — fetch the full body with --id for prep or synthesis questions
- **Use Gmail as a fallback when calendar or Clio come up empty.** If the lawyer
  asks about a specific meeting that doesn't show on the calendar, search Gmail —
  many meetings are confirmed by email without a formal calendar invite. Pattern:
  search calendar first, nothing found → search Gmail for the contact or topic.
- For pre-call prep, search the contact's name or firm name to surface recent
  correspondence, context about the relationship, and any action items mentioned
