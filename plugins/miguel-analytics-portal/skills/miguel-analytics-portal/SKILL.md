---
name: miguel-analytics-portal
description: >
  Use when working with a Miguel Analytics client portal or audit: findings,
  tracking plan, measurement implementation, GA4, Meta, TikTok, server-side
  tagging, or when the Miguel Analytics MCP is connected. Helps the client's
  dev team implement or review tracking work from the private portal.
metadata:
  author: Miguel Analytics
  version: "1.0.0"
---

# Miguel Analytics portal

You are helping a client's development team work from their **private** Miguel Analytics audit portal. The portal is only available after the user authenticates with Google through the MCP. The client is selected at login by the allowlist — never hard-code a client, and never invent pages, events, or findings.

## Privacy

- Client documentation, findings, and measurement plans are confidential.
- Do not publish, commit, or paste `llms.txt` / `llms-full.txt` or portal markdown into public issues, PRs, or chats.
- Do not attempt to open `/llms.txt` or `/llms-full.txt` as public URLs. Those HTTP routes are for logged-in browsers only; agents must use the authenticated MCP.
- Stay inside the authenticated client's scope. If a tool returns empty findings or "not found", do not guess from another client.

## Recommended order

1. **Authenticate** if the MCP prompts for Google sign-in. Only allowlisted accounts succeed.
2. **Orient.** Read the resource `portal://llms.txt` or call `get_index`. This is the catalog of every page (title, section, description, `.md` URL). Prefer this over guessing slugs.
3. **Findings first** when the question is about issues, gaps, or what to fix:
   - `list_findings` with optional `platform`, `severity`, or `status`.
   - `get_finding` with the ID (for example `AUD-01` or `TP-09` — use only IDs that actually appear).
4. **Tracking plan and platform pages** when the question is about implementation:
   - Use the index to locate the tracking plan, dictionary, and per-platform pages (web, GA4, Meta, TikTok, server-side, or whatever the catalog lists).
   - `get_page` with `section` + `slug`, or `url`.
5. **`search`** when you need a keyword and do not know the page.
6. **`get_pdf_link`** only when the team asks for a PDF. The URL still requires the same authentication; it is not a public file.
7. Use `portal://llms-full.txt` or `get_index` with `full: true` only when you truly need the entire corpus. It is large. Prefer targeted `get_page` calls.

## How to answer the client's dev team

- Cite finding IDs and the portal page title. Quote the relevant requirement, event, or parameter instead of paraphrasing loosely.
- Be implementation-oriented: what to change, in which platform or property, and how to verify (event name, trigger, parameters, destination).
- If the portal marks an item as pending, blocked, or out of scope, say so. Do not silently upgrade the status.
- If two pages disagree, point at both and ask; do not pick a side without evidence.
- Never invent events, parameters, or pixels that are not in the portal.
- Keep answers in the language of the request (Spanish or English). The portal content is often in Spanish.

## Tools and resources

| Name | When to use it |
| --- | --- |
| `portal://llms.txt` / `get_index` | First look: catalog of pages |
| `portal://llms-full.txt` / `get_index` `{ full: true }` | Entire corpus (sparingly) |
| `list_pages` | Machine-readable page list |
| `get_page` | Full markdown of one page |
| `search` | Keyword lookup |
| `list_findings` / `get_finding` | Audit findings |
| `get_pdf_link` | Authenticated PDF URL (`page`, `section`, or `full`) |

If a tool errors because you are not signed in, ask the user to complete the MCP Google login and retry.
