# Design Autocomplete

An autocomplete component shows suggestions while the user types in a search box. It is the most common component question, because a small widget contains three hard problems: limiting requests, discarding responses that arrive late, and supporting the keyboard and screen readers.

## Requirements to clarify

| Question | Why it matters |
|---|---|
| Where do suggestions come from: the server, a local list, or both? | Decides whether networking matters at all |
| How many suggestions are shown? | Usually 5 to 10; more needs scrolling |
| Are suggestions plain text or rich rows with images? | Decides the customization API |
| Is this one search box or a reusable library component? | Decides how general the contract must be |
| Should recent searches appear before the user types? | Adds local storage |

Quality targets to state: suggestions within 200 milliseconds of the last keystroke on a typical connection, no flicker between results, and full keyboard operation.

## Sketch

| Part | Responsibility |
|---|---|
| Input | Holds the text and the `combobox` role |
| Suggestion list | Renders options and the active option |
| Controller | Debounces, fetches, cancels, and caches |
| Cache | Maps a query string to its results |

State held by the controller: `query`, `status` (idle, loading, success, error), `items`, `activeIndex`, and `isOpen`.

The request is `GET /suggest?q=<text>&limit=10`. The component should accept a fetch function as a prop, so it does not depend on one endpoint.

## The decisions that matter

### 1. Debouncing

Debouncing waits until the user has stopped typing for a set time before sending a request. A user typing "bananas" at normal speed produces 7 keystrokes. Without debouncing, that is 7 requests. With a 200 millisecond wait, it is usually 1 or 2. A minimum length of 2 characters removes the least useful requests as well.

### 2. Responses that arrive out of order

The request for "ban" can return after the request for "bana". If the component simply shows whichever response arrives last, the user sees results for the wrong query. Two defenses work together:

- Cancel the previous request with an `AbortController` when a new one starts.
- Tag each request with its query, and ignore any response whose query is not the current one.

### 3. Caching

Users delete and retype characters often. A small in-memory map from query to results makes those cases instant. Limit it to about 50 entries and expire each after a few minutes, since suggestions change.

### 4. Keyboard and screen reader support

| Key | Behavior |
|---|---|
| Down and Up | Move the active option, wrapping at the ends |
| Enter | Select the active option |
| Escape | Close the list and keep the text |
| Tab | Close the list and move on |

Focus stays in the input throughout. `aria-activedescendant` tells the screen reader which option is active, and a live region announces the number of results.

## Optimizations to mention

- Highlight the matched part of each suggestion without inserting raw HTML, to avoid script injection.
- Show the previous results, dimmed, while new ones load, so the list does not collapse and reopen.
- Prefetch on focus when recent searches are known.
- On a phone, show the list full width and keep it above the on-screen keyboard.

## What the interviewer probes

- How does a consumer render a product image in each row? Through a `renderOption` prop.
- What happens when the request fails? Keep the typed text, show a one-line error in the list, and retry on the next keystroke.
- How would you test it? Simulated typing with timers, a slow response arriving after a fast one, and a keyboard-only pass.
- How does the server answer in 50 milliseconds? That is the backend half, usually a prefix index.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-autocomplete) designs the autocomplete in full, with the complete props table and accessibility markup.
- Lesson: [Design an Autocomplete](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/design-an-autocomplete?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-autocomplete), in Grokking the Modern Frontend System Design Interview
- [Grokking the System Design Interview](https://www.designgurus.io/course/grokking-the-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-autocomplete) covers the typeahead backend.
- Related pages: [component API design](../concepts/component-api-design.md), [accessibility](../concepts/accessibility.md).
