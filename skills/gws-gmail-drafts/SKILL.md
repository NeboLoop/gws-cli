---
name: gws-gmail-drafts
description: "Gmail: Create, update, list, send, and delete drafts."
metadata:
  version: 0.22.3
  openclaw:
    category: "productivity"
    requires:
      bins:
        - gws
      skills:
        - gws-gmail
triggers:
  - email draft
  - create draft
  - send draft
  - list drafts
  - update draft
  - delete draft
---

# Gmail Draft Management

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

Create, update, list, send, and delete email drafts.

## List Drafts

```bash
gws gmail users drafts list \
  --params '{"userId": "me", "maxResults": 10}' \
  --fields "drafts(id,message/id,message/snippet)"
```

## Get a Draft

```bash
gws gmail users drafts get \
  --params '{"userId": "me", "id": "DRAFT_ID"}'
```

## Create a Draft

Create a draft without sending it. The message body must be base64url-encoded RFC 2822.

```bash
gws gmail users drafts create \
  --params '{"userId": "me"}' \
  --json '{
    "message": {
      "raw": "BASE64URL_ENCODED_RFC2822"
    }
  }'
```

**Tip:** Use `gws gmail +send --draft` to create a draft interactively instead of constructing raw RFC 2822 manually.

```bash
gws gmail +send --to user@example.com --subject "Review this" --body "Please review the attached." --draft
```

## Update a Draft

Replace the content of an existing draft.

```bash
gws gmail users drafts update \
  --params '{"userId": "me", "id": "DRAFT_ID"}' \
  --json '{
    "message": {
      "raw": "BASE64URL_ENCODED_RFC2822"
    }
  }'
```

## Send a Draft

Send an existing draft to its recipients.

```bash
gws gmail users drafts send \
  --params '{"userId": "me"}' \
  --json '{"id": "DRAFT_ID"}'
```

## Delete a Draft

Permanently deletes the draft. Does NOT go to Trash.

```bash
gws gmail users drafts delete \
  --params '{"userId": "me", "id": "DRAFT_ID"}'
```

## Workflow: Draft, Review, Send

1. Create draft: `gws gmail +send --to boss@company.com --subject "Q4 Report" --body "..." --draft`
2. List drafts: `gws gmail users drafts list --params '{"userId": "me"}' --fields "drafts(id,message/snippet)"`
3. Send when ready: `gws gmail users drafts send --params '{"userId": "me"}' --json '{"id": "DRAFT_ID"}'`

## See Also

- [gws-gmail-send](../gws-gmail-send/SKILL.md) — Send email directly
- [gws-gmail](../gws-gmail/SKILL.md) — All Gmail operations
