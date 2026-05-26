---
name: live-prototype-comparer
description: >
  Compare a live Canada.ca page against its prototype counterpart and produce
  a Word document (.docx) with tracked changes highlighting every difference.
  Use this skill whenever the user asks to compare a live page to a prototype,
  generate a redline or tracked-change document, diff two web pages into Word,
  or create a before/after comparison document. Also trigger when the user
  provides an Excel file with live URLs and prototype URLs to compare in bulk.
---

# Live Page to Prototype Word Doc Comparer

You are building Word documents that compare the content of live Canada.ca
pages against their prototype counterparts hosted on GitHub Pages. Each
document uses Word tracked changes (insertions and deletions) so reviewers
can see exactly what changed.

## High-level workflow

1. **Collect URL pairs** from user input or an Excel file
2. **Fetch HTML** for both live and prototype pages
3. **Extract structured content** (headings, paragraphs, bullets, dropdowns, tables, links)
4. **Diff** the content at paragraph level, then word level for similar paragraphs
5. **Generate Word documents** with tracked changes, preserving formatting and hyperlinks
6. **Run validation checks** before delivering

## Dependencies

```
pip install python-docx openpyxl beautifulsoup4 lxml requests
```

The GitHub CLI (`gh`) must be authenticated for fetching GitHub Pages content.

## Fetching pages

### Live pages (canada.ca)

Fetch directly via `requests.get()` with a browser-like User-Agent header.
Retry up to 3 times with 2-second delays on failure.

### Prototype pages (GitHub Pages)

**CRITICAL PITFALL**: Many systems have outdated SSL/TLS libraries (LibreSSL)
that cannot connect to `*.github.io` directly. Always implement a fallback
that fetches HTML through the GitHub API:

```python
def github_pages_to_api_path(url):
    """Convert github.io URL to repo + file path for API access."""
    # Example: https://cra-proto.github.io/part-xiii-non-res/path/file.html
    # becomes repo='cra-proto/part-xiii-non-res', path='path/file.html'
    patterns = [
        (r'https?://cra-proto\.github\.io/([^/]+)/(.+)', 'cra-proto'),
        (r'https?://cra-design\.github\.io/([^/]+)/(.+)', 'cra-design'),
    ]
    for pattern, org in patterns:
        m = re.match(pattern, url)
        if m:
            repo_name = m.group(1)
            file_path = m.group(2)
            if file_path.endswith('/'):
                file_path += 'index.html'
            return f'{org}/{repo_name}', file_path
    return None, None

def fetch_from_github_api(repo, file_path):
    """Fetch file content via GitHub API + base64 decode."""
    cmd = ['gh', 'api', f'repos/{repo}/contents/{file_path}', '--jq', '.content']
    result = subprocess.run(cmd, capture_output=True, text=True, timeout=30)
    if result.returncode != 0:
        return None
    return base64.b64decode(result.stdout.strip()).decode('utf-8', errors='replace')
```

**Proactive check**: Always attempt GitHub API first for `*.github.io` URLs.
Never rely solely on direct HTTPS requests to GitHub Pages.

## Content extraction

### Target elements

Extract these HTML elements from inside `<main>` (or fallback containers):

| Element | Type in output | Notes |
|---------|---------------|-------|
| `h1`-`h6` | `heading` (with level) | Bold, sized by level |
| `p` | `para` | Body text |
| `li` | `bullet` (with depth) | Track nesting depth via parent `ul`/`ol` count |
| `summary` | `details` | Collapsible/dropdown headings inside `<details>` |
| `dt` | `dt` | Definition term (bold) |
| `dd` | `dd` | Definition description (indented) |
| `th` | `th` | Table header (bold) |
| `td` | `td` | Table cell |
| `blockquote` | `para` | Treated as paragraph |
| `figcaption` | `para` | Treated as paragraph |

**CRITICAL PITFALL - Missing `<summary>` elements**: Canada.ca pages frequently
use `<details><summary>` for collapsible/accordion sections. If you do not
include `summary` in the extraction list, entire section headings will be
missing from the output. Always verify `<summary>` is in the tag list.

### Stripping non-content elements

Before extraction, decompose these from `<main>`:

- `script`, `style`, `noscript`
- `footer`, `aside`
- `div` with class matching `pagedetails|date-modified|report-a-problem|share-modal`
- `div` with ID matching `def-preFooter|wb-info|gc-info`
- `nav` elements EXCEPT those containing "On this page" (preserve TOC navigation)

### Segment extraction (preserving hyperlinks)

Each element's content must be extracted as a list of **segments**, not flat
text. Each segment is either plain text or a hyperlink:

```python
segments = [
    {'type': 'text', 'text': 'Visit the '},
    {'type': 'link', 'text': 'CRA website', 'url': 'https://...'},
    {'type': 'text', 'text': ' for details.'}
]
```

Walk element children recursively:
- `NavigableString` nodes become text segments
- `<a href="...">` tags become link segments (resolve relative URLs to absolute)
- Other inline tags (`<span>`, `<strong>`, `<em>`) are recursed into

**CRITICAL PITFALL - Relative URL resolution**: Prototype pages on GitHub
Pages use relative URLs. Always resolve them against the page URL:

```python
def resolve_url(href, page_url):
    if not href or href.startswith('#') or href.startswith('javascript:'):
        return href
    if href.startswith('http'):
        return href
    if href.startswith('/'):
        m = re.match(r'(https?://[^/]+)', page_url)
        return m.group(1) + href if m else href
    # Relative path
    from urllib.parse import urljoin
    return urljoin(page_url, href)
```

### Deduplication

**CRITICAL PITFALL - Heading deduplication**: The "On this page" table of
contents contains bullets whose text is identical to the section headings
below. A naive text-based deduplication will drop the actual headings.

**Solution**: Use a composite deduplication key that includes the element type:

```python
elem_type = elem.name  # 'h2', 'li', 'p', 'summary', etc.
text_key = f"{elem_type}::{text[:200]}"
if text_key in seen_texts:
    continue
seen_texts.add(text_key)
```

This allows an `<li>` with text "How Canadian income tax laws apply" and an
`<h2>` with the same text to both appear in the output.

### Block-child filtering

Skip elements that contain block-level children (to avoid double-counting):

```python
block_tags = ['p', 'h1', 'h2', 'h3', 'h4', 'h5', 'h6', 'li', 'ul', 'ol', 'table', 'dl']
has_block_child = any(
    child for child in elem.children
    if isinstance(child, Tag) and child.name in block_tags
)
if has_block_child:
    continue
```

## Serialization for diffing

Convert content items to tagged lines for `difflib.SequenceMatcher`:

| Type | Tag format | Example |
|------|-----------|---------|
| Heading level N | `HN\|\|text` | `H2\|\|Income tax` |
| Details/dropdown | `DET\|\|text` | `DET\|\|Information you may need` |
| Bullet depth N | `BULN\|\|text` | `BUL1\|\|First item` |
| Paragraph | `P\|\|text` | `P\|\|Some paragraph text` |
| Definition term | `DT\|\|text` | `DT\|\|Term` |
| Definition desc | `DD\|\|text` | `DD\|\|Description` |
| Table header | `TH\|\|text` | `TH\|\|Column name` |
| Table cell | `TD\|\|text` | `TD\|\|Cell value` |

Use `||` as delimiter. Parse back with `line.split('||', 1)`.

**CRITICAL PITFALL - Regex delimiter issues**: Never use `|` alone as a
delimiter in regex patterns. The `|` character is regex alternation. Either
escape it (`\|`) or avoid regex entirely with `str.split()`.

## Diffing strategy

### Paragraph-level diff

Use `difflib.SequenceMatcher` on the tagged lines (autojunk=False):

- `equal` lines appear unchanged
- `delete` lines are tracked deletions (from live, not in prototype)
- `insert` lines are tracked insertions (in prototype, not in live)
- `replace` blocks: pair deleted/inserted lines for word-level diffing

### Word-level diff for similar paragraphs

When a delete is followed by an insert of the same type, check similarity:

```python
similarity = difflib.SequenceMatcher(None, old_text.split(), new_text.split()).ratio()
if similarity > 0.3:
    # Use word-level diff instead of full delete + insert
```

Split text into words, diff word arrays, produce equal/delete/insert word runs.

## Word document generation

### Font and sizing

Use Lato font throughout (closest to Canada.ca's Noto Sans). Apply sizes via
XML `w:sz` (half-points):

| Element | Half-points | Points | Bold |
|---------|------------|--------|------|
| H1 | 48 | 24pt | Yes |
| H2 | 36 | 18pt | Yes |
| H3 | 28 | 14pt | Yes |
| H4 | 24 | 12pt | Yes |
| H5 | 22 | 11pt | Yes |
| Details/dropdown | 28 | 14pt | Yes |
| Body text | 22 | 11pt | No |

**CRITICAL PITFALL - Heading styles**: Do NOT rely on python-docx's built-in
heading styles (`doc.add_heading()`). They may not render with distinct sizes
across word processors. Apply bold and size explicitly via `w:rPr` XML.

### Bullet formatting

**CRITICAL PITFALL - List Bullet styles**: python-docx's built-in List Bullet
styles often lack numbering definitions and render as plain paragraphs. Use
manual bullet formatting:

- Prefix each bullet with literal `bullet character + tab`
- Set `w:ind` indentation: `left = depth * 720` twips, `hanging = 360` twips

### Details/dropdown formatting

Render `<summary>` headings with a `triangle right` prefix to indicate they are
collapsible sections on the web page:

```python
if t == 'details':
    return (HEADING_SIZES[3], True, 0, 'triangle right ')
```

### Tracked changes XML

Use OOXML tracked change elements directly:

**Deletion** (`w:del`):
```xml
<w:del w:id="1" w:author="Prototype Review" w:date="2025-01-01T00:00:00Z">
  <w:r><w:rPr>...</w:rPr><w:delText xml:space="preserve">deleted text</w:delText></w:r>
</w:del>
```

**Insertion** (`w:ins`):
```xml
<w:ins w:id="2" w:author="Prototype Review" w:date="2025-01-01T00:00:00Z">
  <w:r><w:rPr>...</w:rPr><w:t xml:space="preserve">inserted text</w:t></w:r>
</w:ins>
```

Each tracked change needs a unique `w:id`. Use a global counter.

For deleted text, use `w:delText` (not `w:t`). For inserted text, use `w:t`.
Always set `xml:space="preserve"` on text elements.

### Paragraph-level tracked changes

For fully deleted or inserted paragraphs, also add paragraph property changes:

**Deleted paragraph**: Add `w:rPr > w:del` and `w:pPrChange` to paragraph properties.

**Inserted paragraph**: Add `w:pPrChange` to paragraph properties.

### Hyperlinks - Field code approach

**CRITICAL PITFALL - `w:hyperlink` compatibility**: The standard OOXML
`w:hyperlink` element with `r:id` relationship works in Microsoft Word but
often fails in macOS Pages, LibreOffice, and other processors. Use HYPERLINK
field codes instead for maximum compatibility:

```python
def add_hyperlink_element(p_elem, doc_part, url, text, font_size_halfpt, bold):
    """Add a clickable hyperlink using HYPERLINK field codes."""
    # Begin field
    fld_begin_run = OxmlElement('w:r')
    fld_begin_char = OxmlElement('w:fldChar')
    fld_begin_char.set(qn('w:fldCharType'), 'begin')
    fld_begin_run.append(fld_begin_char)
    p_elem.append(fld_begin_run)

    # Instruction: HYPERLINK "url"
    instr_run = OxmlElement('w:r')
    instr_text = OxmlElement('w:instrText')
    instr_text.set(qn('xml:space'), 'preserve')
    instr_text.text = f' HYPERLINK "{url}" '
    instr_run.append(instr_text)
    p_elem.append(instr_run)

    # Separate field
    fld_sep_run = OxmlElement('w:r')
    fld_sep_char = OxmlElement('w:fldChar')
    fld_sep_char.set(qn('w:fldCharType'), 'separate')
    fld_sep_run.append(fld_sep_char)
    p_elem.append(fld_sep_run)

    # Visible text with blue underline
    p_elem.append(make_run(text, font_size_halfpt, bold,
                           is_del=False, is_hyperlink=True))

    # End field
    fld_end_run = OxmlElement('w:r')
    fld_end_char = OxmlElement('w:fldChar')
    fld_end_char.set(qn('w:fldCharType'), 'end')
    fld_end_run.append(fld_end_char)
    p_elem.append(fld_end_run)
```

Hyperlink runs must include:
- `w:rStyle val="Hyperlink"` referencing a character style
- Explicit `w:color val="0563C1"` (blue)
- `w:u val="single"` (underline)
- Font name and size matching surrounding text

Create the Hyperlink character style in the document if it does not exist.

### Hyperlinks in tracked changes

For **inserted hyperlinks**, wrap the entire field code structure inside a
`w:ins` element.

For **deleted hyperlinks**, use a `w:del` element containing only the run
with `w:delText` and hyperlink styling (no field code needed since deleted
links should not be clickable).

### Segment source for equal-text paragraphs

**CRITICAL PITFALL - Wrong hyperlinks on unchanged text**: When the paragraph
text is identical between live and prototype but the hyperlinks differ (link
added, removed, or URL changed), you must use the correct segment source.

**Rule**: For `equal` text in the diff, use the **prototype's** segments.
The prototype is the target state. This ensures:
- Links removed in the prototype do not appear
- Links added in the prototype are shown
- Changed URLs reflect the prototype version

```python
if change_type == 'equal':
    item = proto_text_to_item.get(text) or live_text_to_item.get(text)
```

For `delete` change types, use live segments. For `insert`, use prototype segments.

## File naming

When processing from Excel with row numbers, add +1 to the row number in
the filename to match the Excel row (which includes a header row):

```python
filename = f"Row{data_row + 1:02d}_{title_slug}.docx"
```

Slugify titles: remove non-word characters, replace spaces with hyphens,
truncate to 60 characters.

## Document structure

Each Word document should contain:

1. **Title** (24pt bold Lato) - the page title from the spreadsheet
2. **URLs** (9pt grey) - live URL and prototype URL for reference
3. **Breadcrumbs** (10pt italic) - with tracked changes if different
4. **Content** - all extracted elements with tracked changes

## Proactive error-prevention checklist

Run these checks **before** delivering documents to the user:

### Before extraction
- [ ] Confirm both URLs are reachable (retry with GitHub API fallback)
- [ ] Verify `<main>` element exists in HTML (fall back to `<body>`)
- [ ] Check that extraction produces non-zero elements for both pages

### After extraction
- [ ] Verify `<summary>` elements are captured (count `details` type items)
- [ ] Verify section headings from "On this page" TOC also appear as headings later
- [ ] Check that bullet items have non-zero depth values
- [ ] Confirm no content items have empty text
- [ ] Verify `<ol>` items are tagged as ordered (not rendered with bullet dots)
- [ ] Verify alert/notice `<section>` content is captured (not silently dropped)

### After document generation
- [ ] Verify hyperlink field codes are present (check for `w:fldChar` and `w:instrText`)
- [ ] Verify heading elements exist with correct font sizes
- [ ] Verify bullet items have indentation (`w:ind` elements)
- [ ] Confirm the document can be opened without errors
- [ ] Spot-check that "equal" paragraphs use prototype link segments
- [ ] Check word-level diff paragraphs for link presence (links are often lost)

### Cross-document checks
- [ ] All expected documents were generated (none skipped silently)
- [ ] File naming follows the +1 row convention
- [ ] No documents contain only empty bullet points (indicates extraction failure)
- [ ] GitHub API did not silently truncate large files (check file sizes > 100KB)

## Known edge cases

1. **Pages with no `<main>` tag**: Some prototype pages lack a `<main>`.
   Fall back to `<div id="content">`, then `<article>`, then `<body>`.

2. **GitHub Pages with trailing slashes**: URLs ending in `/` need
   `index.html` appended for API access.

3. **Archived repos**: Some prototypes live in `cra-design` (archived).
   The GitHub API still works for reading content from archived repos.

4. **Very long pages**: Pages with 300+ elements are normal for
   Canada.ca. Ensure no timeouts or memory issues.

5. **Identical pages**: When live and prototype are identical, the
   document should contain all content with no tracked changes (all equal).

6. **Missing prototype pages**: If a prototype URL returns 404, skip
   that row and report it in the summary. Never generate a document with
   only one source.

## Additional pitfalls discovered through testing

### Word-level diffs drop all hyperlinks (HIGH severity)

When two paragraphs have similar but not identical text (similarity > 0.3),
the script uses word-level diffing via `add_word_diff_paragraph()`. This
function only handles plain text — all hyperlink information from both the
live and prototype segments is lost. In real-world testing, **67.5% of
word-level diff paragraphs contained hyperlinks** that were silently dropped.

**Solution**: Implement segment-aware word-level diffing. When both the old
and new paragraph have segments, align the word diff output back to the
segment boundaries to preserve link annotations. Alternatively, for
paragraphs where links changed, fall back to full delete + insert (which
preserves links) rather than word-level diff:

```python
# Before doing word-level diff, check if links differ
live_links = [s for s in live_segs if s['type'] == 'link']
proto_links = [s for s in proto_segs if s['type'] == 'link']
if live_links != proto_links:
    # Links changed — use full delete + insert to preserve link info
    add_paragraph_with_segments(doc, live_segs, 'delete', info)
    add_paragraph_with_segments(doc, proto_segs, 'insert', next_info)
else:
    # Only text changed — word-level diff is safe
    add_word_diff_paragraph(doc, word_diffs, next_info)
```

### Ordered lists rendered as unordered bullets (MEDIUM severity)

`<ol>` list items are extracted with type `bullet` and rendered with a `•`
prefix, losing their numbered sequence. Users see bullet dots instead of
"1. 2. 3." for procedural steps.

**Solution**: Distinguish `<ol>` from `<ul>` during extraction:

```python
elif elem.name == 'li':
    parent_list = elem.find_parent(['ul', 'ol'])
    if parent_list and parent_list.name == 'ol':
        item['type'] = 'ordered'
        # Calculate position among siblings
        position = len(list(elem.previous_siblings)) // 2 + 1  # rough
        item['position'] = position
    else:
        item['type'] = 'bullet'
    item['depth'] = get_list_depth(elem)
```

Render ordered items with `{position}.\t` prefix instead of `•\t`.

### Alert/notice sections silently dropped (HIGH severity)

Canada.ca uses `<section class="alert alert-info|warning|danger|success">`
for important notices. These sections are not in the extraction tag list
(`<section>` is not `<p>`, `<li>`, etc.), and their child `<p>` elements may
be filtered by the block-child check. Result: entire alert boxes vanish.

**Solution**: Before extracting, unwrap alert sections by replacing them with
their content plus a synthetic heading:

```python
for section in main.find_all('section', class_=re.compile(r'alert')):
    classes = ' '.join(section.get('class', []))
    alert_type = 'Info'
    if 'alert-warning' in classes: alert_type = 'Warning'
    elif 'alert-danger' in classes: alert_type = 'Danger'
    elif 'alert-success' in classes: alert_type = 'Success'
    # Insert a visible marker before the section's content
    marker = soup.new_tag('p')
    marker.string = f'[{alert_type} notice]'
    marker['class'] = ['alert-marker']
    section.insert(0, marker)
    section.unwrap()  # Remove <section> wrapper, keep children
```

Or add `section` to the extracted tags and treat alert sections as a
distinct content type with visual styling (coloured background or prefix).

### GitHub API 100KB file size limit (MEDIUM severity)

The GitHub contents API (`repos/{owner}/{repo}/contents/{path}`) only returns
inline base64 content for files under ~100KB. Larger files return a `git_url`
instead, requiring a separate blob API call. In testing, one prototype page
was 96.1KB — close to the limit. Larger pages would silently return no
content.

**Solution**: Check the API response for a `content` field. If absent, use
the blob endpoint:

```python
def fetch_from_github_api(repo, file_path):
    cmd = ['gh', 'api', f'repos/{repo}/contents/{file_path}']
    result = subprocess.run(cmd, capture_output=True, text=True, timeout=30)
    if result.returncode != 0:
        return None
    import json
    data = json.loads(result.stdout)
    if 'content' in data:
        return base64.b64decode(data['content']).decode('utf-8', errors='replace')
    elif 'git_url' in data:
        # File too large, use blob API
        blob_cmd = ['gh', 'api', data['git_url'], '--jq', '.content']
        blob_result = subprocess.run(blob_cmd, capture_output=True, text=True, timeout=30)
        if blob_result.returncode == 0:
            return base64.b64decode(blob_result.stdout.strip()).decode('utf-8', errors='replace')
    return None
```

### Content reordering appears as delete + insert (LOW severity)

When the prototype moves a section to a different position without changing
its text, `SequenceMatcher` cannot detect the move. It shows the section as
deleted from its original position and inserted at the new position. This is
technically correct for tracked changes, but reviewers may find it confusing.

**Mitigation**: There is no simple fix since Word tracked changes have no
"move" operation. Document this behavior for reviewers. Optionally, add a
comment or note at the top of the document explaining that moved sections
appear as a paired deletion and insertion.

### French/bilingual page encoding (LOW severity)

Some Canada.ca pages serve French content or bilingual text with accented
characters (e.g., `e with accent`, `c with cedilla`). If the encoding is not handled
properly, these characters may appear garbled. The `requests` library usually
auto-detects encoding, but GitHub API responses are always UTF-8 base64.

**Solution**: Always decode with `errors='replace'` to prevent crashes:

```python
base64.b64decode(content).decode('utf-8', errors='replace')
```

And ensure the HTML parser uses `lxml` which handles encoding declarations
in `<meta>` tags automatically.

### Hardcoded tracked-change metadata (LOW severity)

The script hardcodes `w:author="Prototype Review"` and a static date. When
comparing multiple batches over time, all changes appear to have the same
author and timestamp, making it impossible to distinguish when comparisons
were generated.

**Solution**: Use the current date and allow the author name to be
configurable:

```python
from datetime import datetime
REVIEW_DATE = datetime.utcnow().strftime('%Y-%m-%dT%H:%M:%SZ')
REVIEW_AUTHOR = 'Prototype Review'  # Make configurable
```

## Validation script

After generating all documents, run a validation pass:

```python
for filepath in generated_files:
    doc = Document(filepath)
    body = doc.element.body
    ns = {'w': 'http://schemas.openxmlformats.org/wordprocessingml/2006/main'}

    # Check hyperlinks exist
    fld_chars = body.findall('.//w:fldChar', ns)
    instr_texts = body.findall('.//w:instrText', ns)
    link_count = sum(1 for it in instr_texts
                     if it.text and 'HYPERLINK' in it.text)

    # Check headings exist (look for large font sizes)
    heading_count = 0
    for sz in body.findall('.//w:sz', ns):
        val = sz.get(qn('w:val'))
        if val in ('48', '36', '28', '24'):
            heading_count += 1

    # Check for details/dropdown headings
    has_details = any('triangle right' in (p.text or '') for p in doc.paragraphs)

    # Check for indented bullets
    ind_elems = body.findall('.//w:ind', ns)
    bullet_count = sum(1 for ind in ind_elems
                       if ind.get(qn('w:hanging')) == '360')

    print(f"{os.path.basename(filepath)}: "
          f"{link_count} links, {heading_count} headings, "
          f"{bullet_count} bullets, details={'Yes' if has_details else 'No'}")
```
