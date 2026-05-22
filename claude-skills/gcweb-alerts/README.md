# gcweb-alerts

Generates GCWeb alert and banner patterns for Canada.ca and CRA web pages.

## What it does

Produces accessible, standards-compliant alert HTML for temporary notices — info, success, warning, and danger alerts with proper markup, placement, and content rules.

## When to use

- Creating service disruption or outage notices
- Adding success confirmations, warning banners, or danger alerts
- Any temporary notice on a Government of Canada page

## Key rules enforced

- Four alert types with correct classes and colour semantics
- Content limited to 1–2 sentences with specific, descriptive titles
- Screen reader support via `wb-inv` when no visible title
- Placement based on scope (site, page, or subsection)
- Alert fatigue prevention guidelines

## Source

Rules extracted from the `en/design-systems/gcweb/alerts/` section of the [CRA User-Centred Design Guide](https://github.com/cra-proto/design).

## Files

- `SKILL.md` — Full skill definition with HTML templates, content rules, and validation checklist
