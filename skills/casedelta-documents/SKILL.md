---
name: casedelta-documents
description: "Search uploaded legal case documents using semantic (AI) search. Use when the lawyer asks about document content: lease terms, contract clauses, income figures, dates, parties, addresses, deadlines, or any factual question answerable from uploaded files."
metadata:
  {
    "openclaw":
      { "emoji": "📄", "requires": { "env": ["CASEDELTA_API_KEY", "CASEDELTA_FIRM_ID"] } },
  }
---

# CaseDelta Document Search

Semantic search over the lawyer's uploaded case documents. Powered by vector embeddings — finds relevant text even when the exact words differ from the query.

## When to use

- "What is the monthly rent?" → search documents
- "Who are the parties to the lease?" → search documents
- "What does the contract say about termination?" → search documents
- "What income figures appear in the tax documents?" → search documents
- Any factual question about file contents

## Setup

Required environment variables (set before starting OpenClaw):

- `CASEDELTA_API_KEY` — Platform API key (`dak_...`)
- `CASEDELTA_FIRM_ID` — The law firm's UUID
- `CASEDELTA_CASE_ID` — The active case UUID (optional but strongly recommended to scope results)
- `CASEDELTA_API_URL` — API base URL (default: `https://qa-api.casedelta.com`)

## Search a case's documents

```bash
node skills/casedelta-documents/scripts/search.js "monthly rent amount"
```

With a specific case scoped (always preferred to avoid cross-case results):

```bash
CASEDELTA_CASE_ID=<uuid> node skills/casedelta-documents/scripts/search.js "termination clause"
```

Scoped to one specific file:

```bash
node skills/casedelta-documents/scripts/search.js "base rent" "lease-2024.pdf"
```

## Output format

Returns JSON with a `chunks` array. Each chunk contains:

- `text` — The relevant passage (~1,200 characters)
- `source` — Filename (e.g., `lease-agreement-2024.pdf`)
- `page_number` — Page where it appears
- `score` — Relevance score (0.0–1.0; anything above 0.20 is reliable signal)

## Tips for the agent

- Always cite `source` and `page_number` in your answer so the lawyer can verify
- If results span multiple documents, flag it — the lawyer may want to scope to one file
- If no results, rephrase: "monthly rent" → try "base rent", "rental payment", "lease payment"
- Score below 0.15 means no relevant content; tell the lawyer honestly
- For multi-page documents, search first, then get surrounding pages if a chunk is cut off
- **When answering rent or financial questions, look for asterisks and footnotes** —
  leases often have rent concessions (reduced rate for first N months) that modify
  the headline number. Report the full structure, not just the standard rate.
- **Cite the section number when available** (e.g., "§2.1 Base Rent") — lawyers
  verify by section, not just page number
- If you find conflicting values across pages or documents, report all versions with
  their source and page — never silently pick one
