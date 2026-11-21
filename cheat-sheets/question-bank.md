# Frontend System Design Question Bank

This page lists 40 questions in four groups. Component questions ask for one reusable widget. Application questions ask for a whole product surface. Platform questions ask for architecture across teams, and concept questions test one topic directly. Practice each design question with a 35-minute timer and [the framework](../framework.md).

## Component questions

The interviewer is measuring the component contract, state handling, and accessibility.

1. Design an autocomplete. ([walkthrough](../questions/design-autocomplete.md))
2. Design an image carousel.
3. Design a data table with sorting, filtering, and 100,000 rows.
4. Design a date range picker.
5. Design a modal dialog and a dropdown menu for a component library.
6. Design a rich text input with mentions.
7. Design an infinite scrolling list.
8. Design a file uploader with progress, pause, and resume.
9. Design a video player. ([walkthrough](../questions/design-a-video-player.md))
10. Design a toast notification system.
11. Design a multi-step form with validation and saved progress.
12. Design a star rating and review widget.

## Application questions

The interviewer is measuring architecture, data flow, and the choice of what to optimize.

13. Design a news feed. ([walkthrough](../questions/design-a-news-feed.md))
14. Design a chat app. ([walkthrough](../questions/design-a-chat-app.md))
15. Design a collaborative document editor. ([walkthrough](../questions/design-a-collaborative-editor.md))
16. Design a product page for an online store. ([walkthrough](../questions/design-a-product-page.md))
17. Design an image gallery or photo grid. ([walkthrough](../questions/design-an-image-gallery.md))
18. Design a real-time analytics dashboard. ([walkthrough](../questions/design-a-real-time-dashboard.md))
19. Design a checkout flow.
20. Design a travel search page with a map.
21. Design a kanban board with drag and drop.
22. Design a calendar app.
23. Design a live ride tracking screen.
24. Design an email client.
25. Design the interface of an AI chat assistant with streamed responses.
26. Design a video streaming site.
27. Design a spreadsheet.
28. Design a music player that continues across page navigation.

## Platform questions

These are more frequent at senior and staff levels. The interviewer is measuring judgment about cost across many teams.

29. Design a design system used by 40 teams.
30. How would you split one large frontend among 10 teams?
31. Design a feature flag system for the client.
32. Design client-side error monitoring and performance reporting.
33. Design an internationalization system for 30 languages.
34. Plan the migration of a large application from one framework to another without stopping feature work.

## Concept questions

These often appear as follow-ups inside a design question.

35. When would you choose server-side rendering over client-side rendering? ([page](../concepts/rendering-strategies.md))
36. How would you improve a Largest Contentful Paint of 4 seconds? ([page](../concepts/performance-and-core-web-vitals.md))
37. Where would you store an authentication token, and why? ([page](../concepts/security.md))
38. How do polling, server-sent events, and WebSockets compare? ([page](../concepts/real-time-updates.md))
39. How would you make a page work offline? ([page](../concepts/offline-and-storage.md))
40. How do you decide where a piece of state should live? ([page](../concepts/state-management.md))

## How to practice a question

| Step | Time | Output |
|---|---|---|
| Answer aloud with a timer | 35 minutes | A diagram, a store shape, one API, one component contract |
| Review against the framework | 10 minutes | The steps you skipped or rushed |
| Review against [common mistakes](common-mistakes.md) | 5 minutes | One correction for the next attempt |
| Compare with a reference answer | 15 minutes | Decisions you did not consider |

Recording the answer helps. Most candidates find on review that they stated fewer numbers than they believed.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-question-bank) contains complete reference answers for most questions on this page, across three case study chapters.
- [Grokking React Advanced](https://www.designgurus.io/course/grokking-react-advanced?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=cheat-sheets-question-bank) helps with the follow-up questions about rendering behavior.
