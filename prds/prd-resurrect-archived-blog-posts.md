# PRD: Resurrect Archived Blog Posts

## Introduction/Overview

This project will resurrect 271 archived blog posts from 2006-2018 that were previously archived in commit `73501339a618c162f2dded3ed7916716e03f77a1`. The archived posts exist as Hugo-generated static HTML files in Git history and need to be converted back to Hugo-compatible Markdown files with proper frontmatter, then placed in the current `/content/blog/` directory structure.

### Problem Statement

Years ago, 271 blog posts were archived by moving them from `docs/` to `archive/` as static HTML files. These posts contain valuable historical content but are no longer accessible on the live site. The goal is to bring them back into the active blog using the current Hugo site structure.

### Source Data

- **Location**: Git commit `73501339a618c162f2dded3ed7916716e03f77a1`
- **Path in commit**: `archive/YYYY/MM/post-slug/index.html`
- **Format**: Hugo-generated static HTML pages
- **Total posts**: 271

### Target Structure

```
content/blog/
├── YYYY-MM-DD-post-slug/
│   └── index.md
```

---

## Goals

1. **Extract** all 271 archived HTML files from Git history
2. **Parse** HTML to extract title, date, and content
3. **Convert** HTML content to clean Markdown
4. **Generate** Hugo-compatible frontmatter for each post
5. **Organize** posts into the correct folder structure (`YYYY-MM-DD-post-slug/`)
6. **Preserve** all image references (external URLs)
7. **Validate** the migration with automated testing
8. **Document** any posts that fail conversion for manual review

---

## User Stories

1. **As a blog owner**, I want my archived posts restored to the live site so that historical content is accessible again.

2. **As a blog owner**, I want the migrated posts to have accurate dates so that they appear correctly in chronological order.

3. **As a blog owner**, I want external image links preserved so that images continue to display correctly.

4. **As a blog owner**, I want automated validation so that I can trust the migration was successful without manually checking 271 posts.

---

## Functional Requirements

### Phase 1: Extraction

| ID | Requirement |
|----|-------------|
| FR-1.1 | The system MUST extract all HTML files from the `archive/` directory in commit `73501339a618c162f2dded3ed7916716e03f77a1` |
| FR-1.2 | The system MUST preserve the original file path structure for reference during conversion |
| FR-1.3 | The system MUST extract files to `_migration/extracted/` directory |
| FR-1.4 | The system MUST log the total count of files extracted |

### Phase 2: HTML Parsing

| ID | Requirement |
|----|-------------|
| FR-2.1 | The system MUST extract the post title from the `<title>` tag (format: "Title - Wade Wegner") |
| FR-2.2 | The system MUST extract the publication date from `<div class="post-data">` (format: "Mon DD, YYYY") |
| FR-2.3 | The system MUST extract the main content from `<div class="container markdown top-pad">` |
| FR-2.4 | The system MUST handle posts where date extraction fails by using the 1st of the month from the path |
| FR-2.5 | The system MUST preserve all `<img>` tags with their `src` and `alt` attributes |
| FR-2.6 | The system MUST preserve all `<a>` tags with their `href` attributes |
| FR-2.7 | The system MUST log any parsing failures with the file path for manual review |

### Phase 3: HTML to Markdown Conversion

| ID | Requirement |
|----|-------------|
| FR-3.1 | The system MUST convert HTML content to valid Markdown |
| FR-3.2 | The system MUST convert `<h1>` through `<h6>` to Markdown headers (`#` through `######`) |
| FR-3.3 | The system MUST convert `<p>` tags to paragraphs with proper spacing |
| FR-3.4 | The system MUST convert `<ul>` and `<ol>` lists to Markdown lists |
| FR-3.5 | The system MUST convert `<a href="...">text</a>` to `[text](...)` |
| FR-3.6 | The system MUST convert `<img src="..." alt="...">` to `![alt](...)` |
| FR-3.7 | The system MUST convert `<code>` and `<pre>` blocks to Markdown code blocks |
| FR-3.8 | The system MUST convert `<strong>` and `<b>` to `**text**` |
| FR-3.9 | The system MUST convert `<em>` and `<i>` to `*text*` |
| FR-3.10 | The system MUST handle nested HTML structures correctly |
| FR-3.11 | The system MUST decode HTML entities (e.g., `&ldquo;` → `"`, `&amp;` → `&`) |

### Phase 4: Frontmatter Generation

| ID | Requirement |
|----|-------------|
| FR-4.1 | The system MUST generate Hugo-compatible YAML frontmatter |
| FR-4.2 | The frontmatter MUST include `title` (extracted from HTML) |
| FR-4.3 | The frontmatter MUST include `date` in ISO 8601 format (e.g., `2007-03-13T00:00:00-08:00`) |
| FR-4.4 | The frontmatter MUST include `draft: false` |
| FR-4.5 | The frontmatter SHOULD include `description` if extractable from meta tags |
| FR-4.6 | The system MUST properly escape special characters in titles (quotes, colons) |

**Example frontmatter:**
```yaml
---
title: "Professional Commerce Server 2007"
date: 2007-03-13T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---
```

### Phase 5: File Organization

| ID | Requirement |
|----|-------------|
| FR-5.1 | The system MUST create folders in format `YYYY-MM-DD-post-slug/` |
| FR-5.2 | The system MUST place `index.md` inside each post folder |
| FR-5.3 | The system MUST sanitize slugs (lowercase, hyphens, no special characters) |
| FR-5.4 | The system MUST handle duplicate slugs by appending a suffix (e.g., `-2`) |
| FR-5.5 | The system MUST first write output to `_migration/output/` for staging/review |
| FR-5.6 | The system MUST provide a command to copy staged posts to `/content/blog/` after validation |

### Phase 6: Validation & Testing

| ID | Requirement |
|----|-------------|
| FR-6.1 | The system MUST validate that all 271 posts were processed |
| FR-6.2 | The system MUST validate that each `index.md` has valid frontmatter |
| FR-6.3 | The system MUST validate that frontmatter `date` is parseable |
| FR-6.4 | The system MUST check all internal links for validity |
| FR-6.5 | The system MUST check all external image URLs for HTTP 200 response |
| FR-6.6 | The system MUST generate a validation report in `_migration/reports/` with: |
|        | - Total posts processed |
|        | - Posts with successful conversion |
|        | - Posts with warnings (e.g., missing date) |
|        | - Posts with errors (failed conversion) |
|        | - Broken links (internal and external) |
| FR-6.7 | The system MUST run `hugo build` to verify no build errors |
| FR-6.8 | The system MUST support a `--dry-run` mode that validates without copying to `content/blog/` |

---

## Non-Goals (Out of Scope)

1. **URL Redirects**: Old URLs will not be redirected; this is a clean break
2. **Comment Migration**: Disqus comments embedded in HTML will not be migrated
3. **Analytics Scripts**: Google Analytics and other tracking scripts will not be preserved
4. **Local Image Migration**: Images are hosted externally; we will not download and re-host them
5. **Content Editing**: Post content will not be modified beyond HTML→Markdown conversion
6. **SEO Optimization**: Meta tags beyond description will not be migrated

---

## Cleanup Plan

After successful migration:

```bash
# 1. Verify migration was successful
hugo server  # Check site builds and posts render correctly

# 2. Remove migration folder
rm -rf _migration/

# 3. Remove from .gitignore (optional)
# Edit .gitignore and remove the _migration/ line

# 4. Commit the new blog posts
git add content/blog/
git commit -m "Resurrect 271 archived blog posts from 2006-2018"
```

The `_migration/` folder contains:
- All Python scripts
- Extracted HTML files (temporary)
- Staged output files (temporary)  
- Validation reports
- Virtual environment

**Nothing in `_migration/` is needed after the posts are moved to `content/blog/`.**

---

## Technical Considerations

### Recommended Approach: Python Script

A **single Python script** (`migrate.py`) is recommended due to:
- Strong HTML parsing libraries (`BeautifulSoup`)
- Reliable HTML-to-Markdown conversion (`markdownify` or `html2text`)
- Easy file system operations
- Good error handling and logging
- Self-contained in one file for easy cleanup

### Dependencies

```
beautifulsoup4>=4.12.0
markdownify>=0.11.0
requests>=2.31.0
pyyaml>=6.0
python-dateutil>=2.8.0
```

### Git Operations

```bash
# Extract files from specific commit
git show <commit>:<path> > output_file

# List all archived files
git show <commit> --name-only | grep "^archive/"
```

### HTML Structure Reference

**Title extraction:**
```html
<title>Professional Commerce Server 2007 - Wade Wegner</title>
```
→ Extract text before " - Wade Wegner"

**Date extraction:**
```html
<div class="post-data" style="font-weight: bold;">
    Mar 13, 2007 |
    1 minute read
</div>
```
→ Parse "Mar 13, 2007" using `dateutil.parser`

**Content extraction:**
```html
<div class="container markdown top-pad">
    <!-- Blog content here -->
</div>
```

### Edge Cases to Handle

1. **Missing dates**: Fall back to `YYYY-MM-01` from path
2. **Malformed HTML**: Log and skip, add to manual review list
3. **Special characters in titles**: Escape quotes, wrap in double quotes
4. **Empty content**: Log warning, create minimal post
5. **Duplicate slugs on same date**: Append `-2`, `-3`, etc.
6. **Very long slugs**: Truncate to 50 characters

---

## Implementation Plan

### Step 1: Create Migration Script Structure

All migration code, temporary files, and artifacts will be contained in a **single disposable folder** at the project root:

```
_migration/                     # ← DELETE THIS FOLDER WHEN DONE
├── migrate.py                  # Main migration script (single file)
├── requirements.txt            # Python dependencies
├── README.md                   # Usage instructions
├── extracted/                  # Temporary: extracted HTML files
│   └── archive/
│       └── YYYY/MM/slug/index.html
├── output/                     # Temporary: converted posts (staged)
│   └── YYYY-MM-DD-slug/
│       └── index.md
├── reports/                    # Validation reports
│   └── migration-report.txt
└── venv/                       # Python virtual environment (optional)
```

**Important**: 
- The `_migration/` folder is prefixed with `_` to sort it first and make it obvious it's temporary
- Add `_migration/` to `.gitignore` to prevent accidental commits
- After successful migration, simply `rm -rf _migration/` to clean up

### Step 2: Development Phases

| Phase | Description | Estimated Effort |
|-------|-------------|------------------|
| 1 | Script setup, extraction logic | 1-2 hours |
| 2 | HTML parsing (title, date, content) | 2-3 hours |
| 3 | Markdown conversion | 2-3 hours |
| 4 | Frontmatter generation | 1 hour |
| 5 | File organization | 1 hour |
| 6 | Validation suite | 2-3 hours |
| 7 | Testing & debugging | 2-4 hours |
| **Total** | | **11-17 hours** |

### Step 3: Testing Strategy

1. **Unit tests**: Test each parser/converter function
2. **Sample migration**: Run on 10 posts first, manually verify in `_migration/output/`
3. **Full migration**: Run on all 271 posts to `_migration/output/`
4. **Automated validation**: Run validation suite, review `_migration/reports/`
5. **Hugo build test**: Copy to `content/blog/`, ensure site builds without errors
6. **Visual spot-check**: Preview 5-10 random posts in browser
7. **Cleanup**: Delete `_migration/` folder after confirming success

---

## Success Metrics

| Metric | Target |
|--------|--------|
| Posts successfully migrated | 271 (100%) |
| Posts with valid frontmatter | 271 (100%) |
| Posts with accurate dates | ≥260 (96%) |
| External images loading | ≥95% |
| Hugo build success | Yes |
| Zero critical conversion errors | Yes |

---

## Validation Report Template

After migration, the script will generate a report:

```
========================================
ARCHIVE MIGRATION REPORT
========================================
Generated: 2026-01-11 10:30:00

SUMMARY
-------
Total HTML files found:     271
Successfully converted:     268
Conversion warnings:        3
Conversion errors:          0

DATE EXTRACTION
---------------
Exact dates extracted:      250
Fallback dates used:        21

LINK VALIDATION
---------------
Total links checked:        1,542
Working links:              1,501
Broken links:               41

IMAGE VALIDATION  
----------------
Total images checked:       892
Images loading (HTTP 200):  845
Images broken:              47

POSTS REQUIRING REVIEW
----------------------
1. 2007/03/some-post-slug - Warning: Date fallback used
2. 2010/05/another-post - Warning: Empty content
...

HUGO BUILD
----------
Status: SUCCESS
Warnings: 0
Errors: 0
========================================
```

---

## Open Questions

1. **Timezone for dates**: Should all dates use `-08:00` (PST) or a neutral timezone?
   - **Proposed**: Use `-08:00` to match existing posts

2. **Draft status**: Should resurrected posts be `draft: false` immediately?
   - **Decided**: Yes, `draft: false` — these are being published

3. **Broken external images**: Should we attempt to find archived versions (Wayback Machine)?
   - **Proposed**: No, out of scope — log broken images for optional manual fix later

4. **Post descriptions**: If meta description is missing, should we auto-generate from content?
   - **Proposed**: No, leave description empty rather than generate potentially poor summaries

---

## Appendix: Post Distribution by Year

| Year | Count | Notes |
|------|-------|-------|
| 2006 | 4 | Earliest posts |
| 2007 | 94 | Commerce Server, BizTalk era |
| 2008 | 12 | |
| 2009 | 21 | Azure early days |
| 2010 | 40 | Azure AppFabric |
| 2011 | 44 | Windows Azure toolkits |
| 2012 | 10 | |
| 2013 | 15 | Salesforce transition |
| 2014 | 17 | Salesforce toolkits |
| 2015 | 1 | |
| 2017 | 8 | Salesforce DX |
| 2018 | 5 | |
| **Total** | **271** | |

---

## Appendix: Sample Conversion

### Input (HTML)

```html
<title>Professional Commerce Server 2007 - Wade Wegner</title>
...
<div class="post-data" style="font-weight: bold;">
    Mar 13, 2007 |
    1 minute read
</div>
...
<div class="container markdown top-pad">
    <p>I have just closed a deal with Wrox to write a book on Commerce Server 2007!</p>
    <p>This book will be part of the Professional series...</p>
    <ul>
        <li>Best practices</li>
        <li>Architecture</li>
    </ul>
</div>
```

### Output (Markdown)

**File**: `content/blog/2007-03-13-professional-commerce-server-2007/index.md`

```markdown
---
title: "Professional Commerce Server 2007"
date: 2007-03-13T00:00:00-08:00
draft: false
description: "From the whiteboard to the keyboard"
---

I have just closed a deal with Wrox to write a book on Commerce Server 2007!

This book will be part of the Professional series...

- Best practices
- Architecture
```
