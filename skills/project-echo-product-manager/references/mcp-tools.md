# MCP tools reference

All tools call the Project Echo REST API with the configured API key. Missing permissions return HTTP 403.

## Discovery

| Tool | Purpose | Permission |
| --- | --- | --- |
| `list_boards` | Boards for filing / filtering requests | `boards:read` |
| `list_statuses` | Status ids (e.g. Completed for changelog eligibility) | `statuses:read` |
| `list_users` / `get_user` | Teammates (authors, owners) | `users:read` |
| `list_members` | Portal members | `members:read` |
| `upsert_member` | Create member (`members:create`) or update existing (`members:update`) by `external_user_id` | create or update |

## Requests

| Tool | Purpose | Permission |
| --- | --- | --- |
| `list_requests` | Filter with `board_id`, `status_id`, `changelog_entry_id`, `unlinked=true`, `limit`, `offset` | `requests:read` |
| `get_request` | One request by id | `requests:read` |
| `create_request` | New feedback request | `requests:create` |

Use `unlinked=true` with a Completed (or changelog-eligible) `status_id` to find shipped work not yet on a changelog.

## Collaboration

| Tool | Purpose | Permission |
| --- | --- | --- |
| `create_comment` | Public comment (needs member identity) | `comments:create` |
| `create_vote` | Upvote (needs member identity) | `votes:create` |
| `list_internal_notes` / `list_request_internal_notes` | Staff-only notes | `internal_notes:read` |
| `create_internal_note` | Add staff note (`author_user_id` required) | `internal_notes:create` |
| `update_internal_note` / `delete_internal_note` | Edit or remove | update / delete |

## Changelogs (drafts only)

| Tool | Purpose | Permission |
| --- | --- | --- |
| `list_changelogs` | Optional `status=draft\|published` | `changelogs:read` |
| `get_changelog` | Entry + linked requests + `content_json` | `changelogs:read` |
| `create_changelog_draft` | New draft; TipTap `content_json` required | `changelogs:create` (+ `changelogs:link_requests` if linking) |
| `update_changelog_draft` | Edit draft fields / body / links | `changelogs:update` (+ link permission if linking) |
| `set_changelog_requests` | Replace linked request ids (all-or-nothing) | `changelogs:link_requests` |
| `upload_changelog_image` | Presigned upload; then PUT file; use `public_url` in TipTap image `src` | `changelogs:create` or `changelogs:update` |

**Cannot publish via API/MCP.** Humans publish in the staff app.

Omitting `author_user_id` defaults to the billing admin with the earliest workspace join time.

## Recommended presets

- **Contributor** — create requests/comments/votes, read + create internal notes, create changelog drafts and link requests
- **Full agent** — broader update/delete except deleting requests, comments, and internal notes
- **Read-only** — inspect only

## Error codes to surface

- `invalid_content_json`, `youtube_missing_src`, `content_too_large`
- `link_validation_failed` (see `details.failed[]`)
- `not_draft`
- `invalid_author`, `no_default_author`
