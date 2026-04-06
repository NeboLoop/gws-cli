---
name: gws-gmail-labels
description: "Gmail: Create, list, update, and delete labels for organizing messages."
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
  - email label
  - create label
  - list labels
  - manage labels
  - rename label
  - delete label
---

# Gmail Label Management

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

Create, list, update, and delete Gmail labels.

## List Labels

```bash
gws gmail users labels list \
  --params '{"userId": "me"}' \
  --fields "labels(id,name,type)"
```

**User labels only (exclude system labels):**
```bash
gws gmail users labels list \
  --params '{"userId": "me"}' \
  --fields "labels(id,name,type)" | jq '[.labels[] | select(.type == "user")]'
```

## Get a Label

```bash
gws gmail users labels get \
  --params '{"userId": "me", "id": "LABEL_ID"}' \
  --fields "id,name,messagesTotal,messagesUnread,threadsTotal,threadsUnread"
```

## Create a Label

```bash
gws gmail users labels create \
  --params '{"userId": "me"}' \
  --json '{
    "name": "Projects/Client-A",
    "labelListVisibility": "labelShow",
    "messageListVisibility": "show"
  }'
```

**Nested labels** use `/` as separator: `"name": "Work/Urgent"`.

### Visibility Options

| Field | Values | Description |
|-------|--------|-------------|
| `labelListVisibility` | `labelShow`, `labelShowIfUnread`, `labelHide` | Show in label list |
| `messageListVisibility` | `show`, `hide` | Show in message list |

## Update a Label

Full replacement of label properties.

```bash
gws gmail users labels update \
  --params '{"userId": "me", "id": "LABEL_ID"}' \
  --json '{
    "name": "Projects/Client-B",
    "labelListVisibility": "labelShow",
    "messageListVisibility": "show"
  }'
```

## Patch a Label

Partial update — only changes specified fields.

```bash
gws gmail users labels patch \
  --params '{"userId": "me", "id": "LABEL_ID"}' \
  --json '{"name": "New Name"}'
```

## Delete a Label

Permanently removes the label and unlinks it from all messages/threads.

```bash
gws gmail users labels delete \
  --params '{"userId": "me", "id": "LABEL_ID"}'
```

## System Label IDs

These are built-in and cannot be created, renamed, or deleted:

| Label ID | Name |
|----------|------|
| `INBOX` | Inbox |
| `SENT` | Sent Mail |
| `DRAFT` | Drafts |
| `SPAM` | Spam |
| `TRASH` | Trash |
| `UNREAD` | Unread |
| `STARRED` | Starred |
| `IMPORTANT` | Important |
| `CATEGORY_PERSONAL` | Primary |
| `CATEGORY_SOCIAL` | Social |
| `CATEGORY_PROMOTIONS` | Promotions |
| `CATEGORY_UPDATES` | Updates |
| `CATEGORY_FORUMS` | Forums |

## Tips

- Always list labels first to get the `id` before applying labels to messages.
- User-created label IDs look like `Label_1`, `Label_5`, etc.
- Use nested naming (`Parent/Child`) for organized label hierarchies.

## See Also

- [gws-gmail-batch](../gws-gmail-batch/SKILL.md) — Bulk apply labels
- [gws-gmail-manage](../gws-gmail-manage/SKILL.md) — Apply labels to single messages
- [recipe-create-gmail-filter](../recipe-create-gmail-filter/SKILL.md) — Auto-label with filters
