---
name: gcweb-headings-titles
description: >
  Generate and review GCWeb headings and page titles for Canada.ca and CRA web
  pages. Use this skill whenever the user asks to create, review, or fix page
  titles (H1), headings, subheadings, or stacked titles for a Government of
  Canada or CRA page. Also trigger when the user mentions heading hierarchy,
  title tag metadata, sentence case for headings, or front-loading keywords.
---

# GCWeb Headings and Titles

You are creating or reviewing headings and page titles for CRA/Canada.ca web pages. Headings are large, bold, concise text that are hierarchical in nature.

## Writing rules for all headings

### Short and clear
- Be short, remove unnecessary words
- Do not use abbreviations unless the audience knows them better than the full form
- No promotional messaging or subjective claims
- When people search both the abbreviation and full form, include both (e.g. "Canada Emergency Response Benefit (CERB)")

### Front-load keywords
- Put the most relevant terms at the beginning
- Users scan down the left side; first few words must attract attention
- Research user search terms to identify words

### Capitalization
- Use **sentence case** for page titles, headings, subheadings, table captions, and table headers
- Capitalize only the first word and proper nouns
- Exception: capitalize each word in titles of guides, forms, reports, and publications

### No end punctuation
- No punctuation at end of titles, headings, or subheadings
- Question marks only in forms, wizards, surveys, or quizzes
- Avoid phrasing headings as questions
- For titles that sound like questions, omit the question mark ("Who is eligible")

### Follow headings with text
- Usually follow headings with text, not another heading
- Use text between heading and subheading to introduce what follows
- Exceptions: "On this page" TOC or when the page purpose is already obvious

## Page title (H1) rules

- Appears at top of page in largest, boldest text
- Must accurately describe the information in plain language
- Must make sense on its own without surrounding context
- Apply H1 **once per page only** (exception: subway pattern)
- H1 and `<title>` tag must match (with " - Canada.ca" appended to `<title>`)
- Keep under 60–65 characters so full title shows in search results
- Must be unique — verify with a search engine

### HTML

```html
<h1 id="wb-cont">Page title</h1>
```

### Stacked H1 — topic pattern

Used when a page belongs to a larger topic and could be confused with similar pages.

```html
<title>H1 heading - Title of parent page</title>
...
<p class="lead mrgn-tp-md mrgn-bttm-0 text-muted" aria-hidden="true">Title of parent page</p>
<h1 class="mrgn-tp-0" id="wb-cont">H1 heading<span class="wb-inv"> – Title of parent page</span></h1>
```

- Parent title in `<p>` with `aria-hidden="true"` (visual only)
- Screen reader context in `<span class="wb-inv">` inside the H1

### Stacked H1 — subway pattern

- Uses two H1 elements — refer to the subway pattern skill for implementation

## Organizing content (H2 and below)

### Heading hierarchy

| Level | Purpose | Usage |
|-------|---------|-------|
| H1 | Page title | Once per page only |
| H2 | Main sections | Many times |
| H3 | Subsections | Many times |
| H4 | Sub-subsections | Many times |
| H5 | **Avoid** | Restructure content instead |

- Never skip heading levels (e.g. H2 → H4)
- Divide text into logical sections approximately every 200 words

### Parallel structure
- Headings at the same level must match grammatically
- Combine parallel structure with front-loading keywords

### Table of contents
- Add when scrolling 2–3+ times is needed and page has multiple headings
- Generally required when there are more than 2 H2 headings

## CSS classes

| Class | Purpose |
|-------|---------|
| `mrgn-tp-0` | Remove top margin (stacked H1) |
| `mrgn-tp-md` | Medium top margin |
| `mrgn-bttm-0` | Remove bottom margin |
| `lead` | Larger intro text (stacked title parent label) |
| `text-muted` | Grey/muted colour (stacked title parent label) |
| `wb-inv` | Visually hidden, accessible to screen readers |
| `h3` | On `<h2>`, renders as H3 size keeping H2 semantics |

## Validation checklist

1. H1 appears exactly once (exception: subway pattern)
2. H1 has `id="wb-cont"` for skip-to-content link
3. H1 matches `<title>` tag content
4. All headings use sentence case
5. No punctuation at end of headings
6. Keywords are front-loaded
7. Heading hierarchy is sequential (no skipped levels)
8. H5 is avoided — content restructured instead
9. Parallel grammatical structure at each heading level
10. Stacked titles include `wb-inv` screen reader text
