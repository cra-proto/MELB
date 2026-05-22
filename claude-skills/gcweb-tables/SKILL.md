---
name: gcweb-tables
description: >
  Generate and review GCWeb table patterns for Canada.ca and CRA web pages.
  Use this skill whenever the user asks to create or review data tables,
  responsive tables, table captions, headers with scope attributes, zebra
  striping, sortable tables, or table accessibility for a Government of
  Canada or CRA page.
---

# GCWeb Tables

You are creating or reviewing tables for CRA/Canada.ca web pages. Tables display data in rows and columns to help people scan, understand, compare, and act on information.

## When to use

- Organize related information (e.g. phone numbers, office locations)
- Make complex information easier to understand in a clear structure
- Enable users to look up specific information
- Highlight trends or patterns in data
- Display data too detailed or precise for text

## When to consider alternatives

- When text layout can use flexboxes, grids, or column-count lists instead
- When information can be presented as a simple list or summarized in text
- If the table would have only one column of data, use a list instead

## Captions

Captions describe the table's purpose and help people find, navigate, and understand tables.

### Caption rules

**Do:**
- Give a clear idea of what is in the table
- Be unique within the context of the page
- Be short with no unnecessary words
- Start with the most relevant terms
- Make sense on its own

**Do not:**
- Give an interpretation of the data
- End with punctuation
- Contain abbreviations (unless better known than long form)
- Include promotional messaging

### Caption with heading

If there is both a heading and caption, keep the caption and use `wb-inv` so only screen readers see it.

```html
<h3>Telephone numbers</h3>
<table class="table">
  <caption class="wb-inv">Telephone numbers for personal taxes</caption>
  ...
</table>
```

## Headers

### Column and row headers

- Give each column and row a unique header describing its data
- Keep headers as short as possible
- Add units of measure in headers, not in each data cell
- Use `scope="col"` for column headers and `scope="row"` for row headers

```html
<thead>
  <tr>
    <th scope="col">City</th>
    <th scope="col">Temperature (&deg;C)</th>
    <th scope="col">Rainfall (mm)</th>
  </tr>
</thead>
<tbody>
  <tr>
    <th scope="row">Ottawa</th>
    <td class="text-right">-6.1</td>
    <td class="text-right">65.4</td>
  </tr>
</tbody>
```

## Table structure

- Keep table data short, clear, and concise
- Include only relevant data to minimize table size
- Turn complex tables into one or more simple tables
- Avoid merged cells (accessibility issues)
- Avoid nested tables
- Place footnotes at the point of need, usually just after the table
- If multiple tables have footnotes, each footnote needs a unique label

## Data alignment

- Left-align text data (default)
- Right-align numerical data using `text-right` class
- Center-align when it improves scanning of short, uniform data

```html
<td class="text-right">1,234.56</td>
```

## Responsive tables

Wrap tables in `table-responsive` for horizontal scrolling on small screens.

```html
<div class="table-responsive">
  <table class="table">...</table>
</div>
```

## Blank cells

- Avoid blank (empty) cells
- If a cell has no data, use a dash or "n/a" and explain its meaning in the caption or a footnote

## Basic HTML structure

```html
<div class="table-responsive">
  <table class="table table-bordered">
    <caption>Table description</caption>
    <thead>
      <tr>
        <th scope="col">Header 1</th>
        <th scope="col">Header 2</th>
        <th scope="col">Header 3</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th scope="row">Row header</th>
        <td>Data</td>
        <td>Data</td>
      </tr>
      <tr>
        <th scope="row">Row header</th>
        <td>Data</td>
        <td>Data</td>
      </tr>
    </tbody>
  </table>
</div>
```

## Style variants

```html
<!-- Bordered -->
<table class="table table-bordered">

<!-- Zebra striping (alternating row colours) -->
<table class="table table-striped">

<!-- Hover effect on rows -->
<table class="table table-hover">

<!-- Condensed (less padding) -->
<table class="table table-condensed">

<!-- Combined styles -->
<table class="table table-bordered table-striped table-hover">
```

## Sortable tables (wb-tables)

Add sorting, filtering, and pagination with the DataTables plugin.

```html
<table class="table wb-tables" data-wb-tables='{"ordering": true, "paging": true}'>
  ...
</table>
```

## Footnotes in tables

```html
<table class="table table-bordered">
  <caption>Table with footnotes</caption>
  ...
  <tbody>
    <tr>
      <td>Data<sup id="fn1-rf"><a class="fn-lnk" href="#fn1"><span class="wb-inv">Footnote </span>1</a></sup></td>
    </tr>
  </tbody>
</table>
<aside class="wb-fnote" role="note">
  <h2 id="fn">Footnotes</h2>
  <dl>
    <dt>Footnote 1</dt>
    <dd id="fn1"><p>Footnote text. <a href="#fn1-rf"><span class="wb-inv">Return to footnote </span>1<span class="wb-inv"> referrer</span></a></p></dd>
  </dl>
</aside>
```

## CSS classes reference

| Class | Purpose |
|-------|---------|
| `table` | Base table styling |
| `table-bordered` | Add borders to all cells |
| `table-striped` | Zebra striping on rows |
| `table-hover` | Highlight rows on hover |
| `table-condensed` | Reduce cell padding |
| `table-responsive` | Horizontal scroll wrapper |
| `wb-tables` | DataTables sorting/filtering plugin |
| `text-right` | Right-align cell content |
| `text-center` | Center-align cell content |
| `wb-inv` | Visually hidden (screen reader only) |

## Validation checklist

1. Table has a `<caption>` (visible or `wb-inv`)
2. All column headers use `<th scope="col">`
3. Row headers (if any) use `<th scope="row">`
4. No merged cells unless absolutely necessary
5. Numerical data is right-aligned
6. Units of measure are in headers, not repeated in cells
7. Table is wrapped in `table-responsive` for mobile
8. No blank cells (use dash or "n/a" with explanation)
9. Complex tables are broken into simpler ones
10. Footnotes have unique labels and are placed after the table
