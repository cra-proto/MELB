# gcweb-subway

Generates and reviews GCWeb subway navigation patterns for Canada.ca and CRA web pages.

## What it does

Produces accessible, standards-compliant subway navigation flows with index pages, step pages (with double H1 headings), connected and non-connected sub-pages, pagination controls, and doormat-style subway menus.

## When to use

- Creating a multi-page step-by-step flow for a product or service
- Implementing subway index pages with section menus
- Building step pages with double H1 pattern and pagination
- Adding connected or non-connected sub-pages to a subway flow
- Structuring content along the "happy path" (introduce, eligibility, core activity, follow-up, contact)

## Key rules enforced

- Index page H1 must be unique within the entire Government of Canada
- Subway menu has 3–8 links with doormat descriptions
- Step pages use double H1 pattern (product name + step title)
- Pagination correctly links previous/next steps
- Title tags follow format: `[Page] - [Product] - Canada.ca`
- Content follows the happy path structure
- Sub-pages use correct type based on content relationship

## Source

Rules extracted from the `en/design-systems/gcweb/subway/` section of the [CRA User-Centred Design Guide](https://github.com/cra-proto/design).

## Files

- `SKILL.md` — Full skill definition with HTML templates, page type rules, and validation checklist
