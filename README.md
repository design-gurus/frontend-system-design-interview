# Frontend System Design Interview

> The free, open companion to **[Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme)** by DesignGurus.io, created by Arslan Ahmad.

[![GitHub stars](https://img.shields.io/github/stars/design-gurus/frontend-system-design-interview?style=social)](https://github.com/design-gurus/frontend-system-design-interview/stargazers)
[![Last commit](https://img.shields.io/github/last-commit/design-gurus/frontend-system-design-interview)](https://github.com/design-gurus/frontend-system-design-interview/commits/main)
[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](LICENSE)
[![PRs welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

A frontend system design interview asks a candidate to design the client side of a product: the part that runs in the browser. The server is usually treated as given. The round measures how the candidate handles rendering, client state, the network boundary, performance, accessibility, and security inside a 45-minute conversation.

This repository is a short, free guide to that round. It contains one method, ten concept pages, eight question walkthroughs, and four cheat sheets. Each page reads in about three minutes and ends with a link to deeper material.

## Start here

| Your situation | Read this first |
|---|---|
| The interview is this week | [The framework](framework.md), then [common mistakes](cheat-sheets/common-mistakes.md) |
| You know backend system design and are new to this round | [Frontend vs backend system design](cheat-sheets/frontend-vs-backend-system-design.md) |
| You want practice questions | [The question bank](cheat-sheets/question-bank.md) and the [walkthroughs](questions/README.md) |
| You have a month | [The four-week plan](cheat-sheets/four-week-plan.md) |
| One topic is weak | [The concept index](concepts/README.md) |

## The framework in one table

The method has five steps. The minutes assume a 45-minute round with 5 minutes of introductions and 5 minutes for the candidate's own questions.

| Step | The question it answers | Minutes |
|---|---|---|
| 1. Requirements | What must the page do, for whom, and how well? | 5 |
| 2. Architecture | Which client parts exist, and how does data move between them? | 8 |
| 3. Data model | What data does the client hold, and in what shape? | 5 |
| 4. Interface | What crosses each boundary: requests, responses, props, and events? | 7 |
| 5. Optimizations | Where will the design miss a target, and what fixes it? | 10 |

The full method, with what to say at each step, is in [framework.md](framework.md).

## All pages

### Concepts

| Page | What it covers |
|---|---|
| [Rendering strategies](concepts/rendering-strategies.md) | Client rendering, server rendering, static generation, and streaming, with a decision table |
| [State management](concepts/state-management.md) | The five places state can live, and how to choose among them |
| [Data fetching and caching](concepts/data-fetching-and-caching.md) | API styles, pagination, the client cache, and HTTP caching |
| [Performance and Core Web Vitals](concepts/performance-and-core-web-vitals.md) | The three metrics, their thresholds, and the fixes for each |
| [Real-time updates](concepts/real-time-updates.md) | Polling, server-sent events, and WebSockets compared |
| [Offline and storage](concepts/offline-and-storage.md) | Browser storage options, service workers, and sync |
| [Accessibility](concepts/accessibility.md) | Keyboard, focus, semantics, and contrast as design requirements |
| [Security](concepts/security.md) | Cross-site scripting, cross-site request forgery, and content security policy |
| [Component API design](concepts/component-api-design.md) | Props, events, controlled state, and composition |
| [Micro-frontends](concepts/micro-frontends.md) | When to split a frontend by team, and what it costs |

### Question walkthroughs

| Page | The decision that matters most |
|---|---|
| [Design a news feed](questions/design-a-news-feed.md) | Cursor pagination and list virtualization |
| [Design autocomplete](questions/design-autocomplete.md) | Debouncing, out-of-order responses, and keyboard support |
| [Design a chat app](questions/design-a-chat-app.md) | The connection, message ordering, and optimistic sends |
| [Design an image gallery](questions/design-an-image-gallery.md) | Responsive images and layout stability |
| [Design a collaborative editor](questions/design-a-collaborative-editor.md) | Merging concurrent edits |
| [Design a product page](questions/design-a-product-page.md) | The rendering strategy and the largest image |
| [Design a video player](questions/design-a-video-player.md) | Adaptive bitrate streaming and buffering |
| [Design a real-time dashboard](questions/design-a-real-time-dashboard.md) | Update rate against rendering cost |

### Cheat sheets

| Page | What it covers |
|---|---|
| [Frontend vs backend system design](cheat-sheets/frontend-vs-backend-system-design.md) | The two rounds compared across eight dimensions |
| [Question bank](cheat-sheets/question-bank.md) | 40 questions grouped by type |
| [Common mistakes](cheat-sheets/common-mistakes.md) | Ten mistakes and the correction for each |
| [Four-week plan](cheat-sheets/four-week-plan.md) | A weekly schedule at about six hours a week |

## FAQ

### What is a frontend system design interview?

It is a design round for frontend and full-stack engineers. The candidate designs the client side of a feature or product, such as a news feed or an autocomplete box. The interviewer grades the structure of the answer, the trade-offs named, and the depth reached in one or two areas.

### How is frontend system design different from backend system design?

The backend round is about data stores, partitioning, and server capacity. The frontend round is about rendering, client state, the network boundary, and what the user perceives. The numbers also differ: milliseconds per interaction and kilobytes of JavaScript replace requests per second. The [comparison page](cheat-sheets/frontend-vs-backend-system-design.md) lists the differences.

### What questions are asked in a frontend system design interview?

Questions fall into two families. Component questions ask for one widget, such as autocomplete, a data table, or an image carousel. Application questions ask for a whole product surface, such as a news feed, a chat app, or a collaborative editor. The [question bank](cheat-sheets/question-bank.md) lists 40 of them.

### Do I need to write code in a frontend system design interview?

Usually very little. Most rounds are run on a whiteboard or a shared document. Candidates are commonly asked to write the props of one component, the shape of the client store, or one request and response. Full implementations belong to a separate coding round.

### Which framework should I use in a frontend system design interview?

The design should not depend on one library. Name the concept first (a server state cache, a virtualized list, a streaming render), then mention the tool you know as one example. Interviewers grade the reasoning, and the reasoning is the same in React, Vue, Angular, or Svelte.

### How long does it take to prepare?

About four weeks at six hours a week is enough for an engineer who already builds web applications daily. The [four-week plan](cheat-sheets/four-week-plan.md) splits that time across concepts, walkthroughs, and timed practice.

## Free reading on DesignGurus.io

- [Frontend System Design Interview: What It Asks and How to Answer It](https://www.designgurus.io/blog/frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): a longer overview of the round and how to answer it.
- [What Is the System Design Interview Like for Frontend Engineers?](https://www.designgurus.io/answers/detail/what-is-the-system-design-interview-like-for-frontend-engineers?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): a short description of the format and the scoring.
- [OpenAI Frontend Engineer Interview: Rounds, Questions, and How to Prepare](https://www.designgurus.io/blog/openai-frontend-engineer-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): one company's frontend loop, round by round.
- [How to Design a Real-Time Chat Application](https://www.designgurus.io/blog/design-chat-application?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): the backend side of the chat question.
- [How to Design a Real-Time Collaborative Document Editor](https://www.designgurus.io/blog/design-real-time-editor?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): the full system behind a collaborative editor.
- [How to Design a Social Media News Feed](https://www.designgurus.io/blog/design-social-media-news-feed?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): the backend side of the news feed question.
- [What Is Polling vs Long-Polling vs Webhooks?](https://www.designgurus.io/answers/detail/what-is-polling-vs-long-polling-vs-webhooks?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): a comparison of delivery methods for live updates.

## Go deeper

This repository stays at the level of approach and trade-offs. The full course teaches every topic here with diagrams, measured examples, complete case studies, and quizzes.

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): the companion course, covering the method, the browser, state, performance, and full case studies.
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): the backend counterpart, for full-stack loops that include both rounds.
- [Grokking React Advanced](https://www.designgurus.io/course/grokking-react-advanced?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=readme): rendering behavior and performance patterns in React.

Related free repositories: [Grokking System Design](https://github.com/design-gurus/grokking-system-design) for the backend round, and [API Design Interview](https://github.com/design-gurus/api-design-interview) for the interface step.

## Contributing

Corrections and new questions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request.

## License

The content is licensed under [CC BY 4.0](LICENSE). See [NOTICE.md](NOTICE.md) for what the license covers and how to attribute.
