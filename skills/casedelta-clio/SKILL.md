---
name: casedelta-clio
description: "Search and retrieve data from the lawyer's Clio practice management system. Use when the lawyer asks about matters, clients/contacts, billing entries, tasks, notes, or activities tracked in Clio."
metadata: { "openclaw": { "emoji": "⚖️", "requires": { "env": ["CLIO_ACCESS_TOKEN"] } } }
---

# Clio Integration

Connects to the Clio Manage API v4 to retrieve matters, contacts, billing activities, tasks, and notes. Clio is where the law firm tracks their active cases and client relationships.

## When to use

- "What are my open matters?" → search Clio matters
- "Find the Smith matter in Clio" → search Clio matters by name
- "Get contact info for John Smith" → search Clio contacts
- "What time entries exist for the Johnson matter?" → get Clio activities
- "Show me tasks due this week" → list Clio tasks
- "What notes are on the Abbott matter?" → get matter notes

## Setup

Required environment variables:

- `CLIO_ACCESS_TOKEN` — OAuth 2.0 Bearer token from the lawyer's Clio account

## Search matters

```bash
node skills/casedelta-clio/scripts/search.js matters "smith estate"
```

## Search contacts

```bash
node skills/casedelta-clio/scripts/search.js contacts "john smith"
```

## Get activities (time entries) for a matter

```bash
node skills/casedelta-clio/scripts/search.js activities "smith"
```

## Get notes for a matter (by matter ID)

```bash
node skills/casedelta-clio/scripts/search.js notes "<matter_id>"
```

## List tasks

```bash
node skills/casedelta-clio/scripts/search.js tasks "urgent"
```

## Output format

Returns JSON from the Clio API. Key fields per type:

**Matters:** `id`, `display_number`, `description`, `status` (open/closed/pending), `client.name`, `practice_area.name`

**Contacts:** `id`, `name`, `type` (Person/Company), `email_addresses`, `phone_numbers`

**Activities (time entries):** `id`, `note`, `date`, `total` (hours), `matter.display_number`

**Notes:** `id`, `subject`, `detail`, `created_at`, `matter.display_number`

**Tasks:** `id`, `name`, `due_at`, `status`, `matter.display_number`

## Tips for the agent

- Clio matter `display_number` is the human-readable ID (e.g., `23-0145`); use it when reporting back to the lawyer
- Always include matter number and client name in your answer; never expose raw UUIDs
- Clio contacts include both people and companies — specify type when relevant
- If the lawyer mentions a matter number, search matters with that number as the query
- Status: `open` = active matter, `closed` = completed, `pending` = intake
- When listing open matters, proactively filter out closed ones and flag how many were excluded
- Flag matters that have been open a long time with no recent activity — those often need attention
- For morning briefings, sort by age (oldest first) and surface any that look stale
