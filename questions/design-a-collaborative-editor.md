# Design a Collaborative Editor

A collaborative editor lets several people edit one document at the same time and see each other's changes within about a second. The central problem is merging edits that were made concurrently, so that every participant ends with the same text. The rest of the design supports that guarantee.

## Requirements to clarify

| Question | Why it matters |
|---|---|
| Plain text or rich text with formatting, tables, and images? | Decides the document model |
| How many simultaneous editors per document: 5 or 500? | Decides presence and update volume |
| Is offline editing required? | Favors one merge approach over the other |
| Are comments, suggestions, and version history in scope? | Each is a large feature; confirm before designing |
| How large can a document grow? | Decides loading and rendering strategy |

Quality targets to state: a local keystroke appears within one frame (16 milliseconds), a remote edit within 1 second, and all clients converge to identical content.

## Sketch

| Part | Responsibility |
|---|---|
| Editor view | Renders the document and captures input |
| Document model | The structured content held in memory |
| Sync engine | Sends local operations, applies remote ones, resolves conflicts |
| Presence layer | Shows other users' cursors and selections |
| Connection | A WebSocket to the collaboration server |

An operation is a small description of one change, such as "insert 'a' at position 12".

## The decisions that matter

### 1. How concurrent edits merge

| | Operational transformation (OT) | CRDT |
|---|---|---|
| Idea | The server orders operations and transforms each one against those it missed | A data type designed so that copies merge to the same result in any order |
| Needs a central server | Yes | No |
| Offline editing | Limited | Natural |
| Cost | Complex transformation logic | Extra metadata per character, larger documents |

CRDT stands for conflict-free replicated data type. A worked case shows the problem both solve. The text is "cat". User A inserts "s" at position 3, and user B inserts "a " at position 0 at the same moment. If B's client applies A's operation unchanged, the "s" lands in the wrong place. OT shifts A's position by 2. A CRDT avoids positions altogether by giving every character a stable identifier.

State one choice and its reason. A product with a reliable server and no offline mode can use OT. A product that must work offline should use a CRDT.

### 2. Local edits first

The editor applies each keystroke to the local model immediately and sends it afterward. Typing never waits for the network. Operations wait in a pending queue until the server acknowledges them, and remote operations are adjusted against that queue.

### 3. Presence

Cursor positions change constantly and do not need to be stored. Send them on a separate channel, at most about 10 times per second, and discard them when a user disconnects. A remote cursor must be expressed relative to the document content, so it moves correctly when text is inserted before it.

### 4. Undo

Undo must reverse only the current user's own changes. A global history would undo a colleague's work. Each client therefore keeps its own stack of operations and their inverses.

## Optimizations to mention

- Batch keystrokes into one message every 50 to 100 milliseconds.
- Load a snapshot of the document first, then only the operations after it.
- Render only the visible pages of a long document.
- Keep unsent operations in IndexedDB, so a closed tab loses nothing.

## What the interviewer probes

- What happens after 30 minutes offline? The client sends its queue, and the merge runs; with a CRDT this needs no special case.
- How is rich text pasted safely? Sanitize it against an allowlist before it enters the model.
- How do screen reader users learn about remote edits? Announce them sparingly through a live region.
- How does version history work? Periodic snapshots plus the operation log between them.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-collaborative-editor) has a lesson on OT and CRDTs and the complete collaborative document editor case study.
- Related pages: [real-time updates](../concepts/real-time-updates.md), [offline and storage](../concepts/offline-and-storage.md).
