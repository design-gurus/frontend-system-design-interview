# Offline and Storage

Browser storage keeps data on the user's device, and offline support lets a page work without a network connection. The two topics belong together, because an offline design is mostly a decision about what to store, where to store it, and how to send changes to the server later.

## Storage options

| Option | Capacity | Access | Sent to the server | Typical use |
|---|---|---|---|---|
| Cookies | About 4 kilobytes each | Synchronous | Yes, on every request | Session identifiers |
| `localStorage` | About 5 megabytes | Synchronous | No | Small preferences, such as a theme |
| `sessionStorage` | About 5 megabytes | Synchronous | No | State for one tab |
| IndexedDB | Large, limited by a share of free disk space | Asynchronous | No | Structured data, drafts, offline queues |
| Cache Storage | Large, same limit | Asynchronous | No | Stored responses for files and API calls |

Synchronous access blocks the main thread while it reads or writes. That is acceptable for a 20-byte preference and harmful for a 2-megabyte list. Use IndexedDB for anything large.

The browser can delete stored data when the device runs short of space. A design must treat storage as a cache of the server's data, with one exception: unsent user changes, which the page should protect by requesting persistent storage.

## Service workers

A service worker is a script that the browser runs in the background, separate from the page. It intercepts network requests and can answer them from Cache Storage. It makes three caching policies possible.

| Policy | Behavior | Fits |
|---|---|---|
| Cache first | Answer from the cache; use the network only on a miss | Versioned scripts, styles, fonts |
| Network first | Try the network; answer from the cache on failure | HTML documents, API reads |
| Stale-while-revalidate | Answer from the cache and refresh it in the background | Avatars, product lists |

## Offline writes

Reading offline is the easier half. Writing offline needs a queue.

1. The user performs an action, such as adding a note.
2. The client applies it to the local store at once and writes it to an outbox in IndexedDB.
3. When the connection returns, the client sends the outbox entries in order.
4. Each entry carries an idempotency key, so a retry after a lost response does not create a duplicate.
5. The server response replaces the local temporary record.

An outbox is a durable list of changes that have not reached the server yet.

## Conflicts

Two devices can change the same record while one is offline. State which policy the product needs.

| Policy | Behavior | Fits |
|---|---|---|
| Last write wins | The later timestamp replaces the earlier one | Preferences, simple fields |
| Field-level merge | Changes to different fields are both kept | Forms, task records |
| Ask the user | Show both versions | Long text without real-time merging |
| CRDT | A data type whose copies merge automatically without conflicts | Collaborative documents |

## A worked example

A field inspection app is used in basements with no signal. An inspector completes 12 forms with photos during one visit. The app shell is cached by the service worker, forms are saved to IndexedDB, and photos wait in the outbox as files. When the phone reconnects, the outbox sends the 12 forms first and the photos after them, so the small, important data arrives even on a weak connection.

## What the interviewer probes

- How does the user know what is not saved to the server yet? Show a sync state per item: pending, sent, or failed.
- What happens when a new version of the app is deployed? The new service worker waits, and the page offers a reload.
- What must never be stored? Access tokens in `localStorage` are readable by any injected script.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-offline-and-storage) covers browser storage, offline-first sync, and merging concurrent edits.
- [Security](security.md) explains the token storage decision.
