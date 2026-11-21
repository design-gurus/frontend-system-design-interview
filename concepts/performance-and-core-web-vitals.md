# Performance and Core Web Vitals

Core Web Vitals are three metrics that Google defines to measure how a page feels to a user: loading, responsiveness, and visual stability. They give a frontend design its quality targets. A candidate who states the three thresholds in the requirements step has numbers to design against for the rest of the round.

## The three metrics

| Metric | What it measures | "Good" threshold |
|---|---|---|
| Largest Contentful Paint (LCP) | Time until the largest image or text block is visible | 2.5 seconds or less |
| Interaction to Next Paint (INP) | Delay between a user action and the next visual update | 200 milliseconds or less |
| Cumulative Layout Shift (CLS) | How much visible content moves unexpectedly | 0.1 or less |

Google assesses each threshold at the 75th percentile of real page visits. A page passes when 75 percent of visits meet the number, which means slow phones and slow networks count.

## Fixing each metric

| Metric | Common cause | Fix |
|---|---|---|
| LCP | The main image is discovered late | Preload it, and give it high fetch priority |
| LCP | HTML arrives late | Render on the server or serve static HTML from a CDN |
| LCP | Render-blocking scripts and styles | Inline critical CSS, defer scripts |
| INP | A long task blocks the main thread | Split the work, or move it to a web worker |
| INP | Too many components re-render on each keystroke | Narrow the state, memoize, debounce |
| CLS | Images without dimensions | Set width and height, or an aspect ratio |
| CLS | Content inserted above existing content | Reserve the space with a placeholder |
| CLS | Fonts swap late | Preload the font and match the fallback metrics |

The main thread is the single thread that runs JavaScript, layout, and painting. A long task is any work that holds it for more than 50 milliseconds. A web worker is a background thread that runs JavaScript without blocking the page.

## Shipping less JavaScript

JavaScript costs time twice: once to download and once to parse and run. Three techniques reduce it.

1. Code splitting loads the code for a route or a dialog only when the user reaches it.
2. Tree shaking removes exports that nothing imports.
3. Lazy loading defers components below the visible area.

A useful budget for a first load is about 170 kilobytes of compressed JavaScript. Treat this figure as a planning number, since the right budget depends on the audience and devices.

## Long lists

Virtualization renders only the rows that are visible, plus a small buffer. A list of 10,000 rows then keeps about 30 row elements in the page. Scrolling replaces their content. Without it, the browser lays out all 10,000 rows and every interaction slows down.

## Lab data and field data

| Type | Source | Use |
|---|---|---|
| Lab data | A test run on one machine with a simulated network | Catching regressions before release |
| Field data | Measurements from real users | Knowing what users experience |

A design should include field measurement. The page reports the three metrics to an analytics endpoint, tagged by route and device class.

## A worked example

A product page has an LCP of 4.1 seconds on a mid-range phone. The largest element is the product photo, which is requested only after a 300 kilobyte script runs. Rendering the image tag on the server and preloading the photo lets the browser request it with the HTML. LCP falls to about 2.2 seconds, with no change to the script.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-performance-and-core-web-vitals) has a performance chapter with measured examples for each metric.
- [Grokking React Advanced](https://www.designgurus.io/course/grokking-react-advanced?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-performance-and-core-web-vitals) covers re-render control and code splitting in React.
- [Rendering strategies](rendering-strategies.md) explains the choice that affects LCP most.
- Free answer: [What Is Lazy Loading in React?](https://www.designgurus.io/answers/detail/what-is-lazy-loading-in-react?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-performance-and-core-web-vitals), with code for loading components on demand.
- Free article: [CDN Architecture in System Design: How Content Delivery Networks Work](https://www.designgurus.io/blog/content-delivery-network-cdn-system-design-basics?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-performance-and-core-web-vitals), which explains how static assets reach the browser quickly.
