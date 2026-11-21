# Design a News Feed

A news feed frontend shows an endless list of posts with text, images, and actions such as like and comment. The design is decided by three choices: how the list is paged, how many rows exist in the page at once, and how a like is shown before the server confirms it.

## Requirements to clarify

| Question | Why it matters |
|---|---|
| What can a post contain: text, images, video, links? | Decides row height and media loading |
| How many posts does a user scroll through in one session? | 50 needs no virtualization; 2,000 does |
| Do new posts appear while the user reads? | Decides the update transport and the insert behavior |
| Must the first screen be indexed by search engines? | Decides the rendering strategy |
| Which actions exist: like, comment, share, hide? | Each needs an optimistic path |

Quality targets to state: Largest Contentful Paint within 2.5 seconds, smooth scrolling at 60 frames per second, and an interaction response within 200 milliseconds.

## Sketch

| Part | Responsibility |
|---|---|
| `FeedPage` | Owns the route and starts the first request |
| `FeedList` | Virtualized list; requests the next page near the end |
| `PostCard` | Renders one post and reports actions |
| `Composer` | Creates a post |
| Data layer | Fetches pages, caches them, applies optimistic changes |

The client store is normalized: `posts` and `users` keyed by identifier, and a `feed` object holding the ordered post identifiers and the next cursor.

The main request is `GET /feed?after=<cursor>&limit=20`. The response returns the posts, the authors, and `nextCursor`.

## The decisions that matter

### 1. Cursor pagination

New posts arrive at the top while the user scrolls. With offset pagination, the next page would repeat posts that have shifted down. A cursor marks the last post received, so each page continues exactly where the previous one ended.

### 2. Virtualization with variable heights

After 500 posts, a plain list holds thousands of elements, and scrolling slows. A virtualized list keeps only the visible rows plus a buffer of a few rows on each side. Posts differ in height, so the list estimates each height first and measures it after render. Images must reserve their space through an aspect ratio, or measured heights change and the scroll position jumps.

### 3. Optimistic actions

A like updates the count at once. The data layer keeps the previous value and restores it if the request fails. It also ignores a background refetch for that post until the request settles.

### 4. New posts

Inserting new posts above the reader moves the content they are reading. The usual design shows a "5 new posts" button, and inserts them only when the user taps it or is already at the top.

## Optimizations to mention

- Load images lazily, with a small blurred placeholder.
- Fetch the next page when the user is about three screens from the end, before they reach it.
- Restore the scroll position when the user returns from a post detail page.
- Start video only when at least half of it is visible, and pause it when it leaves the screen.

## What the interviewer probes

- How is the scroll position restored after navigation? Keep the cached pages and the offset in memory.
- What does a screen reader user experience? Use the `feed` role and offer a "load more" button.
- What happens to memory after two hours of scrolling? Drop the oldest pages from the store and refetch them on demand.
- How is a failed like shown? Restore the count and show a short message.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-news-feed) has the complete news feed case study with the store shape, the API contract, and measurements.
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-news-feed) covers the server side: feed generation and ranking.
- Related pages: [data fetching and caching](../concepts/data-fetching-and-caching.md), [state management](../concepts/state-management.md).
