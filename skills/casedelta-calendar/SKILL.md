---
name: casedelta-calendar
description: "Search and read events from the lawyer's Google Calendar. Use when the lawyer asks about their schedule, upcoming events, hearings, depositions, court dates, deadlines, or meetings."
metadata: { "openclaw": { "emoji": "📅", "requires": { "env": ["GOOGLE_ACCESS_TOKEN"] } } }
---

# Google Calendar Integration

Connects to Google Calendar API v3 to search and retrieve the lawyer's calendar events.

## When to use

- "What do I have on my calendar today?" → list calendar events
- "When is the Martinez hearing?" → search calendar
- "Do I have any depositions this week?" → search calendar
- "What's my schedule for Thursday?" → list calendar events
- "When is the Smith case status conference?" → search calendar

## Setup

Required environment variables:

- `GOOGLE_ACCESS_TOKEN` — OAuth 2.0 access token from the lawyer's Google account

## List today's events

```bash
node skills/casedelta-calendar/scripts/search.js today
```

## List events for a date range

```bash
node skills/casedelta-calendar/scripts/search.js "2026-03-05" "2026-03-12"
```

## Search events by keyword

```bash
node skills/casedelta-calendar/scripts/search.js --query "deposition"
```

## Output format

Returns a list of events with:

- `id` — Calendar event ID
- `summary` — Event title
- `start` — Start date/time
- `end` — End date/time
- `location` — Location if set
- `description` — Event description/notes
- `attendees` — List of attendees

## Tips for the agent

- **For "today's calendar", always search the full day (midnight to midnight), not
  from the current time forward.** Pass `today` or an explicit full-date range.
  Never pass only a time-of-day start — that misses events earlier in the day.
- **For general schedule queries, use an empty/no keyword query with the date range.**
  Never filter by keyword unless the user asks for a specific event type. Prior
  session context (Clio results, document searches) must not narrow this search.
- For "this week", use Monday–Sunday of the current week
- Always include start time and date in your answer; format clearly for readability
- If the lawyer mentions a client name or matter, use it as a keyword
- Court hearings, depositions, and deadlines are often in the event title
- **If calendar shows nothing for a specific meeting the lawyer mentions, pivot to
  Gmail** — the meeting may have been confirmed by email without a calendar invite
