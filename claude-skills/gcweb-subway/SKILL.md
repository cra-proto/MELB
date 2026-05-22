---
name: gcweb-subway
description: >
  Generate and review GCWeb subway navigation patterns for Canada.ca and CRA
  web pages. Use this skill whenever the user asks to create or review subway
  navigation, step-by-step page flows, index pages with subway menus, step
  pages with double h1 headings, or sub-pages (connected or non-connected)
  for a Government of Canada or CRA page.
---

# GCWeb Subway Pattern

You are creating or reviewing subway navigation patterns for CRA/Canada.ca web pages. The subway pattern presents content as a series of related tasks broken down into steps.

## When to use

- Guide users through a series of related tasks
- Break up long and complex content over a group of related pages
- Provide persistent navigation through a user journey for a product or service

## What to avoid

- Do not use the subway pattern for any reason except guiding users through related tasks
- Do not misuse the pattern as it creates inconsistency across Canada.ca

## Page types

The subway pattern has 3 page types:

| Page type | Required | Level |
|-----------|----------|-------|
| Index page | Yes | First level |
| Step page | Yes | Second level |
| Sub-page | No | Third level |

## Happy path structure

Content should follow a logical user journey:

1. **Introduce** the product or service
2. **Explain who it's for** (eligibility)
3. **Explain the core activity** (apply, claim, file, register)
4. **Explain follow-up activities** (after you apply)
5. **Contact details** (optional)

### Recommended step page wording

**Eligibility:**
- Who can apply / Who can claim / Who can file / Who is eligible / Who must register

**Core activity:**
- Get ready to apply / How to apply / When to apply / Calculate your [x]

**Follow-up:**
- After you apply / Our review and decision / When and how to get your payment

**Contact:**
- Contact us

## Index page

The main landing page for the product or service.

### Rules

- **H1**: Product or service name — must be 100% unique within the entire Government of Canada
- **Brief description** below the H1
- **Subway menu** with links to each step page plus a short doormat description
- **Supporting links** (optional) below the subway menu under "Related information"
- **No pagination buttons** — users choose from the subway menu
- **Title tag**: `[Product or service name] - Canada.ca`

### HTML

```html
<h1 property="name" id="wb-cont" class="gc-thickline">[Product or service name]</h1>
<p>[Brief description of the product or service]</p>
<nav class="gc-subway provisional">
  <h2>Sections</h2>
  <dl>
    <dt><a href="step-1.html">[Step 1 title]</a></dt>
    <dd>[Short description]</dd>
    <dt><a href="step-2.html">[Step 2 title]</a></dt>
    <dd>[Short description]</dd>
    <dt><a href="step-3.html">[Step 3 title]</a></dt>
    <dd>[Short description]</dd>
  </dl>
</nav>
```

### Index page navigation rules

- Menu links should be concise (the double H1 carries the product name)
- Aim for 3 to 8 links
- Only link to second-level pages (step pages)
- All links must go to pages with the corresponding subway menu
- Include doormat descriptions following doormat link description rules

## Step page

Contains the content for each step in the user journey.

### Rules

- **Double H1**: first H1 is the product name (smaller, grey), second H1 is the step title
- **Breadcrumb** must include the index page
- **Subway menu** appears to the right on desktop, top on mobile
- **Pagination**: backwards button to previous step, forwards button to next step
- **Title tag**: `[Step page title] - [Product or service name] - Canada.ca`

### HTML

```html
<nav class="gc-subway provisional">
  <h2>Sections</h2>
  <dl>
    <dt><a href="step-1.html">[Step 1]</a></dt>
    <dd>[Description]</dd>
    <dt><a href="step-2.html" class="active">[Step 2 - current]</a></dt>
    <dd>[Description]</dd>
    <dt><a href="step-3.html">[Step 3]</a></dt>
    <dd>[Description]</dd>
  </dl>
</nav>
<h1 property="name" id="wb-cont" class="gc-thickline">[Step page title]</h1>
<!-- Page content -->
<nav class="gc-subway-pagination">
  <ul class="pager">
    <li class="previous"><a href="step-1.html" rel="prev">[Previous step title]</a></li>
    <li class="next"><a href="step-3.html" rel="next">[Next step title]</a></li>
  </ul>
</nav>
```

### Double H1 structure

Step pages and sub-pages use two H1 elements:

- **First H1**: Product/service name (displayed as smaller grey text above)
- **Second H1**: Step page title (displayed as the main page heading)

On mobile, the first H1 is part of the subway border container. On desktop, it appears as muted text above the second H1.

## Sub-pages

Use when a step's content is too complex for a single page.

### Non-connected sub-page

- Does **not** display the subway navigation menu
- Appears as a regular page linked from a step page
- Uses standard single H1
- Breadcrumb includes both the index page and the parent step page
- Title tag: `[Sub-page title] - [Step page title] - [Product or service name] - Canada.ca`

### Connected sub-page

- **Displays** the subway navigation menu
- Visually connected to its parent step in the subway line
- Uses the double H1 pattern
- Has pagination buttons connecting to sibling sub-pages and parent step
- Title tag: `[Sub-page title] - [Step page title] - [Product or service name] - Canada.ca`

### Choosing sub-page type

| Criteria | Non-connected | Connected |
|----------|--------------|-----------|
| Shows subway menu | No | Yes |
| H1 pattern | Single | Double |
| Pagination | Back to parent step only | Full sub-page pagination |
| Use when | Content is supplementary | Content is part of the step sequence |

## Pagination rules

| Page type | Back button | Forward button |
|-----------|-------------|----------------|
| Index page | None | None |
| First step page | Back to index | Next step |
| Middle step page | Previous step | Next step |
| Last step page | Previous step | None |
| Connected sub-page | Previous sub-page or parent step | Next sub-page or next step |

## Validation checklist

1. Index page H1 is unique within the entire Government of Canada
2. Index page includes brief description and subway menu with doormat descriptions
3. Subway menu has 3–8 links
4. Step pages use double H1 pattern
5. Step pages include subway navigation and pagination
6. Pagination correctly links to previous/next steps
7. Title tags follow the format: `[Page] - [Product] - Canada.ca`
8. Breadcrumbs include the index page
9. Content follows the happy path structure
10. Sub-pages use correct type (connected vs non-connected)
