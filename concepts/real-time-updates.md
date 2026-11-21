# Real-Time Updates

A real-time feature shows new data without a page reload. The client has three common ways to receive it: polling, server-sent events, and WebSockets. The choice depends on two facts: how fresh the data must be, and whether the client also needs to send frequent messages.

## The three options

| Option | How it works | Direction |
|---|---|---|
| Polling | The client sends a request on a timer, for example every 10 seconds | Client asks, server answers |
| Server-sent events (SSE) | The client opens one HTTP response that stays open; the server writes events to it | Server to client only |
| WebSocket | One connection upgraded from HTTP that both sides can write to at any time | Both directions |

Long polling is a variant of polling. The server holds each request open until it has data, then the client immediately asks again.

## Trade-offs

| | Polling | SSE | WebSocket |
|---|---|---|---|
| Delay before the user sees new data | Up to one interval | Under 1 second | Under 1 second |
| Automatic reconnection | Not needed | Built into the browser | Must be written |
| Works through proxies and firewalls | Always | Almost always | Usually |
| Server cost when nothing changes | One request per interval per user | One idle connection | One idle connection |
| Client sends frequent messages | Poor fit | Uses separate requests | Good fit |

## The decision

1. If a delay of 10 to 30 seconds is acceptable, use polling. It needs no new infrastructure.
2. If the server pushes and the client mostly listens, use server-sent events. Notifications, live scores, and streamed text responses fit here.
3. If both sides send several messages per second, use a WebSocket. Chat with typing indicators, collaborative editing, and multiplayer games fit here.

## What a production connection needs

A design that names WebSocket and stops is incomplete. The connection will drop, and the client must handle it.

| Concern | Design |
|---|---|
| Reconnection | Retry with exponential backoff and random jitter, capped at about 30 seconds |
| Missed messages | Send the last received sequence number on reconnect, and ask for everything after it |
| Dead connections | A heartbeat every 25 to 30 seconds detects a silent drop |
| Hidden tabs | Close or slow the connection when the tab is not visible |
| Many tabs | Share one connection across tabs through a shared worker or a broadcast channel |
| Fallback | Use long polling when the WebSocket cannot be opened |

Jitter is a small random delay added to each retry. It prevents thousands of clients from reconnecting in the same instant after a server restart.

## Rendering cost

Receiving data quickly does not require drawing it quickly. A dashboard that receives 50 updates per second should collect them and render once per animation frame, or even once per second. The human eye gains nothing from 50 changes of one number per second, and the main thread loses the time.

## A worked example

An order tracking page shows the courier on a map. The position changes every 5 seconds, and the client sends nothing. Server-sent events fit: one connection, automatic reconnection, and a `Last-Event-ID` header that lets the server resume from the last position sent. A WebSocket would add reconnection code without any benefit.

## What the interviewer probes

- What does the user see while the connection is down? Show a small "reconnecting" indicator and keep the last known data.
- How are duplicate messages handled? Each message carries an identifier, and the client ignores one it already holds.
- How does this scale on the client with 20 open tabs? One shared connection serves all of them.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-real-time-updates) compares the three transports with measurements and uses them in the chat, dashboard, and ride tracking case studies.
- [Design a chat app](../questions/design-a-chat-app.md) and [design a real-time dashboard](../questions/design-a-real-time-dashboard.md) apply this page.
- Free answer: [What Is Polling vs Long-Polling vs Webhooks?](https://www.designgurus.io/answers/detail/what-is-polling-vs-long-polling-vs-webhooks?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-real-time-updates), a comparison of three delivery methods.
- Free answer: [What Is HTTP Long Polling?](https://www.designgurus.io/answers/detail/what-is-http-long-polling?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-real-time-updates), a closer look at one of them.
