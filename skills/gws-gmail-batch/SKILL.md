---
name: gws-gmail-batch
description: "Gmail: Bulk modify or delete messages (mark read, label, archive, delete in batch)."
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
  - batch email
  - bulk email
  - batch modify
  - batch delete
  - bulk mark read
  - bulk archive
---

# Gmail Batch Operations

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

Bulk modify labels or delete up to 1000 messages in a single API call.

## Batch Modify

Adds or removes labels from multiple messages at once.

```bash
gws gmail users messages batchModify \
  --params '{"userId": "me"}' \
  --json '{
    "ids": ["MSG_ID_1", "MSG_ID_2", "MSG_ID_3"],
    "addLabelIds": ["LABEL_ID"],
    "removeLabelIds": ["INBOX"]
  }'
```

### Common Patterns

**Mark multiple messages as read:**
```bash
gws gmail users messages batchModify \
  --params '{"userId": "me"}' \
  --json '{
    "ids": ["MSG_ID_1", "MSG_ID_2"],
    "removeLabelIds": ["UNREAD"]
  }'
```

**Bulk archive (remove from inbox):**
```bash
gws gmail users messages batchModify \
  --params '{"userId": "me"}' \
  --json '{
    "ids": ["MSG_ID_1", "MSG_ID_2"],
    "removeLabelIds": ["INBOX"]
  }'
```

**Bulk mark as read AND archive:**
```bash
gws gmail users messages batchModify \
  --params '{"userId": "me"}' \
  --json '{
    "ids": ["MSG_ID_1", "MSG_ID_2"],
    "addLabelIds": [],
    "removeLabelIds": ["UNREAD", "INBOX"]
  }'
```

**Bulk label and archive:**
```bash
gws gmail users messages batchModify \
  --params '{"userId": "me"}' \
  --json '{
    "ids": ["MSG_ID_1", "MSG_ID_2"],
    "addLabelIds": ["Label_5"],
    "removeLabelIds": ["INBOX"]
  }'
```

**Bulk star messages:**
```bash
gws gmail users messages batchModify \
  --params '{"userId": "me"}' \
  --json '{
    "ids": ["MSG_ID_1", "MSG_ID_2"],
    "addLabelIds": ["STARRED"]
  }'
```

## Batch Delete

Permanently deletes messages. This is NOT recoverable — messages skip Trash.

```bash
gws gmail users messages batchDelete \
  --params '{"userId": "me"}' \
  --json '{
    "ids": ["MSG_ID_1", "MSG_ID_2"]
  }'
```

## Workflow: Search then Batch Modify

1. Search for messages to act on:
```bash
gws gmail users messages list \
  --params '{"userId": "me", "q": "from:notifications@github.com is:unread", "maxResults": 50}' \
  --fields "messages(id)"
```

2. Extract the IDs and batch modify:
```bash
gws gmail users messages batchModify \
  --params '{"userId": "me"}' \
  --json '{
    "ids": ["ID1", "ID2", "ID3"],
    "removeLabelIds": ["UNREAD"]
  }'
```

## System Label IDs

| Label ID | Meaning |
|----------|---------|
| `INBOX` | In the inbox |
| `UNREAD` | Unread |
| `STARRED` | Starred |
| `IMPORTANT` | Marked important |
| `SPAM` | Spam |
| `TRASH` | Trash |
| `SENT` | Sent mail |
| `DRAFT` | Drafts |
| `CATEGORY_PERSONAL` | Primary tab |
| `CATEGORY_SOCIAL` | Social tab |
| `CATEGORY_PROMOTIONS` | Promotions tab |
| `CATEGORY_UPDATES` | Updates tab |
| `CATEGORY_FORUMS` | Forums tab |

## Limits

- Maximum 1000 message IDs per batchModify/batchDelete call.
- For more than 1000, split into multiple calls.

## See Also

- [gws-gmail-manage](../gws-gmail-manage/SKILL.md) — Single message modify
- [gws-gmail-labels](../gws-gmail-labels/SKILL.md) — Create and manage labels
- [gws-gmail-triage](../gws-gmail-triage/SKILL.md) — Inbox summary
