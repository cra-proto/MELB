---
name: gcweb-doormats
description: >
  Generate GCWeb doormat navigation patterns for Canada.ca and CRA web pages.
  Use this skill whenever the user asks to create doormats, navigation tiles,
  link-and-description blocks, landing page link sections, topic page links,
  subway index page links, or any set of linked tiles with short descriptions
  for a Government of Canada or CRA page. Also trigger when the user is
  building a GCWeb index page, topic page, institutional landing page, or
  subway index and needs the link navigation section. If the user mentions
  "doormats", "doormat pattern", "navigation links with descriptions",
  "link tiles", or is working on a Canada.ca page that needs a set of
  linked sections with summaries, use this skill.
---

# GCWeb Doormats

You are generating doormat patterns — sets of links with short descriptions displayed in concise blocks. Doormats are the primary navigation pattern on Government of Canada pages, helping users choose the right path to complete their tasks.

## The HTML structure

Every doormat set uses this exact structure. Do not deviate from these class names:

```html
<div class="wb-eqht row">
  <div class="col-lg-4 col-md-6">
    <section>
      <h3><a href="[url]">[Link title]</a></h3>
      <p>[Link description]</p>
    </section>
  </div>
  <!-- repeat col-lg-4 col-md-6 blocks for each doormat -->
</div>
```

Key structural rules:
- All doormat `<div>` blocks go inside a single `<div class="wb-eqht row">` wrapper
- Each doormat is a `<div class="col-lg-4 col-md-6">` containing a `<section>` with an `<h3>` link and a `<p>` description
- This grid creates 3 columns on large screens, 2 on medium, 1 on small
- The heading level (`h3`) assumes the doormats sit under an `h2`. Adjust if the page context requires a different level

## Link title rules

The link title is the clickable heading. It must be concise because users scan these quickly.

**Character limits:**
- Optimal: 45 characters (with spaces) — this works for both English and French
- Maximum: 75 characters (with spaces)
- Tip: aim for English titles under 35 characters so the French translation stays under 45

**Content rules:**
- Must be descriptive and unique within the page — each title should be clearly distinguishable from the others
- Does not need to exactly match the destination page's title
- Sentence case only (capitalize first word and proper nouns)
- Never end with punctuation (no periods, question marks, colons)

**Status labels** (optional): Add after the link text when a program or service has recently changed:
```html
<h3><a href="#">Program name <span class="label label-info">New</span></a></h3>
<h3><a href="#">Program name <span class="label label-danger">Closed</span></a></h3>
```

## Link description rules

The description helps users decide whether this path is relevant to them. This is where most doormat quality issues happen — generic descriptions defeat the entire purpose of the pattern.

**Character limits:**
- Maximum: 120 characters (with spaces) for both languages
- Tip: aim for English under 100 characters so French stays under 120

**Content rules:**
- Describe what the user can find or do on the linked page
- Recommended style: comma-separated keywords or phrases (not full sentences)
- Never start with introductory phrases: ~~"Includes…"~~, ~~"Information on…"~~, ~~"Learn more about…"~~
- Never repeat information already in the link title
- Never include promotional messaging
- Never include links inside descriptions
- Never end with a period or any punctuation
- Never add formatting (no bold, no bullets, no markup inside the `<p>`)
- Do not include "and" before the last item in a comma list — it saves characters and avoids Oxford comma ambiguity

**The distinguishability rule — this is the most important rule:**
Each description must help users tell one doormat apart from another. If you find yourself writing the same generic text across multiple doormats (like "Who can apply, how to apply, how much you can get"), stop. Instead, write something specific to each one.

Good — each description is unique and specific:
- Canada child benefit (CCB): "Monthly payment for eligible families with children under 18 years of age"
- GST/HST credit: "Quarterly payment for people with low and modest incomes, payment amounts and dates"
- Canada Dental Benefit: "Interim benefit for eligible families with children under 12"

Bad — generic, identical descriptions that don't help users choose:
- Canada child benefit (CCB): "Who can apply, how to apply, how much you can get, payment dates"
- GST/HST credit: "Who can apply, how to apply, how much you can get, payment dates"
- Canada Dental Benefit: "Who can apply, how to apply, how much you can get, return a payment"

## Parallel structure

All doormats on a page must match each other grammatically. Pick ONE style and use it consistently:

| Style | Example titles |
|-------|---------------|
| Topics (nouns) | Arts and media, History and heritage, Cultural trade |
| Products/services (nouns) | Registered retirement savings plan, Tax-free savings accounts |
| Actions (verbs) | Notify the CRA of the date of death, Apply for the death benefit |
| Audience groups (nouns) | Individuals, Businesses, Charitable organizations |

Never mix styles on the same page. All verbs, or all nouns — not a combination.

Bad (mixed): "Apply for the Canada child benefit", "Quarterly GST/HST credit payment", "Getting the climate action incentive"
Good (consistent nouns): "Canada child benefit (CCB)", "GST/HST credit", "Climate action incentive payment"

## Description style options

Pick ONE style for descriptions and use it consistently across the set:

1. **Comma-separated subtopics** (recommended): List the links or headings found on the destination page
   - "Tax-free savings accounts, registered savings plans, pooled pension plans, plan administrators"

2. **Task descriptions**: Describe what the user can do
   - "File income tax, get the income tax and benefit package, check status of your tax refund"

3. **Eligibility/benefit summaries**: Describe what you get and who qualifies
   - "Monthly payment for eligible families with children under 18 years of age"

## Ordering

- Order doormats by demand (highest-traffic links first) unless the user specifies otherwise
- Priority flow: left to right, then top to bottom (in a 3-column layout, position 1 is top-left, position 3 is top-right, position 4 is second-row-left)
- For sequential/step-by-step processes, number the doormats and use the stylized steps pattern instead (`<ol class="lst-stps ld-zr">`)

## Grouping with subheadings

When a page has many doormats, group them under subheadings:
```html
<h2>Benefits for individuals</h2>
<div class="wb-eqht row">
  <!-- doormats -->
</div>

<h2>Benefits for businesses</h2>
<div class="wb-eqht row">
  <!-- doormats -->
</div>
```
- Keep each group to 9 or fewer doormats
- Only use subheadings when doormats are genuinely similar enough to need grouping

## Where doormats are used

| Template | Doormats required? |
|----------|-------------------|
| Institutional landing page | Mandatory |
| Topic page | Mandatory |
| Subway index page | Mandatory |
| Subway sub-step links | Optional |
| Campaign page | Optional |
| Stylized steps | Optional |

## Validation checklist

Before outputting doormats, verify:

1. Every link title is under 75 characters (ideally under 45)
2. Every description is under 120 characters (ideally under 100)
3. No description ends with a period or punctuation
4. No description starts with "Includes", "Information on", "Learn more about"
5. All titles use the same grammatical style (all nouns or all verbs, not mixed)
6. Each description is unique — no two doormats have the same or nearly-same description text
7. No description repeats information from its own title
8. The HTML uses `wb-eqht row` wrapper, `col-lg-4 col-md-6` columns, `<section>` with `<h3><a>` and `<p>`
9. Descriptions contain no links, no bold, no bullet points, no HTML markup
10. Link titles use sentence case and have no trailing punctuation

When generating doormats, include a brief validation summary showing the character count for each title and description so the user can verify compliance at a glance.
