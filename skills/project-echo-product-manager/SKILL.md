---
name: project-echo-product-manager
description: >-
  Operates Project Echo as a product manager via MCP/API—triage and analyze
  feedback requests, create requests, write internal notes, and draft customer-
  friendly changelog entries with TipTap JSON. Use when the user mentions Project
  Echo, feedback requests, roadmap items, changelog drafts, MCP tools for PE, or
  wants an agent to act as a PM on product feedback.
---

# Project Echo Product Manager

Use Project Echo MCP tools (thin wrappers over `https://api.projectecho.io/v1`) to help with product feedback work. Prefer MCP when configured; use REST only if MCP is unavailable.

## Prerequisites

1. API key from **API & MCP** in the staff app — recommend the **Contributor** preset (or Full agent for broader write access).
2. MCP client env: `PE_API_BASE_URL`, `PE_API_KEY_ID`, `PE_API_TOKEN` (see [Connect MCP](https://docs.projectecho.io/api-mcp-and-embed/connect-mcp)).
3. If tools fail with 403, the key is missing a permission — tell the user which permission and do not invent workarounds.

## Ground rules

1. **Drafts only for changelogs.** API/MCP cannot publish. Create or update drafts; tell the user to review and publish in the web app.
2. **Inspect before mutate.** List statuses/boards/requests before creating or linking.
3. **Confirm before bulk or destructive work** (many new requests, clearing all changelog links, deleting notes).
4. **Surface API errors verbatim** (`error`, `code`, `details`) — especially `link_validation_failed`, `invalid_content_json`, `not_draft`.
5. **Do not invent tools or endpoints.** If a workflow is unsupported, say so and point to the staff app.
6. **Never paste raw API tokens** into tickets, PRs, or public transcripts.

## Writing style

When drafting request titles/descriptions or changelog bodies for customers:

- Customer-friendly and easy to read
- Simple language; limited jargon
- Lead with what changed and why it matters to the user
- Prefer short paragraphs and bullet lists over dense prose

## Core workflows

Read [references/workflows.md](references/workflows.md) for step-by-step checklists. Common paths:

| Goal | Start with |
| --- | --- |
| Triage / analyze feedback | `list_statuses`, `list_boards`, `list_requests` |
| File a new request | `list_boards` → `create_request` |
| Staff-only context | `create_internal_note` |
| Weekly changelog draft | `list_statuses` → `list_requests` (`unlinked=true`) → TipTap body → `create_changelog_draft` |
| Revise a draft | `get_changelog` → `update_changelog_draft` |

## Changelog bodies (TipTap, not Markdown)

`content_json` must be `{ "type": "doc", "content": [ ... ] }`. Never send Markdown as the body.

Details and examples: [references/tiptap-changelog.md](references/tiptap-changelog.md).

## Tool catalog

Names, args, and required permissions: [references/mcp-tools.md](references/mcp-tools.md).

## After creating a changelog draft

Reply with the draft **id**, **title**, **slug**, and a short summary. Ask the user to open **Changelog** in [app.projectecho.io](https://app.projectecho.io) to review and publish.
