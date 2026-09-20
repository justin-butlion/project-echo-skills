# Product manager workflows

Copy the checklist and track progress. Writing for customers: simple language, limited jargon, lead with user value.

## Analyze / triage feedback

```
- [ ] list_boards
- [ ] list_statuses
- [ ] list_requests (filter board/status as needed)
- [ ] Summarize themes, volume, and suggested next steps for the user
- [ ] Only create/update when asked
```

## Create a request

```
- [ ] list_boards → pick board_id (ask if unclear)
- [ ] Draft a clear customer-facing title and description
- [ ] create_request
- [ ] Reply with request id and title
```

## Add an internal note

```
- [ ] Confirm request_id and author_user_id (teammate UUID from list_users)
- [ ] create_internal_note
- [ ] Keep notes staff-only; do not put secrets in public comments
```

## Weekly changelog draft from completed work

```
- [ ] list_statuses → find Completed (or changelog-eligible) status id
- [ ] list_requests with status_id + unlinked=true (limit ~50)
- [ ] Summarize shipped work in customer-friendly language
- [ ] Build TipTap content_json (see tiptap-changelog.md)
- [ ] create_changelog_draft (title like "Week of <date>"; optional linked_request_ids, published_at intent)
- [ ] If linking fails, report details.failed and continue without those ids
- [ ] Do NOT publish — return draft id, slug, title, short summary for web-app review
```

## Revise an existing draft

```
- [ ] get_changelog
- [ ] Confirm status is draft (API rejects published edits)
- [ ] update_changelog_draft and/or set_changelog_requests
- [ ] Report changes and any validation errors
```

## Image in a changelog

```
- [ ] upload_changelog_image with content_type
- [ ] PUT file to upload_url with returned headers
- [ ] Put public_url in image node attrs.src inside content_json
```
