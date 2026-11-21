# Design an Image Gallery

An image gallery shows a grid of thumbnails and opens a larger view of one image at a time. Images are the heaviest content on most pages, so this question tests whether a candidate can deliver the right number of bytes to each device and keep the layout from shifting.

## Requirements to clarify

| Question | Why it matters |
|---|---|
| How many images: 30, 3,000, or unlimited? | Decides pagination and virtualization |
| Is the grid uniform, or a masonry layout with mixed heights? | Decides the layout algorithm |
| Is there a full-screen view with next and previous? | Adds preloading, focus handling, and gestures |
| Can users upload images? | Adds client-side resizing and progress |
| Which devices and connection speeds? | Decides image sizes and formats |

A masonry layout places items of different heights into columns with no gaps between rows.

Quality targets to state: first thumbnails visible within 2.5 seconds, Cumulative Layout Shift under 0.1, and smooth scrolling.

## Sketch

| Part | Responsibility |
|---|---|
| `Gallery` | Fetches pages of image records and owns the selection |
| `Grid` | Lays out thumbnails; virtualized for long galleries |
| `Thumbnail` | One lazy image with a placeholder |
| `Lightbox` | The full-screen view, as a modal dialog |

An image record holds `id`, `width`, `height`, `alt`, a small placeholder (a dominant color or a tiny blurred preview), and a base URL from which sized versions are requested.

The request is `GET /images?after=<cursor>&limit=40`.

## The decisions that matter

### 1. The right bytes for each device

A 4,000 pixel original is about 3 megabytes. A thumbnail shown at 300 pixels wide needs about 20 kilobytes. The `srcset` and `sizes` attributes let the browser choose among several widths, such as 320, 640, and 1,280 pixels, based on the layout and the screen density. An image service or CDN produces each width on request and serves a modern format such as AVIF or WebP where the browser supports it.

### 2. Layout stability

The server sends each image's width and height with the record. The grid reserves the exact space through an aspect ratio before the image loads, and shows the placeholder color in it. Nothing moves when the image arrives. In a masonry layout the same dimensions let the client assign each image to the shortest column before any image has loaded.

### 3. Lazy loading

Images below the visible area should not load until the user approaches them. Native `loading="lazy"` handles most cases. An intersection observer, which reports when an element nears the visible area, gives more control. The first row is the exception: load it eagerly, and give the largest image high priority.

### 4. The lightbox

- It is a modal dialog: focus moves in, stays in, and returns to the thumbnail on close.
- Left and Right arrows change the image, and Escape closes the view.
- Show the thumbnail scaled up at once, then replace it with the large version when it loads.
- Preload the next and previous images, and no more.
- Put the image identifier in the URL, so the view can be shared and the Back button closes it.

## Optimizations to mention

- Decode images off the main thread with `decoding="async"`.
- Virtualize the grid beyond a few hundred images, to limit memory.
- Cache image files with a long lifetime, since each URL is immutable.
- For uploads, resize on the client before sending, and show progress per file.

## What the interviewer probes

- What is the alternative text when users upload their own images? Ask for a description, and fall back to a generated one.
- How do swipe gestures work on a phone? Pointer events with a distance threshold, and the same actions on buttons.
- What happens when an image fails to load? Keep the reserved space and show a retry control.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-an-image-gallery) includes complete designs for an image carousel and a masonry photo grid.
- Related pages: [performance and Core Web Vitals](../concepts/performance-and-core-web-vitals.md), [accessibility](../concepts/accessibility.md).
