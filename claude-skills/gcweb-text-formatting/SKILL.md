---
name: gcweb-text-formatting
description: >
  Generate and review GCWeb text formatting patterns for Canada.ca and CRA
  web pages. Use this skill whenever the user asks about bold, italic,
  underline, text alignment, font sizing, text colour, wrapping, or
  truncating text for a Government of Canada or CRA page.
---

# GCWeb Text Formatting

You are creating or reviewing text formatting for CRA/Canada.ca web pages. Use text formatting to support the most important information on a page.

## Bold, italic, underline

### Bold (`<strong>`)

**Do:**
- Use for emphasis to highlight certain words, phrases, or numbers
- Use sparingly — the more you use it, the less effective it is

**Do not:**
- Bold an entire line or body of text (harder to read)
- Combine bold with underline unless it's part of hyperlinked text on a topic page

```html
<p>Regular text plus <strong>bold text</strong></p>
```

### Italic (`<em>`)

**Do:**
- Use sparingly for short sections of text
- Use for branded trademarks (e.g. *Interac®*)
- Use for French and foreign words, Latin terms, legal references, mathematical material, titles of works

**Do not:**
- Use for design or decorative purposes
- Use to emphasize a word or phrase (use bold instead)
- Use for long passages of text or quotations
- Use in page titles
- Italicize multiple hyperlinked publication titles in a list

```html
<p>Regular text plus <em>italicized text</em></p>
```

### Underline (`<u>`)

- Use **only** for link text
- Do not underline non-link text as it mimics link appearance and causes usability problems

```html
<p>Regular text plus <u>underlined text</u></p>
```

## Alignment

Default is left-aligned. Left alignment performs best for readers.

```html
<!-- Left aligned (default) -->
<p class="text-left">Left aligned text</p>

<!-- Center aligned -->
<p class="text-center">Center aligned text</p>

<!-- Right aligned -->
<p class="text-right">Right aligned text</p>
```

**Do:**
- Center or right-align table data cells to mimic accounting tables

**Do not:**
- Use to align non-text information
- Avoid using unless it adds value to the content

## Sizing

Default font size is **16px** with line-height **1.428**.

### Size classes

```html
<!-- Heading sizes (visual only, no semantic heading) -->
<p class="h1">H1 size text</p>
<p class="h2">H2 size text</p>
<p class="h3">H3 size text</p>
<p class="h4">H4 size text</p>
<p class="h5">H5 size text</p>

<!-- Lead text (larger intro text) -->
<p class="lead">Lead size text</p>

<!-- Small text (CSS class) -->
<p class="small">Small size text</p>

<!-- Small text (HTML element for disclaimers, fine print, copyright) -->
<small>Small text element</small>
```

**Do:**
- Use appropriate semantic markup (size changes can convey information)
- Use when you need to add or reduce impact to text that isn't an actual heading

**Do not:**
- Use in place of actual headings
- Use small text to squeeze a lot of text into a small area

### Small text coding note

- Use `.small` CSS class to **style and reduce text in size and impact**
- Use `<small>` element to **define small text** (side-comments, disclaimers, copyright, legal text)

## Colour

### Text colour classes

```html
<p class="text-muted">Muted (gray) text</p>
<p class="text-info">Info (light blue) text</p>
<p class="text-primary">Primary (dark blue) text</p>
<p class="text-success">Success (green) text</p>
<p class="text-warning">Warning (yellow) text</p>
<p class="text-danger">Danger (red) text</p>
```

### Background colour classes

```html
<p class="bg-info">Info background</p>
<p class="bg-primary">Primary background</p>
<p class="bg-success">Success background</p>
<p class="bg-warning">Warning background</p>
<p class="bg-danger">Danger background</p>
```

### Choosing the right colour

| Colour | Class | Usage |
|--------|-------|-------|
| Muted (gray) | `text-muted` | De-emphasize text |
| Info (light blue) | `text-info` | Fairly important information |
| Primary (dark blue) | `text-primary` | Match site's main colour palette |
| Success (green) | `text-success` | Correct or right way of doing something |
| Warning (yellow) | `text-warning` | Call attention and warn the user |
| Danger (red) | `text-danger` | Very important content or dangerous action |

**Do:**
- Use colour for decorative purposes or to convey information
- Wrap text in a `<span>` tag if a style doesn't appear correctly due to specificity

**Do not:**
- Use colour as the only way to communicate information (not accessible)
- Use colours unless they add value to the content

## Wrapping and truncating

### No-wrap (`nowrap`)

Prevents specific text from wrapping to the next line.

```html
<p>Filing deadline is <span class="nowrap">April 30, 2025</span></p>
```

**Do:**
- Use for telephone numbers, postal codes, dates, mathematical equations, French punctuation with required spaces

**Do not:**
- Use to wrap complete sentences
- Use for non-text information

### Truncate (`wb-elps`)

Crops text to fit within a grid column on a single line, adding an ellipsis. Text remains readable by screen readers.

```html
<p class="wb-elps">This very long text will be truncated with an ellipsis</p>
```

**Do:**
- Use primarily for hyperlinks to prevent word wrap when height is a concern

**Do not:**
- Use for sentences (can hide information visually)
- Use in place of equal height when trying to achieve equal height on a grid row

## CSS classes reference

| Class | Purpose |
|-------|---------|
| `text-left` | Left align text |
| `text-center` | Center align text |
| `text-right` | Right align text |
| `h1`–`h5` | Apply heading size to non-heading elements |
| `lead` | Larger intro text |
| `small` | Smaller text |
| `text-muted` | Gray/muted colour |
| `text-info` | Light blue colour |
| `text-primary` | Dark blue colour |
| `text-success` | Green colour |
| `text-warning` | Yellow colour |
| `text-danger` | Red colour |
| `bg-info/primary/success/warning/danger` | Background colours |
| `nowrap` | Prevent line wrapping |
| `wb-elps` | Truncate with ellipsis |

## Validation checklist

1. Bold is used sparingly for emphasis only
2. Italic is not used for emphasis (bold is used instead)
3. Underline is only used for link text
4. Text alignment is left by default, alternatives used only when adding value
5. Size classes are not used in place of semantic headings
6. Colour is not the sole means of conveying information
7. No-wrap is applied to dates, phone numbers, postal codes
8. Truncation is only used for hyperlinks, not sentences
