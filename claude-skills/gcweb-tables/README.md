# gcweb-tables

Generates and reviews GCWeb table patterns for Canada.ca and CRA web pages.

## What it does

Produces accessible, standards-compliant data tables with proper captions, header scope attributes, responsive wrappers, zebra striping, sortable columns, footnotes, and alignment rules.

## When to use

- Creating or reviewing data tables on a CRA or Canada.ca page
- Implementing responsive tables for mobile devices
- Adding sortable/filterable tables with wb-tables (DataTables)
- Checking table accessibility (captions, scope attributes, header structure)
- Simplifying complex tables into multiple simple ones

## Key rules enforced

- Every table has a `<caption>` (visible or `wb-inv` for screen readers)
- Column headers use `<th scope="col">`, row headers use `<th scope="row">`
- No merged cells unless absolutely necessary
- Numerical data is right-aligned; units of measure in headers only
- Tables wrapped in `table-responsive` for mobile
- No blank cells (use dash or "n/a" with explanation)
- Complex tables broken into simpler ones

## Source

Rules extracted from the `en/design-systems/gcweb/tables/` section of the [CRA User-Centred Design Guide](https://github.com/cra-proto/design).

## Files

- `SKILL.md` — Full skill definition with HTML templates, style variants, and validation checklist
