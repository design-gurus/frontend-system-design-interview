# Component API Design

A component API is the contract between a component and the code that uses it: its props, its events, and the parts it lets a consumer replace. Component questions such as autocomplete or a data table are graded heavily on this contract. A good API makes the common case one line and the unusual case possible.

## The four parts of the contract

| Part | Question it answers | Example for an autocomplete |
|---|---|---|
| Data in | What does the consumer supply? | `value`, `fetchSuggestions`, `minLength` |
| Events out | What does the component report? | `onChange`, `onSelect`, `onOpenChange` |
| Customization | What can be replaced? | `renderOption`, `renderEmpty` |
| Behavior options | What can be tuned? | `debounceMs`, `maxResults` |

## Controlled and uncontrolled

A controlled component receives its value from the parent and reports every change. An uncontrolled component keeps its own value and reports only when asked.

| | Controlled | Uncontrolled |
|---|---|---|
| Props | `value` and `onChange` | `defaultValue` |
| Who owns the state | The parent | The component |
| Fits | Forms with validation, values synchronized with the URL | Simple, isolated inputs |

Supporting both is common. If `value` is supplied, the component is controlled. Otherwise it starts from `defaultValue` and manages itself.

## Three levels of customization

1. Props for common variations: `size="small"`, `disabled`.
2. Render functions or slots for content: `renderOption={(item) => ...}`.
3. Composition for structure: the consumer assembles sub-components.

```jsx
<Autocomplete value={query} onChange={setQuery} onSelect={goToProduct}>
  <Autocomplete.Input placeholder="Search products" />
  <Autocomplete.List>
    {(item) => <Autocomplete.Option value={item}>{item.name}</Autocomplete.Option>}
  </Autocomplete.List>
</Autocomplete>
```

A headless component provides behavior, state, and accessibility with no styling. The consumer supplies all markup. It suits a design system that serves several products with different visual styles.

## A sample contract

| Prop | Type | Default | Purpose |
|---|---|---|---|
| `value` | string | none | The current input text (controlled) |
| `onChange` | (text) => void | none | Called on each keystroke |
| `fetchSuggestions` | (text, signal) => Promise of items | required | Returns suggestions; `signal` cancels a stale request |
| `onSelect` | (item) => void | none | Called when an option is chosen |
| `debounceMs` | number | 200 | Wait after the last keystroke before fetching |
| `minLength` | number | 2 | Characters required before fetching |
| `renderOption` | (item, state) => node | text label | Custom option content |

Passing a fetch function keeps the component independent of any API. The same component can then search products, users, or cities.

## Rules that prevent later pain

- Prefer one prop with named values over several booleans. `variant="primary"` is clearer than `primary`, `secondary`, and `danger` as three flags that can conflict.
- Name events by what happened: `onSelect`, not `onClick`.
- Do not expose internal state that you may need to change.
- Build accessibility into the component, so consumers cannot omit it.
- Forward `className`, `id`, and a ref to the root element.
- Treat a removed or renamed prop as a breaking change, with a version increase and a migration note.

## What the interviewer probes

- How does a consumer show a product image in each option? Through `renderOption`.
- How is the component used in two products with different styles? Through tokens or a headless core.
- How would 40 teams upgrade? Through versioning, deprecation warnings, and automated code changes.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-component-api-design) designs six UI components in full, including a data table and a date picker.
- [Grokking Modern API Design Interview](https://www.designgurus.io/course/grokking-modern-api-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-component-api-design) applies the same contract thinking to server APIs.
- [Grokking React Advanced](https://www.designgurus.io/course/grokking-react-advanced?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-component-api-design) covers composition patterns in React.
