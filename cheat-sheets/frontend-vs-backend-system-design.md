# Frontend vs Backend System Design

Both rounds ask a candidate to design a system in about 45 minutes, and both reward clear requirements and named trade-offs. They differ in where the system runs. A backend design runs on servers that the company controls. A frontend design runs on devices that the company does not control, over networks it cannot predict, for a person who is watching the result.

## Side by side

| Dimension | Backend system design | Frontend system design |
|---|---|---|
| A typical question | "Design a URL shortener" | "Design autocomplete" or "design a news feed" |
| Where the code runs | Data centers with known hardware | Millions of different phones and laptops |
| The scarce resource | Server capacity and storage | The main thread, the network, and the battery |
| Key numbers | Requests per second, terabytes, replicas | Milliseconds to first content, kilobytes of JavaScript, frames per second |
| The diagram | Load balancers, services, databases, queues | Components, a client store, a data layer, the server boundary |
| The data model | Tables, partition keys, indexes | Client entities and the shape of the store |
| The interface | Service APIs | Requests and responses, plus component props and events |
| Failure to plan for | A node or region goes down | The network drops, a request is slow, a script fails |
| Quality attributes | Availability, consistency, durability | Performance, accessibility, security, usability |
| Scaling means | More users and more data | More features, more teams, and slower devices |

## What carries over

A candidate who has prepared for the backend round already has several skills that apply directly:

- Clarifying requirements before drawing.
- Stating assumptions as numbers.
- Caching, and the question of when cached data is stale.
- Pagination, retries, timeouts, and idempotency.
- Choosing between polling and a persistent connection.

## What is new

| Topic | Why backend preparation does not cover it |
|---|---|
| Rendering strategies | The choice of where HTML is built has no server-side equivalent |
| The browser pipeline | Layout, painting, and long tasks decide responsiveness |
| Client state | Component state, shared stores, URL state, and server state caches |
| Core Web Vitals | Targets are measured on real user devices at the 75th percentile |
| Accessibility | Keyboard, focus, and screen reader behavior are part of the design |
| Browser security | Cross-site scripting and content security policy are client concerns |
| Component contracts | Props, events, and composition for reusable widgets |

## The same question, two rounds

Consider "design a news feed".

| | Backend answer | Frontend answer |
|---|---|---|
| Opening question | How many users, and how many posts per day? | How many posts per session, on which devices? |
| Core problem | Building each user's feed: on write or on read | Rendering a long list of mixed content smoothly |
| Key decisions | Storage, fan-out, ranking, caching | Cursor pagination, virtualization, optimistic likes |
| Deep area | Handling an account with 50 million followers | Keeping scroll position stable as images load |

A candidate in a frontend round who spends 10 minutes on fan-out has answered a different question. State the boundary early: "I will treat the feed API as given and design the client."

## Full-stack loops

Some loops include both rounds, and some ask one question that spans both. In a spanning question, spend the first 5 minutes agreeing on the split. A common split is 60 percent on the side that matches the role and 40 percent on the other. Define the API contract between the halves first, since it is the one artifact both sides share.

## How expectations change by level

| Level | The round expects |
|---|---|
| Mid-level | A working design for the main flow, with correct fundamentals |
| Senior | Trade-offs stated with numbers, plus failure handling, accessibility, and performance |
| Staff | Architecture across teams: a design system, release safety, measurement, and migration |

These expectations are commonly reported and vary by company.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-frontend-vs-backend-system-design) opens with this comparison and builds the frontend method from it.
- Lesson: [Frontend System Design vs Backend System Design](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/frontend-system-design-vs-backend-system-design?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-frontend-vs-backend-system-design), in Grokking the Modern Frontend System Design Interview
- Lesson: [The Parts of a Web Client](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/the-parts-of-a-web-client?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-frontend-vs-backend-system-design), in Grokking the Modern Frontend System Design Interview
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-frontend-vs-backend-system-design) is the backend counterpart.
- The free [Grokking System Design](https://github.com/design-gurus/grokking-system-design) repository covers the backend round in the same format as this one.
- Free answer: [What Is the System Design Interview Like for Frontend Engineers?](https://www.designgurus.io/answers/detail/what-is-the-system-design-interview-like-for-frontend-engineers?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-frontend-vs-backend-system-design), a short description of the frontend round.
