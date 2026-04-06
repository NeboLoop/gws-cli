---
name: gws-gmail-threads
description: "Gmail: Manage conversations — list, get, modify labels, trash, and delete threads."
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
  - email thread
  - conversation thread
  - thread label
  - archive thread
  - trash thread
---

# Gmail Thread Operations

> **PREREQUISITE:** Read `../gws-shared/SKILL.md` for auth, global flags, and security rules.

Operate on entire conversations (threads) instead of individual messages. Label changes on a thread apply to all messages in that thread.

## List Threads

```bash
gws gmail users threads list \
  --params '{"userId": "me", "q": "is:unread", "maxResults": 20}' \
  --fields "threads(id,snippet,historyId)"
```

**With pagination:**
```bash
gws gmail users threads list \
  --params '{"userId": "me", "q": "label:inbox"}' \
  --page-all --page-limit 5
```

## Get a Thread

Retrieve all messages in a thread.

```bash
gws gmail users threads get \
  --params '{"userId": "me", "id": "THREAD_ID"}' \
  --fields "id,messages(id,snippet,labelIds,payload/headers)"
```

## Modify Thread Labels

Add or remove labels from all messages in a thread.

```bash
gws gmail users threads modify \
  --params '{"userId": "me", "id": "THREAD_ID"}' \
  --json '{"addLabelIds": ["LABEL_ID"], "removeLabelIds": ["LABEL_ID"]}'
```

### Common Operations

**Mark entire conversation as read:**
```bash
gws gmail users threads modify \
  --params '{"userId": "me", "id": "THREAD_ID"}' \
  --json '{"removeLabelIds": ["UNREAD"]}'
```

**Archive a conversation:**
```bash
gws gmail users threads modify \
  --params '{"userId": "me", "id": "THREAD_ID"}' \
  --json '{"removeLabelIds": ["INBOX"]}'
```

**Mark as read and archive:**
```bash
gws gmail users threads modify \
  --params '{"userId": "me", "id": "THREAD_ID"}' \
  --json '{"removeLabelIds": ["UNREAD", "INBOX"]}'
```

**Label a conversation:**
```bash
gws gmail users threads modify \
  --params '{"userId": "me", "id": "THREAD_ID"}' \
  --json '{"addLabelIds": ["Label_5"]}'
```

## Trash a Thread

Moves the entire conversation to Trash. Recoverable within 30 days.

```bash
gws gmail users threads trash \
  --params '{"userId": "me", "id": "THREAD_ID"}'
```

## Untrash a Thread

```bash
gws gmail users threads untrash \
  --params '{"userId": "me", "id": "THREAD_ID"}'
```

## Delete a Thread

Permanently deletes the thread and all its messages. NOT recoverable.

```bash
gws gmail users threads delete \
  --params '{"userId": "me", "id": "THREAD_ID"}'
```

## When to Use Threads vs Messages

- **Threads** — when you want to act on an entire conversation (e.g., archive a thread, mark a conversation as read).
- **Messages** — when you need to act on specific messages within a conversation.
- **Batch** — when you need to act on many messages across different threads.

## See Also

- [gws-gmail-manage](../gws-gmail-manage/SKILL.md) — Single message operations
- [gws-gmail-batch](../gws-gmail-batch/SKILL.md) — Bulk message operations
