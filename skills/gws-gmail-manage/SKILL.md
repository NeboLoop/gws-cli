---
name: gws-gmail-manage
description: "Gmail: Mark read/unread, star, trash, delete, and modify labels on individual messages."
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
  - mark read
  - mark unread
  - star email
  - trash email
  - delete email
  - archive email
  - modify labels
---

# Gmail Message Management

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

Modify labels, trash, or delete individual messages.

## Modify Labels

Add or remove labels on a single message.

```bash
gws gmail users messages modify \
  --params '{"userId": "me", "id": "MSG_ID"}' \
  --json '{"addLabelIds": ["LABEL_ID"], "removeLabelIds": ["LABEL_ID"]}'
```

### Common Operations

**Mark as read:**
```bash
gws gmail users messages modify \
  --params '{"userId": "me", "id": "MSG_ID"}' \
  --json '{"removeLabelIds": ["UNREAD"]}'
```

**Mark as unread:**
```bash
gws gmail users messages modify \
  --params '{"userId": "me", "id": "MSG_ID"}' \
  --json '{"addLabelIds": ["UNREAD"]}'
```

**Star a message:**
```bash
gws gmail users messages modify \
  --params '{"userId": "me", "id": "MSG_ID"}' \
  --json '{"addLabelIds": ["STARRED"]}'
```

**Archive (remove from inbox):**
```bash
gws gmail users messages modify \
  --params '{"userId": "me", "id": "MSG_ID"}' \
  --json '{"removeLabelIds": ["INBOX"]}'
```

**Mark as important:**
```bash
gws gmail users messages modify \
  --params '{"userId": "me", "id": "MSG_ID"}' \
  --json '{"addLabelIds": ["IMPORTANT"]}'
```

**Apply a custom label:**
```bash
gws gmail users messages modify \
  --params '{"userId": "me", "id": "MSG_ID"}' \
  --json '{"addLabelIds": ["Label_5"]}'
```

## Trash a Message

Moves a message to Trash. Can be recovered within 30 days.

```bash
gws gmail users messages trash \
  --params '{"userId": "me", "id": "MSG_ID"}'
```

## Untrash a Message

Removes a message from Trash.

```bash
gws gmail users messages untrash \
  --params '{"userId": "me", "id": "MSG_ID"}'
```

## Delete a Message

Permanently deletes a message. NOT recoverable.

```bash
gws gmail users messages delete \
  --params '{"userId": "me", "id": "MSG_ID"}'
```

## Tips

- Use `modify` for label changes — it's the only way to mark read/unread.
- `UNREAD` is a label in Gmail. Removing it marks as read; adding it marks as unread.
- For bulk operations on multiple messages, use [gws-gmail-batch](../gws-gmail-batch/SKILL.md).
- Always use `--dry-run` before destructive operations like `delete`.

## See Also

- [gws-gmail-batch](../gws-gmail-batch/SKILL.md) — Bulk modify/delete
- [gws-gmail-threads](../gws-gmail-threads/SKILL.md) — Thread-level operations
- [gws-gmail-labels](../gws-gmail-labels/SKILL.md) — Create and manage labels
