---
name: gcweb-links
description: >
  Generate and review GCWeb link patterns for Canada.ca and CRA web pages.
  Use this skill whenever the user asks to create, review, or fix hyperlinks,
  button links, rescue links, download links, or email links for a Government
  of Canada or CRA page. Also trigger when the user mentions link text
  accessibility, vanity URLs, or "you may be looking for" rescue links.
---

# GCWeb Links

You are creating or reviewing links for CRA/Canada.ca web pages. Links must be accessible, descriptive, and strategically placed.

## Strategic link use

- Include links that directly support the topic or task on the current page
- Link to the original authoritative source
- Never link to intranet sites unless targeting government employees
- Never bury crucial links mid-paragraph or at the bottom of the page

## Link text rules

Link text must represent the destination content and work within page context.

**Do:**
- Use the first words or full title of the target page
- Front-load keywords that describe the target
- Use unique text for links to different pages
- Use identical text when linking to the same destination multiple times

**Do not:**
- Use the same link text for 2 different pages
- Use "click here", "read more", or other vague text
- Use promotional messaging
- Use link text tied to time-sensitive headings

## Capitalization

- Standalone link: capitalize first letter, then sentence case
- Link in a sentence: lowercase unless it contains a proper name
- Published works: match original capitalization

## Link behaviour

- Open in the same tab by default
- Only open in a new tab if same-tab would interrupt the user's task
- Use anchor IDs on headings for in-page linking (never scroll-to-text browser functions)

## Link placement

- Place links at the end of sentences, not mid-sentence
- Introduce related links with a verb: "Refer to:", "Learn more:", "Review:", "For details:"
- Never use "see" to introduce links
- Multiple related links: use bullet points at the point of need
- Never create a page that is just a list of links with no context

### HTML for related links

```html
<!-- Single link -->
<p>For details: <a href="#">Disability deductions and credits</a></p>

<!-- With lead-in sentence -->
<p>For more information, refer to: <a href="#">Paying your income tax by instalments</a>.</p>

<!-- Multiple links -->
<p>To learn more, review:</p>
<ul>
  <li><a href="#">Section 4.1.1, Assistance and Contract Payments Policy</a></li>
  <li><a href="#">Provincial and territorial R&D tax credits</a></li>
</ul>
```

## Rescue links

Use when a page could be confused with another similar page. Always placed below the title.

```html
<p class="pull-left small"><strong>You may be looking for:</strong></p>
<ul class="pull-left mrgn-lft-md list-unstyled small mrgn-bttm-lg">
  <li><a href="#">Link to reference page</a></li>
  <li><a href="#">Link to second reference page</a></li>
</ul>
```

## Email links

Write email addresses in full as the visible link text — never embed in generic text.

```html
<!-- Correct -->
Submit your request by email to <a href="mailto:abcxyz@canada.ca">abcxyz@canada.ca</a>

<!-- Wrong -->
<a href="mailto:abcxyz@canada.ca">Email us</a>
```

## Link design variants

### Default link
```html
<a href="#">Your link</a>
```

### Button link
```html
<a href="#" class="btn btn-default">Your link</a>
```
Use sparingly — no more than one button link per page.

### Image link
```html
<a href=""><img src="image.png" alt="Description of destination" /></a>
```

## Internal government and external links

- Internal government content: add "(accessible only on the Government of Canada network)" after the link
- External content not in both languages: state the available language after the link, e.g. "(French only)"

## Vanity URLs

- Use sparingly for promotional material (TV, radio, print)
- Use dashes between words, plain language keywords
- Avoid acronyms unless better known than the full form
- Link to main page of a form/publication, not subsections

## Validation checklist

1. Link text describes the destination (no "click here")
2. Unique text for links to different pages
3. Links open in same tab by default
4. Keywords are front-loaded in link text
5. Links placed at end of sentences, not mid-sentence
6. Related links introduced with a verb and colon
7. Email addresses written in full as visible link text
8. Maximum one button link per page
9. Rescue links use "You may be looking for:" with correct classes
