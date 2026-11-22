# Design a Product Page

A product page shows one item of an online store: images, title, price, options such as size and color, stock, reviews, and an "add to cart" button. Most visitors arrive from a search engine on a phone, so the page is graded on how fast the first view appears and on whether search engines can read it.

## Requirements to clarify

| Question | Why it matters |
|---|---|
| How many products exist, and how often do they change? | Decides between static generation and server rendering |
| How often do price and stock change? | Decides which fields may be cached |
| Is any content personal: recommendations, a member price? | Personal content cannot be cached for everyone |
| Do options change the price, the images, and the URL? | Decides the state model |
| Which regions, currencies, and languages? | Decides caching keys and formatting |

Quality targets to state: Largest Contentful Paint within 2.5 seconds on a mid-range phone, Cumulative Layout Shift under 0.1, and a response to "add to cart" within 200 milliseconds.

## Sketch

| Part | Rendered where | Data |
|---|---|---|
| Image gallery | Server HTML, first image prioritized | Image records with dimensions |
| Title, description, options | Server HTML | Product record |
| Price and stock | Server HTML, refreshed on the client | Price service |
| Add to cart | Interactive on the client | Cart store |
| Reviews | Loaded when scrolled near | Paged reviews |
| Recommendations | Loaded on the client | Per-user list |

The product record holds `id`, `title`, `description`, `images`, `options`, and `variants`. A variant is one purchasable combination, such as "blue, medium", with its own price, stock, and images.

## The decisions that matter

### 1. The rendering strategy

Product content is the same for all visitors and changes rarely, so the page can be generated statically and regenerated on a schedule or when the product is edited. A CDN then serves the HTML in well under a second. Client-side rendering alone would delay the first view and weaken search indexing. Server rendering on every request would work but would cost a render per visit for content that seldom changes.

### 2. Fresh price and stock

A cached page can show a price that is five minutes old. Two defenses apply together. The client refetches price and stock for the selected variant after load, and reserves space so the update does not shift the layout. The server validates the price again when the item is added to the cart, since the client is never the authority.

### 3. The largest image

The main product photo is almost always the largest element. Put it in the server HTML with `srcset`, explicit dimensions, and high fetch priority. Load every other gallery image lazily.

### 4. Option selection

The selected variant belongs in the URL, for example `?color=blue&size=m`. The page is then shareable, the Back button works, and a link from an advertisement can open the exact variant. All variants arrive with the page, so changing an option needs no request.

## Optimizations to mention

- Add structured data (a machine-readable description of the product) so search engines can show price and rating.
- Update the cart optimistically, and restore it with a message if the request fails.
- Defer reviews, recommendations, and third-party scripts until after the first view.
- Format prices and dates with the browser's internationalization API for the visitor's locale.

## What the interviewer probes

- What does the user see when an option is out of stock? The option stays visible and is marked unavailable, including for screen readers.
- How is a sale that starts at midnight handled? Regenerate on the price event, and rely on the client refresh for the gap.
- How do 50,000 product pages get built? Generate the top sellers at build time and the rest on first request.
- What breaks the layout shift target? Late banners, images without dimensions, and review widgets without reserved height.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-product-page) uses an online grocery store as its running example and includes a full store checkout case study.
- Lesson: [Design an Online Store Checkout](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/design-an-online-store-checkout?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-product-page), in Grokking the Modern Frontend System Design Interview
- Lesson: [Images, Fonts, and the Critical Path](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/images-fonts-and-the-critical-path?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-product-page), in Grokking the Modern Frontend System Design Interview
- Related pages: [rendering strategies](../concepts/rendering-strategies.md), [performance and Core Web Vitals](../concepts/performance-and-core-web-vitals.md).
- Free article: [Design a Scalable E-Commerce Platform](https://www.designgurus.io/blog/design-scalable-e-commerce-platform?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=questions-design-a-product-page), the backend services that a product page calls.
