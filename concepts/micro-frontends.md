# Micro-Frontends

A micro-frontend architecture splits one web application into parts that separate teams build and deploy independently. It solves an organizational problem: many teams blocked by one shared release. It does not make a page faster, and it usually makes it slightly slower. Interviewers ask about it to see whether a candidate can name the costs.

## When it helps

| Signal | Micro-frontends help? |
|---|---|
| 5 engineers, one product | No. A single application is simpler |
| 30 engineers, releases blocked on each other weekly | Possibly. Try a monorepo with clear module ownership first |
| 100 or more engineers, distinct product areas, separate release schedules | Yes |
| Merging an acquired product with a different framework | Yes, at least during the migration |

A monorepo is one repository that holds many packages. With ownership rules and a build cache, it gives teams clear boundaries while keeping one deployment, and it is often sufficient.

## Ways to compose

| Method | How the parts combine | Trade-off |
|---|---|---|
| Route-based | Each URL prefix is a separate application | Simplest; a full page load between areas |
| Build-time packages | Each team publishes a package; one application imports them | One release again, so independence is lost |
| Runtime module loading | A shell loads each team's bundle at run time | Independent deploys; shared dependency versions must be managed |
| Server-side composition | A server assembles HTML fragments from each team | Fast first paint; more server infrastructure |
| Iframes | Each part runs in its own frame | Strongest isolation; poor for layout, focus, and performance |

The shell is the small host application that owns the page frame, the router, and sign-in, and that loads the other parts.

## The costs

| Cost | What happens | Mitigation |
|---|---|---|
| Duplicate code | Two teams ship two copies of the same library | Declare shared dependencies loaded once |
| Version conflicts | Two parts need different major versions of the framework | A shared upgrade schedule |
| Inconsistent interface | Buttons look different in each area | One design system, versioned |
| Cross-part communication | Parts import each other's internals | Custom events or a small typed event bus |
| Harder testing | A defect appears only when parts are combined | Contract tests and one integrated test environment |
| Slower pages | More requests and more JavaScript | A performance budget per part |

A design system is a shared set of components, styles, and rules. It is the main tool for keeping independently built parts visually consistent.

## Rules for the boundaries

- Split by product area, such as search, checkout, and account. Do not split by technical layer.
- Each part owns its routes, its state, and its API calls.
- Parts share only three things: the design system, the sign-in session, and a small set of events.
- No part reads another part's store.

## A worked example

An online store has 90 frontend engineers in 9 teams. All teams ship in one release every two weeks, and a defect in checkout delays the search team. The redesign creates a shell and four parts: search, product, checkout, and account. Each loads at run time and deploys on its own schedule. React and the design system load once as shared dependencies.

The teams now release daily. The cost is measurable: the first load grows by about 40 kilobytes for the loading code, and a platform team of 4 engineers maintains the shell and the shared versions.

## What the interviewer probes

- How does the cart count in the header update after checkout adds an item? Through an event that the shell listens for.
- How is one part rolled back? Each part is versioned, and the shell reads a manifest that maps part names to versions.
- What would you try first? A modular single application, because it costs less.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-micro-frontends) has a chapter on architecture at scale: design systems, micro-frontends, monorepos, feature flags, and server-driven UI.
- [Component API design](component-api-design.md) covers the shared component contract.
