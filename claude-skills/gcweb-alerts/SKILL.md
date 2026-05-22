---
name: gcweb-alerts
description: >
  Generate GCWeb alert/banner patterns for Canada.ca and CRA web pages.
  Use this skill whenever the user asks to create alerts, banners, notices,
  warning messages, success confirmations, or danger notices for a Government
  of Canada or CRA page. Also trigger when the user mentions "alert-info",
  "alert-warning", "alert-danger", "alert-success", service disruption
  notices, or outage banners.
---

# GCWeb Alerts

You are generating alert patterns — short, temporary notices that draw attention to important messages or changes. They are often time-sensitive. Also referred to as "banners."

## Four alert types

| Type | Classes | Colour | When to use |
|------|---------|--------|-------------|
| Info | `alert alert-info` | Blue | Clarification, helpful advice, processing times, confirming no disruption |
| Success | `alert alert-success` | Green | Successful action, task completion, disruption resolved |
| Warning | `alert alert-warning` | Yellow | Possible consequence of action/inaction, delays, closures, penalties |
| Danger | `alert alert-danger` | Red | Risk to health/safety, do-not-travel, systems down, service cancelled |

## HTML structure

### With a visible title

```html
<section class="alert alert-info">
  <p class="h3 mrgn-tp-0">Descriptive title</p>
  <p>Content of your alert <a href="#">link text</a>.</p>
</section>
```

### Without a visible title

```html
<section class="alert alert-info">
  <span class="wb-inv">Alert: Info</span>
  <p>Content of your alert <a href="#">link text</a>.</p>
</section>
```

The invisible text in `<span class="wb-inv">` must match the alert type: "Alert: Info", "Alert: Success", "Alert: Warning", or "Alert: Danger".

## Key CSS classes

- `alert` — base class, always required
- `alert-info` / `alert-success` / `alert-warning` / `alert-danger` — type modifier
- `h3` — applied to `<p>` for title styling (not an actual `<h3>`)
- `mrgn-tp-0` — removes top margin on title paragraph
- `wb-inv` — visually hidden text for screen readers

## Coding rules

- Must begin `<section>` with a heading specific to the featured content
- Must not nest a `<section>` within another `<section>`
- If no visible title, must add `<span class="wb-inv">` text for screen readers

## Content rules

Alerts should:
1. Be short and simple — aim for 1 or 2 sentences
2. Include a descriptive title that is concise and specific (never "Note", "Info", "Important")
3. Describe the impact on the user
4. Be tailored to the page on which they appear
5. Relate to the content immediately around them
6. Use links sparingly — only one link if needed

## When to use

- Display temporary information or highlight temporary importance
- Highlight an important change: service/site outage, process change
- Provide the result of a user action (confirm success, notify of error)
- Warn of a consequence of action or inaction related to the task
- Summarize a large change and link to full details

## When NOT to use

- Normal steps in a process or task
- Adding emphasis to regular content
- Low-risk or infrequent warnings
- Long explanations of changes or disruptions
- Marking content as new or updated — use Labels instead

## Placement rules

| Scope | Placement |
|-------|-----------|
| Entire site | Top of page, above main heading |
| Entire page | Under page heading, before intro text, after rescue link |
| Page subsection | Within the subsection, under its heading or between paragraphs |

Use alerts on pages where services are impacted. Avoid on general theme or topic pages.

## Alert fatigue prevention

- Limit the number of alerts per page and across pages
- Only use on a temporary basis
- Only for significant situations impacting most users
- Canada.ca home page: only when 50%+ of population affected
- Institutional landing pages: only when 40%+ of users impacted
- Never use expand/collapse on alerts — content must be immediately visible

## Validation checklist

1. Alert type matches the severity (info/success/warning/danger)
2. Content is 1–2 sentences maximum
3. Title is specific and descriptive (not generic)
4. No nested `<section>` elements
5. Screen reader text present if no visible title
6. Maximum one link per alert
7. Placement matches scope (site/page/subsection)
