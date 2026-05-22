# gcweb-text-formatting

Generates and reviews GCWeb text formatting patterns for Canada.ca and CRA web pages.

## What it does

Produces accessible, standards-compliant text formatting including bold, italic, underline, alignment, sizing, colour, wrapping, and truncating — with correct usage rules for each.

## When to use

- Applying bold, italic, or underline formatting to CRA web content
- Setting text alignment (left, center, right)
- Adjusting text sizing with heading-size classes or lead/small
- Using contextual colour classes for text or backgrounds
- Preventing line wrapping on dates, phone numbers, or postal codes
- Truncating text with ellipsis in grid layouts

## Key rules enforced

- Bold used sparingly for emphasis only; never bold entire lines
- Italic for trademarks and titles of works, never for emphasis
- Underline only for link text (never for non-link text)
- Left alignment by default; alternatives only when adding value
- Colour never used as the sole means of conveying information
- Size classes not used in place of semantic headings
- No-wrap applied to dates, phone numbers, postal codes

## Source

Rules extracted from the `en/design-systems/gcweb/text-formatting/` section of the [CRA User-Centred Design Guide](https://github.com/cra-proto/design).

## Files

- `SKILL.md` — Full skill definition with HTML templates, CSS classes, and validation checklist
