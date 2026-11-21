# State Management

State is any data that can change while the page is open. The main design question is where each piece of state should live. Most problems with client state come from placing it in the wrong location, and few come from the choice of library.

## The five places state can live

| Location | Example | Survives a reload | Shareable by link |
|---|---|---|---|
| Component state | Whether a dropdown is open | No | No |
| Shared client store | The items in a cart | No | No |
| Server state cache | The list of products returned by the API | No, but it refetches | No |
| URL | The active filter, the page number, the search query | Yes | Yes |
| Browser storage | A draft message, the theme preference | Yes | No |

A server state cache is a client-side copy of data that the server owns. It tracks loading, errors, and freshness for each request, and it removes most of the code that a general store would otherwise need.

## How to choose

1. If only one component reads the value, keep it in that component.
2. If the value should survive a reload or be shared by link, put it in the URL.
3. If the value comes from the server, keep it in a server state cache.
4. If several distant components read and write a client-only value, use a shared store.
5. If the value must survive closing the tab, write it to browser storage.

## Normalization

A normalized store keeps each entity once, keyed by its identifier. Lists hold identifiers only.

```js
{
  posts: { p1: { id: "p1", authorId: "u7", likes: 12 } },
  users: { u7: { id: "u7", name: "Ada" } },
  feed: ["p1", "p4", "p9"]
}
```

When the like count of post `p1` changes, one write updates the feed, the detail view, and the profile page. Without normalization, the same post exists in three lists, and the three copies drift apart.

## Derived state

Derived state is a value computed from other state, such as a cart total. Compute it when it is read, and do not store it. A stored total needs an update on every change to the cart, and one missed update shows the user a wrong number.

## Optimistic updates

An optimistic update changes the screen before the server confirms the action. The user taps "like" and the count rises at once. The design needs three parts:

| Part | Purpose |
|---|---|
| A snapshot of the previous value | Allows a rollback |
| A pending marker | Prevents a refetch from overwriting the change |
| A failure path | Restores the snapshot and tells the user |

Optimistic updates suit actions that succeed more than 99 percent of the time and are cheap to undo. They do not suit payments.

## What the interviewer probes

- What happens when two tabs change the same data? One answer is a storage event or a broadcast channel that tells the other tab to refetch.
- How does the cache know that data is stale? Name a freshness time, such as 30 seconds, and a refetch when the window regains focus.
- Why not put everything in one global store? Every write then notifies many components, and server data gains no loading or error tracking.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-state-management) covers where state lives, normalization, server state caches, and rollback in a full chapter.
- [Grokking React Fundamentals](https://www.designgurus.io/course/grokking-react-fundamentals?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-state-management) teaches component state from the start.
- [Data fetching and caching](data-fetching-and-caching.md) continues with the server side of this topic.
- Free answer: [What Is Redux vs useState?](https://www.designgurus.io/answers/detail/what-is-redux-vs-usestate?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-state-management), a comparison of a global store with local component state.
- Free answer: [What Is Redux in React?](https://www.designgurus.io/answers/detail/what-is-a-redux-in-react?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-state-management), an introduction to the store, actions, and reducers.
