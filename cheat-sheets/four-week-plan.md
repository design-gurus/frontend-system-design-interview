# A Four-Week Plan for the Frontend System Design Interview

This plan takes about six hours a week for four weeks, or 24 hours in total. It suits an engineer who already builds web applications and has not practiced design rounds. The first half builds knowledge, and the second half is timed practice, because the round rewards fluency under a clock more than reading.

## The plan at a glance

| Week | Focus | Hours | Output by the end of the week |
|---|---|---|---|
| 1 | The method and the browser | 6 | You can explain the five steps and choose a rendering strategy |
| 2 | Data, state, and real time | 6 | You can write a store shape and an API contract in 10 minutes |
| 3 | Quality attributes and component questions | 6 | Three timed component answers |
| 4 | Application questions and mock rounds | 6 | Four timed application answers and two mock rounds |

## Week 1: the method and the browser

| Session | Work |
|---|---|
| 1 (2 hours) | Read [the framework](../framework.md) and [frontend vs backend system design](frontend-vs-backend-system-design.md). Write the five steps and the time budget from memory. |
| 2 (2 hours) | Read [rendering strategies](../concepts/rendering-strategies.md) and [performance and Core Web Vitals](../concepts/performance-and-core-web-vitals.md). |
| 3 (2 hours) | Measure a page you have built with the browser's performance tools. Record its three Core Web Vitals and name one cause for each. |

## Week 2: data, state, and real time

| Session | Work |
|---|---|
| 1 (2 hours) | Read [state management](../concepts/state-management.md) and [data fetching and caching](../concepts/data-fetching-and-caching.md). |
| 2 (2 hours) | Read [real-time updates](../concepts/real-time-updates.md) and [offline and storage](../concepts/offline-and-storage.md). |
| 3 (2 hours) | For three products you use daily, write the normalized store shape and one paginated request. Limit each to 10 minutes. |

## Week 3: quality attributes and component questions

| Session | Work |
|---|---|
| 1 (2 hours) | Read [accessibility](../concepts/accessibility.md), [security](../concepts/security.md), and [component API design](../concepts/component-api-design.md). |
| 2 (2 hours) | Answer "design autocomplete" aloud in 35 minutes. Then compare with [the walkthrough](../questions/design-autocomplete.md). |
| 3 (2 hours) | Answer two more component questions from [the question bank](question-bank.md), such as a data table and an image carousel. |

Operate one component you built with the keyboard only during this week. It is the fastest way to learn what the accessibility page describes.

## Week 4: application questions and mock rounds

| Session | Work |
|---|---|
| 1 (2 hours) | Answer "design a news feed" and "design a chat app" with a timer. Compare with the [walkthroughs](../questions/README.md). |
| 2 (2 hours) | Answer two more: a collaborative editor and a real-time dashboard. Read [micro-frontends](../concepts/micro-frontends.md) if the role is senior or above. |
| 3 (2 hours) | Two mock rounds with another engineer, 45 minutes each, one as candidate and one as interviewer. |

Acting as the interviewer is useful preparation. It shows how little of a monologue the listener retains, and how much a stated number helps.

## How to review each practice answer

Spend 15 minutes on review after every timed answer.

1. Check the clock: was the diagram complete by minute 18, and did optimizations begin by minute 30?
2. Count the numbers you stated in the first 10 minutes. The target is four to six.
3. Go through the checklist in [common mistakes](common-mistakes.md).
4. Write one sentence naming the single change for the next attempt.

## If you have less time

| Time available | Do this |
|---|---|
| One week | Week 1 session 1, then four timed answers: autocomplete, news feed, chat app, product page |
| Three days | The framework, common mistakes, and two timed answers |
| One day | The framework and one timed answer, reviewed against the checklist |

## Signs that you are ready

- You can finish a full answer in 35 minutes without checking notes.
- You state the three Core Web Vitals thresholds without hesitation.
- You can explain one choice among polling, server-sent events, and WebSockets in under a minute.
- You describe keyboard behavior for a component without being asked.
- Your last three practice answers each passed at least six items on the checklist.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-four-week-plan) follows the same order as this plan, with full lessons, complete case studies, chapter quizzes, and a final exam.
- [Grokking JavaScript Fundamentals](https://www.designgurus.io/course/grokking-javascript-fundamentals?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-four-week-plan) and [Grokking React Fundamentals](https://www.designgurus.io/course/grokking-react-fundamentals?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-four-week-plan) cover the prerequisites, if a gap appears during week 1.
