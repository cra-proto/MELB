# gcweb-interactive-questions

Generates and reviews GCWeb interactive question patterns (decision trees/wizards) for Canada.ca and CRA web pages.

## What it does

Produces accessible, standards-compliant interactive question flows using the wb-fieldflow component, with radio button inputs, nested sub-questions, alert-based answers, and full text version alternatives.

## When to use

- Creating decision trees or wizards that guide users to specific answers
- Implementing wb-fieldflow interactive question components
- Reviewing question structure, answer alerts, or accessibility of existing interactive questions
- Adding supportive statements or lead-in text to questions

## Key rules enforced

- Content must already exist in a text format (interactive questions are supportive, not authoritative)
- Radio buttons as default input; dropdowns for many options
- Answers displayed in alerts (info/success/warning) with "The result is" headers
- Answers wrapped in `aria-live="polite"` for screen reader announcement
- Heading states the task, never uses "wizard" or "decision tree"
- Fewer questions with fewer options preferred over front-loaded complex questions

## Source

Rules extracted from the `en/design-systems/gcweb/interactive-questions/` section of the [CRA User-Centred Design Guide](https://github.com/cra-proto/design).

## Files

- `SKILL.md` — Full skill definition with HTML templates, question patterns, and validation checklist
