# Design a Chat App

A chat frontend shows a list of conversations and the messages of the open one, and it sends and receives messages in real time. The design is decided by the connection, by how messages are ordered and deduplicated, and by what the user sees when a message has not reached the server yet.

## Requirements to clarify

| Question | Why it matters |
|---|---|
| One-to-one, groups, or both? How large is a group? | Decides the volume of incoming events |
| Which message types: text, images, files, reactions? | Decides upload and rendering work |
| Are typing indicators, read receipts, and presence required? | Each adds a class of short-lived events |
| Must messages be readable and sendable offline? | Adds local storage and an outbox |
| How much history is loaded? | Decides pagination and virtualization |

Quality targets to state: a sent message appears on the sender's screen within 100 milliseconds, a received message within 1 second, and no message is lost or shown twice.

## Sketch

| Part | Responsibility |
|---|---|
| Conversation list | Shows conversations ordered by last activity, with unread counts |
| Message list | Virtualized, anchored to the bottom, loads older messages upward |
| Composer | Text input, attachments, send action |
| Connection manager | Owns the WebSocket, reconnection, and heartbeats |
| Store | Normalized conversations and messages, plus the outbox |

A message record holds `id`, `clientId`, `conversationId`, `senderId`, `body`, `createdAt`, `sequence`, and `status` (sending, sent, delivered, read, failed).

History loads over HTTP: `GET /conversations/:id/messages?before=<cursor>&limit=30`. Live events arrive over the WebSocket.

## The decisions that matter

### 1. The connection

Both sides send often, so a WebSocket fits. One connection serves all conversations. The manager reconnects with backoff and jitter. After reconnecting, it sends the last sequence number it received per conversation and asks for everything after it. Without that step, messages sent during the gap are missing until a reload.

### 2. Optimistic sends and deduplication

The client creates a `clientId` for each new message, adds the message to the list with status "sending", and sends it. The server echoes the message with the same `clientId` and a permanent `id`. The client replaces the temporary record instead of adding a second one. If the acknowledgment is lost and the client retries, the server recognizes the `clientId` and does not store a duplicate.

### 3. Ordering

Device clocks disagree, so the client cannot order by its own timestamps. The server assigns a sequence number per conversation, and the list sorts by it. Messages still in "sending" state are shown at the end.

### 4. Scroll behavior

| Situation | Behavior |
|---|---|
| User is at the bottom and a message arrives | Scroll to show it |
| User is reading older messages and a message arrives | Do not move; show a "new messages" button |
| Older messages load at the top | Keep the visible message in the same position |

## Optimizations to mention

- Batch typing events: send at most one every 3 seconds, and expire the indicator after 5 seconds.
- Upload images directly to storage, show a local preview at once, and send the message when the upload completes.
- Keep the outbox in IndexedDB so unsent messages survive a reload.
- Render message text as text, never as HTML, and build links safely.

## What the interviewer probes

- What happens with the app open in three tabs? Share one connection, or deduplicate by message identifier in each tab.
- How are unread counts kept correct? The server owns the count, and the client adjusts it optimistically.
- How is a failed message shown? A "not sent" marker with a retry action, kept in the outbox.
- How does a group of 5,000 members change the design? Presence and typing events must be sampled or disabled.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-chat-app) has the complete chat case study, plus an AI chat interface with streamed responses.
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-chat-app) covers the messaging backend.
- Related pages: [real-time updates](../concepts/real-time-updates.md), [offline and storage](../concepts/offline-and-storage.md).
