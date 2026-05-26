# live-prototype-comparer

Compares live Canada.ca pages against their prototype counterparts and generates Word documents (.docx) with tracked changes showing every difference.

## What it does

Fetches HTML from both a live page and its prototype, extracts all structured content (headings, paragraphs, bullets, collapsible sections, tables, hyperlinks), diffs the content, and produces a Word document where additions appear as tracked insertions and removals appear as tracked deletions.

## When to use

- Comparing a live Canada.ca page against a GitHub Pages prototype
- Generating redline/tracked-change documents for content review
- Bulk-processing URL pairs from an Excel spreadsheet into comparison Word docs
- Any before/after web page comparison that needs to be in Word format

## Key features

- Extracts all content types: headings (H1-H6), paragraphs, bullets (nested), collapsible/dropdown sections (`<details><summary>`), definition lists, table cells
- Preserves and renders clickable hyperlinks using HYPERLINK field codes (compatible with Word, Pages, and LibreOffice)
- Word-level tracked changes for paragraphs with partial edits
- Handles GitHub Pages SSL issues via GitHub API fallback
- Proper heading hierarchy with distinct sizes and bold formatting
- Indented bullets with depth tracking
- Deduplication that preserves headings even when "On this page" TOC has matching text

## Pitfalls it prevents

- SSL/TLS failures fetching GitHub Pages (uses API fallback)
- Missing `<summary>`/dropdown headings (explicitly extracted)
- Heading deduplication dropping section headings that match TOC bullets (composite key)
- Invisible hyperlinks in non-Word processors (field codes instead of w:hyperlink)
- Wrong hyperlinks on unchanged text (uses prototype segments for equal paragraphs)
- Empty bullets from missing numbering definitions (manual bullet character + indent)
- Indistinct heading sizes (explicit XML font sizing, not style-based)
- Word-level diffs silently dropping all hyperlinks (67.5% of diffs affected in testing)
- Ordered lists rendered with bullet dots instead of numbers
- Alert/notice `<section>` content silently vanishing from output
- GitHub API 100KB file size limit causing silent failures on large pages
- Content reordering appearing as confusing delete + insert pairs
- Hardcoded tracked-change dates making batch comparisons indistinguishable

## Files

- `SKILL.md` — Full skill definition with code patterns, XML structures, pitfall documentation, and validation checklist
