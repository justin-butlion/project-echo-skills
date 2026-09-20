# TipTap changelog body format

Changelog bodies are **not Markdown**. Use TipTap JSON: `{ "type": "doc", "content": [ ... ] }`.

Public docs: [Changelog TipTap body format for agents](https://docs.projectecho.io/api-mcp-and-embed/changelog-tiptap-body-for-agents).

## Minimal valid document

```json
{
  "type": "doc",
  "content": [
    {
      "type": "paragraph",
      "content": [{ "type": "text", "text": "Hello from this release." }]
    }
  ]
}
```

Empty body: `{ "type": "doc", "content": [] }`

## Common blocks

**Paragraph**

```json
{
  "type": "paragraph",
  "content": [{ "type": "text", "text": "Plain paragraph text." }]
}
```

**Marks** — on text nodes: `{ "type": "bold" }`, `{ "type": "italic" }`, `{ "type": "underline" }`, or link `{ "type": "link", "attrs": { "href": "https://example.com", "target": "_blank" } }`.

**Heading** — `attrs.level` 2 or 3:

```json
{
  "type": "heading",
  "attrs": { "level": 2 },
  "content": [{ "type": "text", "text": "What is new" }]
}
```

**Bullet list** — `bulletList` → `listItem` → `paragraph` → text.

**Ordered list** — `orderedList` with `attrs.start`, same listItem shape.

**Blockquote / codeBlock / horizontalRule** — standard TipTap node types (`horizontalRule` has no content).

**Image**

```json
{
  "type": "image",
  "attrs": {
    "src": "https://cdn.example.com/changelog/screenshot.png",
    "alt": "New dashboard screenshot"
  }
}
```

Upload: `upload_changelog_image` → PUT bytes to `upload_url` with returned headers → use `public_url` as `src`.

**YouTube** — `attrs.src` required (watch or youtu.be URL):

```json
{
  "type": "youtube",
  "attrs": {
    "src": "https://www.youtube.com/watch?v=VIDEO_ID",
    "width": 640,
    "height": 360
  }
}
```

## Full example

```json
{
  "type": "doc",
  "content": [
    {
      "type": "heading",
      "attrs": { "level": 2 },
      "content": [{ "type": "text", "text": "Improvements" }]
    },
    {
      "type": "paragraph",
      "content": [
        {
          "type": "text",
          "text": "This week we shipped faster triage and clearer changelog linking."
        }
      ]
    },
    {
      "type": "bulletList",
      "content": [
        {
          "type": "listItem",
          "content": [
            {
              "type": "paragraph",
              "content": [{ "type": "text", "text": "Bulk status updates" }]
            }
          ]
        }
      ]
    }
  ]
}
```

## Common mistakes

| Mistake | Result |
| --- | --- |
| Markdown string as body | `invalid_content_json` |
| Missing `"type": "doc"` | `invalid_content_json` |
| YouTube without `attrs.src` | `youtube_missing_src` |
| Huge `content_json` | `content_too_large` (500 KB) |
