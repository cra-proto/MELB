# gcweb-toc

Generates GCWeb table of contents patterns for Canada.ca and CRA web pages.

## What it does

Produces accessible, standards-compliant table of contents HTML in two variants: same-page anchor navigation ("On this page") and index-page directories ("Table of contents").

## When to use

- Adding an "On this page" section to a long page with multiple headings
- Creating an index page that links to subpages
- Any page requiring anchor navigation or a directory of content

## Key rules enforced

- Exact heading labels: "On this page" vs "Table of contents"
- Correct heading semantics (`<h2 class="h3">`)
- Placement after h1 and description, before substantive content
- Maximum 3 levels of nesting; column layout only for index-page variant

## Source

Rules extracted from the `en/design-systems/gcweb/toc/` section of the [CRA User-Centred Design Guide](https://github.com/cra-proto/design).

## Files

- `SKILL.md` — Full skill definition with HTML templates, variant rules, and validation checklist
