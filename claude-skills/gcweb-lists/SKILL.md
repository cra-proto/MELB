---
name: gcweb-lists
description: >
  Generate and review GCWeb list patterns for Canada.ca and CRA web pages.
  Use this skill whenever the user asks to create or review bulleted lists,
  numbered lists, alpha or roman lists, description lists, icon lists,
  checklists, stylized steps, multi-column lists, or and/or patterns
  for a Government of Canada or CRA page.
---

# GCWeb Lists

You are creating or reviewing lists for CRA/Canada.ca web pages. There are 3 types of lists.

## List types

- **Bulleted (unordered)** lists — when list item order does not matter
- **Numbered, alpha, or roman (ordered)** lists — when list item order matters
- **Description** lists — for terms with their associated descriptions or definitions

## Lead-in to a list

Every list requires one of the following:

- **Lead-in sentence**: introduces or applies to all list items; emphasize the common element. If necessary, specify in bold: "**all**", "**any**", "**or**", "**one or more**", or "**one**"
- **Introductory paragraph**: a short paragraph above the list for context
- **Descriptive heading**: a clear heading above the list

## Bulleted (unordered) list

### Structure rules

- Place only one idea in each bulleted item
- Use one sentence per single list item
- Use sub-bullets sparingly for additional information
- Arrange list items by priority, importance, or popularity (alphabetically only on rare occasions)
- Use similar text and list types consistently within the same document
- Front-load the most important words at the beginning of each item
- Use consistent grammatical structure (same tense, same parts of speech)
- If a list has more than 7 items, consider breaking it into categories

### Tone and punctuation

- Use positive statements as much as possible
- Place negatively phrased items together
- Capitalize the first word of every list item
- Do not end list items with any punctuation

### HTML — basic bulleted list

```html
<ul>
  <li>List item 1</li>
  <li>List item 2</li>
  <li>List item 3</li>
</ul>
```

### Style variants

```html
<!-- Remove bullets -->
<ul class="lst-none">...</ul>

<!-- Unstyled (remove bullets and indentation) -->
<ul class="list-unstyled">...</ul>

<!-- Inline (horizontal) -->
<ul class="list-inline">...</ul>

<!-- Added spacing between items -->
<ul class="lst-spcd">...</ul>
```

## Icon lists

Replace bullets with Font Awesome 5.15.4 icons. Use `fa-ul` on the `<ul>` and `fa-li` on each icon `<span>`.

```html
<ul class="fa-ul">
  <li><span class="fas fa-check text-success fa-li"></span>Correct item</li>
  <li><span class="fas fa-times text-danger fa-li"></span>Incorrect item</li>
</ul>
```

### Checkmark and X icon rules

- Use green checkmarks for correct actions, red X marks for incorrect actions
- Use together for comparisons: yes/no, can/cannot, correct/incorrect
- Use only on one level of bullet (not mixed levels)
- Do not use in a table of contents
- Do not use outside of a list
- Do not use as a header with bullets beneath

## Multi-column lists

```html
<!-- Two columns -->
<ul class="colcount-sm-2">...</ul>

<!-- Three columns -->
<ul class="colcount-sm-3">...</ul>

<!-- Four columns -->
<ul class="colcount-sm-4">...</ul>
```

Do not combine multi-column and multi-level (nested) lists.

## And/or pattern

Use to visually show "and" or "or" conditions between list items.

```html
<!-- "Or" condition -->
<ul class="list-unstyled cnjnctn-type-or cnjnctn-sm">
  <li class="cnjnctn-col">Option A</li>
  <li class="cnjnctn-col">Option B</li>
</ul>

<!-- "And" condition -->
<ul class="list-unstyled cnjnctn-type-and cnjnctn-md">
  <li class="cnjnctn-col">Requirement A</li>
  <li class="cnjnctn-col">Requirement B</li>
</ul>
```

## Numbered (ordered) list

Use when the order of items matters.

```html
<ol>
  <li>First step</li>
  <li>Second step</li>
  <li>Third step</li>
</ol>
```

### Ordered list variants

```html
<!-- Alphabetical (lowercase) -->
<ol class="lst-lwr-alph">...</ol>

<!-- Alphabetical (uppercase) -->
<ol class="lst-upr-alph">...</ol>

<!-- Roman numerals (lowercase) -->
<ol class="lst-lwr-rmn">...</ol>

<!-- Roman numerals (uppercase) -->
<ol class="lst-upr-rmn">...</ol>
```

### Stylized steps

Use for process steps with large step numbers.

```html
<ol class="lst-stps">
  <li><h3>Step title</h3>
    <p>Step content</p>
  </li>
  <li><h3>Step title</h3>
    <p>Step content</p>
  </li>
</ol>
```

Sub-steps variant:

```html
<ol class="lst-stps-sub">
  <li><h4>Sub-step title</h4>
    <p>Sub-step content</p>
  </li>
</ol>
```

## Description list

Use for terms paired with their descriptions or definitions.

```html
<dl>
  <dt>Term 1</dt>
  <dd>Description for term 1</dd>
  <dt>Term 2</dt>
  <dd>Description for term 2</dd>
</dl>
```

### Horizontal description list

```html
<dl class="dl-horizontal">
  <dt>Term 1</dt>
  <dd>Description for term 1</dd>
  <dt>Term 2</dt>
  <dd>Description for term 2</dd>
</dl>
```

## Interactive checklist

```html
<ul class="list-unstyled lst-spcd">
  <li class="checkbox">
    <label><input type="checkbox"> Checklist item 1</label>
  </li>
  <li class="checkbox">
    <label><input type="checkbox"> Checklist item 2</label>
  </li>
</ul>
```

## CSS classes reference

| Class | Purpose |
|-------|---------|
| `lst-none` | Remove bullets only |
| `list-unstyled` | Remove bullets and indentation |
| `list-inline` | Display items horizontally |
| `lst-spcd` | Add spacing between items |
| `fa-ul` | Font Awesome icon list container |
| `fa-li` | Font Awesome icon list item marker |
| `colcount-sm-2/3/4` | Multi-column layout |
| `cnjnctn-type-or` | "Or" condition pattern |
| `cnjnctn-type-and` | "And" condition pattern |
| `lst-lwr-alph` | Lowercase alphabetical |
| `lst-upr-alph` | Uppercase alphabetical |
| `lst-lwr-rmn` | Lowercase roman numerals |
| `lst-upr-rmn` | Uppercase roman numerals |
| `lst-stps` | Stylized step numbers |
| `lst-stps-sub` | Stylized sub-step numbers |
| `dl-horizontal` | Horizontal description list |

## Validation checklist

1. Every list has a lead-in sentence, introductory paragraph, or descriptive heading
2. Only one idea per bulleted item
3. First word of every list item is capitalized
4. No punctuation at end of list items
5. Most important words are front-loaded
6. Consistent grammatical structure across items at the same level
7. Lists with 7+ items are broken into categories
8. Icon lists use `fa-ul` on `<ul>` and `fa-li` on icons
9. Multi-column and multi-level lists are not combined
10. And/or patterns use correct `cnjnctn-type-or` or `cnjnctn-type-and` classes
