# Common Mistakes in the Frontend System Design Interview

Most weak answers in this round fail in the same few ways. The candidate usually knows the material, and the problem lies in how the 45 minutes are spent. This page lists ten mistakes with the correction for each, in the order they tend to occur during the round.

## The ten mistakes

| # | Mistake | What it looks like | Correction |
|---|---|---|---|
| 1 | Skipping requirements | Drawing components in the first minute | Spend 5 minutes on user actions and four to six numbers |
| 2 | No numbers | "It should be fast" | "The largest element appears within 2.5 seconds on a mid-range phone" |
| 3 | Designing the backend | Ten minutes on databases and sharding | State the boundary early and treat the API as given |
| 4 | Naming tools without reasons | "We use GraphQL, micro-frontends, and a CRDT" | Name the problem and its number first, then the option and its cost |
| 5 | A library as the design | "I would use Redux" as the whole state answer | Describe where each piece of state lives, then name a tool as an example |
| 6 | No data model | Boxes and arrows with no entities | Write the store shape and one request with its response |
| 7 | Only the successful path | No loading, empty, or error states | For each request, say what the user sees while it loads and if it fails |
| 8 | Accessibility as a closing remark | "And it would be accessible" at minute 44 | State keyboard and screen reader behavior for the main component |
| 9 | A monologue | Twelve minutes without a question | Summarize and ask for direction at the end of each step |
| 10 | No time for depth | Still drawing boxes at minute 32 | Finish the diagram by minute 18 and begin optimizations by minute 30 |

## Why these cost so much

The interviewer writes feedback against a small set of signals: problem framing, technical depth, trade-off reasoning, and communication. Each mistake removes evidence for one of them.

| Signal | Mistakes that remove it |
|---|---|
| Problem framing | 1, 2, 3 |
| Technical depth | 6, 7, 10 |
| Trade-off reasoning | 4, 5 |
| Communication | 8, 9 |

## Weak and strong statements

| Topic | Weak | Strong |
|---|---|---|
| Rendering | "I would use server-side rendering because it is faster" | "The page is public and identical for all users, so I would generate it statically and regenerate every 5 minutes" |
| Lists | "We can add infinite scroll" | "A session reaches about 500 posts, so the list is virtualized and keeps about 30 rows in the page" |
| Real time | "We use WebSockets" | "The client only listens and a 2 second delay is acceptable, so server-sent events are enough" |
| State | "Everything goes in a global store" | "Filters live in the URL, server data in a cache with a 30 second fresh time, and the open menu in the component" |
| Errors | "We show an error" | "A failed like restores the previous count and shows a short message with a retry" |

Each strong statement has the same form: a fact about the product, then the decision, then the number.

## When the clock slips

If minute 18 arrives and the diagram is unfinished, say so and adjust. Complete the diagram in one more minute with fewer boxes. Shorten the data model to the two main entities. Protect the optimization step, since it carries the most signal. An explicit adjustment reads as good judgment, and silence followed by a rushed ending reads as poor planning.

## A self-review checklist

After each practice answer, confirm the following:

- I stated at least four numbers in the first 10 minutes.
- I said that the server was out of scope.
- My diagram had a view layer, a store, a data layer, and the server boundary.
- I wrote a store shape and one API contract.
- I described the loading, empty, and error states.
- I covered keyboard use for the main interaction.
- I went deep on two areas for about 5 minutes each.
- I asked for direction at least three times.

Fewer than six confirmed items means the answer needs another attempt on the same question before moving to a new one.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-common-mistakes) grades three answers to one question side by side, and shows how expectations differ by level.
- [The framework](../framework.md) gives the time budget that prevents mistakes 1, 9, and 10.
