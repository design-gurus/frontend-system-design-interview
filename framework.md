# The Frontend System Design Framework

A frontend design answer has five steps: requirements, architecture, data model, interface, and optimizations. The order matters, because each step produces the input for the next one. A candidate who follows it reaches the deep technical discussion with about 15 minutes left, which is where most of the grading happens.

## The clock for a 45-minute round

| Minutes | Step | What you produce |
|---|---|---|
| 0 to 5 | Introductions | A short summary of your recent work |
| 5 to 10 | Requirements | A list of user actions and four to six numbers |
| 10 to 18 | Architecture | A box diagram of the client parts and the data flow |
| 18 to 23 | Data model | The entities and the shape of the client store |
| 23 to 30 | Interface | The key requests and responses, and the props of one component |
| 30 to 40 | Optimizations | Performance, accessibility, security, and the area the interviewer picks |
| 40 to 45 | Your questions | Two or three questions about the team |

Two checkpoints keep the round on schedule. The architecture diagram should be complete by minute 18, and the optimization discussion should begin by minute 30.

## Step 1: Requirements

State what the page must do and how well it must do it. Functional requirements are user actions: "a user types a query and sees up to 10 suggestions". Quality requirements are numbers.

Useful questions to ask:

- Which devices and browsers matter most, and how slow is the typical network?
- How many items does a typical user see: 20, 2,000, or 200,000?
- Must the page be indexed by search engines?
- Must it work offline or on an unreliable connection?
- Which languages and accessibility standards apply?

State the boundary as well. Say that the server API is treated as given, and that the design covers the client.

## Step 2: Architecture

Draw four to six boxes and the arrows between them. Most designs contain the same parts:

| Part | Its job |
|---|---|
| View components | Draw the screen and report user events |
| Client store | Holds state shared across components |
| Data layer | Sends requests, caches responses, retries failures |
| Server or CDN | Returns data and static files |

A content delivery network (CDN) is a set of servers near users that returns cached files. Name the rendering strategy at this step, because it decides where the first HTML comes from.

## Step 3: Data model

List the entities the client holds, such as `Post`, `User`, and `Comment`, with four or five fields each. Then show the store shape. A normalized store keeps each entity once, keyed by its identifier, so one update changes every place that shows it.

## Step 4: Interface

Define what crosses each boundary. Write one or two requests with their responses, including the pagination fields. Then write the props and events of the most important component. This step shows whether the candidate can turn a diagram into a contract that two engineers could build against.

## Step 5: Optimizations

Return to the numbers from step 1 and ask where the design will miss them. Pick two areas and go deep, or follow the interviewer's choice.

| Area | A typical statement |
|---|---|
| Performance | "A list of 5,000 rows needs virtualization, so only about 30 rows exist in the page at once" |
| Accessibility | "The suggestion list is a listbox, and arrow keys move the active option" |
| Security | "User content is escaped on output, and a content security policy blocks inline scripts" |
| Reliability | "A failed request retries twice with backoff, then shows a retry button" |

## A worked opening

For "design autocomplete", a strong first five minutes sounds like this: up to 10 suggestions, shown within 200 milliseconds of the last keystroke, usable with a keyboard and a screen reader, on a mid-range phone with a slow connection. Those four numbers drive every later choice.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=framework) teaches the five steps on a full example and applies them to complete case studies.
- [Common mistakes](cheat-sheets/common-mistakes.md) lists what goes wrong at each step.
- [Design autocomplete](questions/design-autocomplete.md) applies the method to one question.
