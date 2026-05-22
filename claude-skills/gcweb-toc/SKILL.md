---
name: gcweb-toc
description: >
  Generate GCWeb table of contents patterns for Canada.ca and CRA web pages.
  Use this skill whenever the user asks to create a table of contents, "On this
  page" section, or index-page directory for a Government of Canada or CRA page.
  Also trigger when the user mentions "on this page", "page TOC", "table of
  contents", or needs same-page anchor navigation or an index page linking to
  subpages.
---

# GCWeb Table of Contents

You are generating table of contents patterns — lists of links that outline page content or act as a directory to subpages. There are two distinct variants with different rules.

## Variant 1: Same-page TOC

Links to headings further down the same page.

### When to use

- More than 2 headings/subheadings on the page
- Excessive scrolling (2–3+ scrolls) required to view all content
- Page is divided into subsections with their own headings

### When NOT to use

- Document exists across several web pages (use Variant 2 instead)
- Page consists solely of tables, images, etc. with no headers

### Rules

- Label the heading exactly **"On this page"**
- Place after the `<h1>` and short description, before substantive content
- Present as a vertical, left-aligned list — never use column layout
- Use standard link styles
- Include mainly `<h2>` headings; only include selective `<h3>` subheadings when there is clear user value
- Never go more than 3 levels deep

### HTML structure

Basic (h2 only):

```html
<h2 class="h3">On this page</h2>
<ul>
  <li><a href="#h-1">First H2 on the page</a></li>
  <li><a href="#h-2">Second H2 on the page</a></li>
  <li><a href="#h-3">Third H2 on the page</a></li>
</ul>
```

With optional h3 subheadings:

```html
<h2 class="h3">On this page</h2>
<ul>
  <li><a href="#h-1">First H2 on the page</a></li>
  <li><a href="#h-2">Second H2 on the page</a>
    <ul>
      <li><a href="#h-2-1">H3 subheading 1</a></li>
      <li><a href="#h-2-2">H3 subheading 2</a></li>
    </ul>
  </li>
  <li><a href="#h-3">Third H2 on the page</a></li>
</ul>
```

## Variant 2: Index-page TOC

Placed on its own page to act as a directory of subpages.

### Rules

- Label the heading exactly **"Table of contents"**
- May use column layout (`colcount-sm-2` or `colcount-sm-3`) if link titles look crowded
- Never combine multi-column and multi-level lists — break into separate groups in grids instead

### HTML structure

Default (single column):

```html
<h2 class="h3 mrgn-tp-0">Table of contents</h2>
<ul>
  <li><a href="#">First page</a>
    <ul>
      <li><a href="#">Subsection 1</a></li>
      <li><a href="#">Subsection 2</a></li>
    </ul>
  </li>
  <li><a href="#">Second page</a></li>
  <li><a href="#">Third page</a></li>
</ul>
```

Two-column layout:

```html
<h2 class="h3 mrgn-tp-0">Table of contents</h2>
<ul class="colcount-sm-2">
  <!-- list items -->
</ul>
```

Three-column layout:

```html
<h2 class="h3 mrgn-tp-0">Table of contents</h2>
<ul class="colcount-sm-3">
  <!-- list items -->
</ul>
```

## CSS classes

| Class | Purpose |
|-------|---------|
| `h3` | Applied to `<h2>` to visually size heading as h3 |
| `mrgn-tp-0` | Removes top margin (index-page variant) |
| `colcount-sm-2` | Renders list in 2 columns (index-page only) |
| `colcount-sm-3` | Renders list in 3 columns (index-page only) |

## Anchor ID convention

Use hyphenated sequential numbering: `h-1`, `h-2`, `h-2-1`, `h-3` — mirroring the heading hierarchy.

## Decision logic

1. Single page with 2+ headings and scrolling needed → Variant 1 ("On this page", `class="h3"`)
2. Index/directory page linking to subpages → Variant 2 ("Table of contents", `class="h3 mrgn-tp-0"`)
3. Crowded link titles → Add `colcount-sm-2` or `colcount-sm-3` (index-page only)
4. Need sub-items → Nest `<ul>` inside parent `<li>`, max 3 levels
5. Multi-column AND multi-level → Do not combine; break into separate grid groups

## Validation checklist

1. Heading text is exactly "On this page" or "Table of contents" (no other labels)
2. Heading is `<h2>` with `class="h3"` (or `class="h3 mrgn-tp-0"` for index-page)
3. TOC is positioned after `<h1>` and description, before substantive content
4. No more than 3 levels of nesting
5. Same-page variant uses vertical left-aligned layout only (no columns)
6. Column layout only on index-page variant
7. Standard link styling used throughout
