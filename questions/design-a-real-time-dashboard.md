# Design a Real-Time Dashboard

A real-time dashboard shows charts, counters, and tables that update while the user watches. The main difficulty is the gap between how often data arrives and how often the screen can usefully change. A design that draws every incoming value will freeze the page long before the network becomes a limit.

## Requirements to clarify

| Question | Why it matters |
|---|---|
| How fresh must the data be: 1 second, 10 seconds, 1 minute? | Decides the transport |
| How many widgets on screen, and how many data points per chart? | Decides the chart technology |
| Can users change the time range and filters? | Decides the query model and the URL state |
| Is the dashboard left open for hours on a wall screen? | Makes memory growth and reconnection critical |
| Are widgets configurable per user? | Adds a saved layout |

Quality targets to state: an update is visible within 2 seconds of the event, interactions respond within 200 milliseconds during updates, and memory stays flat over 8 hours.

## Sketch

| Part | Responsibility |
|---|---|
| Dashboard shell | Layout grid, time range, filters |
| Widget | One chart, counter, or table, with its own loading and error state |
| Subscription manager | One connection; routes each update to the widgets that need it |
| Series store | Recent points per metric, with a fixed maximum length |

Each widget loads history over HTTP, for example `GET /metrics/orders?from=...&to=...&step=10s`, and then receives new points over the live connection.

## The decisions that matter

### 1. The transport

| Freshness needed | Choice |
|---|---|
| 30 seconds or more | Polling |
| 1 to 10 seconds, server to client only | Server-sent events |
| Under 1 second, or the client changes subscriptions often | WebSocket |

Most dashboards only receive data, so server-sent events are a sound default. One connection carries all metrics, and the client tells the server which ones the visible widgets need.

### 2. Separating arrival from rendering

Updates enter a buffer. A render step runs at a fixed rate, such as once per second, and draws the latest state. If 40 updates arrive in that second, the screen changes once. Counters can animate between values to appear smooth. This single decision keeps the main thread free for user input.

### 3. Chart rendering

| Technology | Fits | Limit |
|---|---|---|
| SVG | Up to a few thousand points; easy styling and accessibility | Each point is an element, so large series slow down |
| Canvas | Tens of thousands of points | No built-in elements for interaction or screen readers |
| WebGL | Hundreds of thousands of points | Most complex to build |

A chart that is 800 pixels wide cannot show more than 800 distinct horizontal positions. Downsampling on the server reduces a series of 86,400 points (one per second for a day) to about 800 before sending it.

### 4. Bounded memory

Each series is a fixed-size buffer. When a new point arrives at a full buffer, the oldest point is removed. Without this rule, a dashboard left open overnight grows until the tab stops responding.

## Optimizations to mention

- Pause updates for widgets outside the visible area and for hidden tabs.
- After a reconnect, request the missed interval, so charts have no gaps.
- Keep the time range and filters in the URL, so a view can be shared.
- Give each widget its own error boundary, so one failed query does not blank the page.
- Provide a data table alternative to each chart for screen reader users.

## What the interviewer probes

- How does the user know the data is stale? Show the time of the last update, and mark it when the connection is down.
- How are 30 widgets loaded? Request visible widgets first and limit concurrent queries to about 6.
- What happens when the time range changes during loading? Cancel the pending requests and ignore late responses.
- Where should aggregation happen? On the server; the client should receive data that is ready to draw.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-real-time-dashboard) has a complete live analytics dashboard case study and a live ride tracking case study.
- Lesson: [Design a Live Analytics Dashboard](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/design-a-live-analytics-dashboard?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-real-time-dashboard), in Grokking the Modern Frontend System Design Interview
- Lesson: [Polling, Server Sent Events, and WebSockets](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/polling-server-sent-events-and-websockets?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-real-time-dashboard), in Grokking the Modern Frontend System Design Interview
- Related pages: [real-time updates](../concepts/real-time-updates.md), [performance and Core Web Vitals](../concepts/performance-and-core-web-vitals.md).
- Free answer: [How Would You Design a Real-Time Analytics Dashboard System?](https://www.designgurus.io/answers/detail/how-would-you-design-a-real-time-analytics-dashboard-system-handling-live-metrics-and-updates?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-real-time-dashboard), the data pipeline that feeds the dashboard.
