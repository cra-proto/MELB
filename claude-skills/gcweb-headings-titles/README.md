# gcweb-headings-titles

Generates and reviews GCWeb headings and page titles for Canada.ca and CRA web pages.

## What it does

Produces accessible, standards-compliant headings with proper hierarchy, sentence case, front-loaded keywords, and stacked title patterns for related page sets.

## When to use

- Creating or reviewing page titles (H1) and subheadings
- Implementing stacked titles (topic or subway pattern)
- Checking heading hierarchy and parallel structure
- Matching H1 with title tag metadata

## Key rules enforced

- H1 once per page with `id="wb-cont"`, matching `<title>` tag
- Sentence case, no end punctuation, front-loaded keywords
- Sequential heading hierarchy (no skipped levels), avoid H5
- Stacked title patterns with proper `aria-hidden` and `wb-inv` markup

## Source

Rules extracted from the `en/design-systems/gcweb/headings-titles/` section of the [CRA User-Centred Design Guide](https://github.com/cra-proto/design).

## Files

- `SKILL.md` — Full skill definition with HTML templates, writing rules, and validation checklist
