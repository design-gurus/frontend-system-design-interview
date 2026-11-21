# Rendering Strategies

A rendering strategy decides where and when the HTML of a page is produced. The four common choices are client-side rendering, server-side rendering, static generation, and streaming. The right choice depends on three facts: whether the content is personal, how often it changes, and whether search engines must read it.

## The four strategies

| Strategy | Where HTML is built | When |
|---|---|---|
| Client-side rendering (CSR) | In the browser, by JavaScript | After the script downloads and runs |
| Server-side rendering (SSR) | On a server | On every request |
| Static site generation (SSG) | On a build machine | Once, before any request |
| Streaming SSR | On a server, sent in pieces | On every request, as each part becomes ready |

Incremental static regeneration is a variant of static generation. The server rebuilds a static page in the background after a set interval, so the page stays fresh without a full rebuild.

## Hydration

Hydration is the step where JavaScript attaches event handlers to HTML that the server already sent. Until hydration finishes, the page is visible but buttons do not respond. Server-rendered pages therefore show content early and become interactive later. The gap grows with the amount of JavaScript shipped.

Two techniques reduce that gap. Partial hydration attaches handlers only to the interactive parts of the page. Server components render on the server and send no JavaScript for themselves at all.

## Trade-offs

| Strategy | First content | Server cost | Search indexing | Fits |
|---|---|---|---|---|
| CSR | Slow: blank until the script runs | Lowest | Weak | Dashboards behind a sign-in |
| SSR | Fast | One render per request | Strong | Personalized or frequently changing pages |
| SSG | Fastest: served from a CDN | Near zero per request | Strong | Marketing pages, documentation, articles |
| Streaming SSR | Fast shell, slow parts arrive later | One render per request | Strong | Pages with one slow data source |

## The decision table

Ask the questions in order and stop at the first match.

| Question | If yes |
|---|---|
| Is the page the same for every user and does it change less than once an hour? | Static generation |
| Is it the same for every user but changes every few minutes? | Static generation with regeneration |
| Must search engines read it, and is it personal or always changing? | Server-side rendering |
| Does one section wait on a slow service? | Streaming, so the rest is not blocked |
| Is it behind a sign-in with no need for indexing? | Client-side rendering |

## A worked example

Consider a product page for an online store with 50,000 products. The title, images, and description are identical for all users and change about once a day. The price and stock level change every few minutes. The recommendations depend on the signed-in user.

A sound design generates the page statically and regenerates it every five minutes. The recommendations load on the client after the first paint. The page arrives from a CDN in well under one second, search engines read the full description, and no server renders 50,000 pages on each request.

## What the interviewer probes

- Why not render everything on the server? The answer is cost and time to first byte under load.
- What does the user see while a streamed section is pending? The answer is a placeholder with a reserved size.
- What breaks during hydration? A mismatch between server HTML and client output forces a re-render.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-rendering-strategies) has a full chapter on the browser pipeline, hydration cost, and choosing a strategy.
- [Grokking React Advanced](https://www.designgurus.io/course/grokking-react-advanced?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-rendering-strategies) covers rendering behavior in React.
- [Design a product page](../questions/design-a-product-page.md) applies this decision to one question.
