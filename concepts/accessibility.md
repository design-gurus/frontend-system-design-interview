# Accessibility

Accessibility means that people can use the page with a keyboard, a screen reader, voice control, or enlarged text. In a design round it is a requirement to state in step 1 and a design to show in step 5. Candidates who mention it only as a closing remark receive little credit for it.

## Four requirements to state

| Requirement | The test |
|---|---|
| Keyboard operation | Every action works with Tab, Enter, Space, Escape, and arrow keys |
| Visible focus | The focused element is always visibly marked |
| Semantics | A screen reader announces the role, name, and state of each control |
| Contrast and size | Text contrast is at least 4.5 to 1, and the layout holds at 200 percent zoom |

A screen reader is software that reads the page aloud and lets the user move through it by element. The contrast and zoom figures come from the Web Content Accessibility Guidelines (WCAG) at level AA, the standard most companies target.

## Native elements first

A native `<button>` is focusable, responds to Enter and Space, and announces itself as a button. A `<div>` with a click handler does none of these. The first rule is to use the native element wherever one exists: `<button>`, `<a>`, `<input>`, `<select>`, `<dialog>`, `<table>`.

ARIA (Accessible Rich Internet Applications) is a set of HTML attributes that describe roles and states to assistive technology. It is needed only when no native element fits, as in a custom autocomplete.

## Patterns for common components

| Component | Roles and attributes | Keyboard |
|---|---|---|
| Autocomplete | `combobox` on the input, `listbox` and `option` on the list, `aria-activedescendant` for the active option | Arrows move, Enter selects, Escape closes |
| Modal dialog | `dialog` with `aria-modal` and a label | Focus moves in, stays in, and returns to the trigger on close |
| Tabs | `tablist`, `tab`, `tabpanel`, `aria-selected` | Arrows change tab, Tab enters the panel |
| Toast message | A live region with `aria-live="polite"` | None; it must not take focus |
| Infinite list | `feed` with `aria-busy` while loading | A "load more" button as an alternative to scrolling |

A live region is an element whose changes a screen reader announces without moving focus.

## Focus management

Single-page applications change the screen without a page load, so the browser does not reset focus. The design must do it.

- On a route change, move focus to the new page heading and announce the new title.
- When a dialog opens, move focus into it. When it closes, return focus to the element that opened it.
- When an item is deleted from a list, move focus to the next item, not to the top of the page.

## Motion and timing

Respect the `prefers-reduced-motion` setting by removing large animations. Avoid time limits, or let the user extend them. Auto-playing carousels need a pause control.

## A worked example

An autocomplete returns 8 suggestions. A sighted mouse user sees them appear. A screen reader user hears nothing unless the design includes a live region that announces "8 suggestions available". As the user presses the down arrow, `aria-activedescendant` points to the active option, and the screen reader reads it while focus stays in the input. This costs about 10 lines of markup and makes the component usable for everyone.

## What the interviewer probes

- How would you test this? Automated checks find about a third of issues; keyboard and screen reader passes find the rest.
- How does virtualization affect a screen reader? Rows outside the window do not exist, so each row needs `aria-setsize` and `aria-posinset`.
- Where does this live in a large codebase? In the shared component library, so each team inherits it.

## Go deeper

- [Grokking the Modern Frontend System Design Interview](https://www.designgurus.io/course/grokking-the-modern-frontend-system-design-interview?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-accessibility) teaches accessibility as a design input and builds an accessible modal, dropdown, and autocomplete.
- Lesson: [Accessibility by Design](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/accessibility-by-design?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-accessibility), in Grokking the Modern Frontend System Design Interview
- Lesson: [Design an Accessible Modal and Dropdown](https://www.designgurus.io/course-play/grokking-the-modern-frontend-system-design-interview/doc/design-an-accessible-modal-and-dropdown?utm_source=github&utm_medium=repo&utm_campaign=frontend-system-design-interview&utm_content=concepts-accessibility), in Grokking the Modern Frontend System Design Interview
- [Component API design](component-api-design.md) shows how accessible behavior becomes part of a component contract.
