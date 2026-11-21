# Data Fetching and Caching

Data fetching covers how the client asks the server for data, how it pages through long lists, and how it avoids asking twice. A good answer names the API style, the pagination method, and two caching layers: one in the application and one in HTTP.

## API styles

| Style | How the client asks | Strength | Cost |
|---|---|---|---|
| REST | One URL per resource | Simple, cacheable by URL | Several requests for one screen |
| GraphQL | One query naming the exact fields | One request per screen, no unused fields | Harder HTTP caching, more server work |
| Backend for frontend | A server endpoint shaped for one screen | Smallest client code | One more service to maintain |

A backend for frontend is a thin server layer owned by the frontend team. It combines calls to several services into one response for one screen.

## Pagination

| Method | The request | Problem |
|---|---|---|
| Offset | `?offset=40&limit=20` | Items shift when new ones arrive, so rows repeat or disappear |
| Cursor | `?after=abc123&limit=20` | No jumping to page 50 |

A cursor is an opaque marker for the last item the client received. Feeds and chat histories should use cursors, because new items arrive while the user scrolls. Tables with page numbers can use offsets.

## The client cache

A server state cache stores each response under a key, such as `["products", { category: "fruit" }]`. Three settings define its behavior:

| Setting | Meaning | A typical value |
|---|---|---|
| Fresh time | How long a response is served without a refetch | 30 seconds |
| Cache time | How long an unused response stays in memory | 5 minutes |
| Refetch triggers | Events that cause a background refetch | Window focus, reconnect |

Stale-while-revalidate is the usual policy. The cache returns the stored response at once, refetches in the background, and updates the screen if the data changed. The user sees content immediately, and it becomes correct within one round trip.

Deduplication is the second benefit. If five components request the same key in the same moment, the cache sends one request.

## HTTP caching

| Header | Effect |
|---|---|
| `Cache-Control: max-age=31536000, immutable` | The browser reuses the file for a year without asking |
| `Cache-Control: no-cache` | The browser must revalidate before each use |
| `ETag` | A version tag; the server answers 304 with no body if nothing changed |

Static files should carry a content hash in the file name, such as `app.3f9a1c.js`. A new build produces a new name, so the long cache lifetime is safe. The HTML document itself should use `no-cache`, because it points to the current file names.

## Failures

Every request can fail. State the policy:

- A timeout, for example 8 seconds.
- Two retries with exponential backoff (waiting 1 second, then 2 seconds) for read requests.
- No automatic retry for writes unless the request carries an idempotency key, which is an identifier that lets the server ignore a duplicate.
- A visible error with a retry button after the last attempt.

## A worked example

A product list shows 20 items per page across 400 categories. With a fresh time of 30 seconds, a user who opens a category, views a product, and returns within 30 seconds triggers no second request. After 30 seconds, the list appears immediately from the cache and refreshes in the background.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-data-fetching-and-caching) has a chapter on the network boundary: API styles, pagination, retries, and HTTP caching.
- [Grokking Modern API Design Interview](https://www.designgurus.io/course/grokking-modern-api-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-data-fetching-and-caching) covers the server side of the same contract.
- [State management](state-management.md) explains where the cached data lives.
- Free answer: [What Is React Query?](https://www.designgurus.io/answers/detail/what-is-a-react-query?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-data-fetching-and-caching), an introduction to one widely used client cache library.
