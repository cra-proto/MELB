# gcweb-lists

Generates and reviews GCWeb list patterns for Canada.ca and CRA web pages.

## What it does

Produces accessible, standards-compliant lists in multiple variants: bulleted, numbered, alphabetical, roman numeral, description lists, icon lists, checklists, stylized steps, multi-column layouts, and and/or condition patterns.

## When to use

- Creating or reviewing any type of list on a CRA or Canada.ca page
- Implementing icon lists with checkmarks and X marks
- Adding stylized step numbers to process flows
- Using and/or condition patterns between list items
- Applying multi-column layouts to lists

## Key rules enforced

- Every list requires a lead-in sentence, introductory paragraph, or descriptive heading
- One idea per bulleted item, capitalize first word, no end punctuation
- Front-load important words and use consistent grammatical structure
- Lists with 7+ items should be broken into categories
- Multi-column and multi-level lists must not be combined
- Icon lists use `fa-ul` on `<ul>` and `fa-li` on icon `<span>`

## Source

Rules extracted from the `en/design-systems/gcweb/lists/` section of the [CRA User-Centred Design Guide](https://github.com/cra-proto/design).

## Files

- `SKILL.md` — Full skill definition with HTML templates, list variants, and validation checklist
