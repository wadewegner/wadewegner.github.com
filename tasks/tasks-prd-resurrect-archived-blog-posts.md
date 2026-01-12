# Tasks: Resurrect Archived Blog Posts

> Generated from: `prds/prd-resurrect-archived-blog-posts.md`

## Relevant Files

- `_migration/migrate.py` - Main migration script containing all extraction, parsing, conversion, and validation logic
- `_migration/requirements.txt` - Python dependencies (beautifulsoup4, markdownify, requests, pyyaml, python-dateutil)
- `_migration/README.md` - Usage instructions for running the migration
- `_migration/extracted/` - Temporary directory for HTML files extracted from Git history
- `_migration/output/` - Staging directory for converted Markdown posts before copying to content/blog/
- `_migration/reports/migration-report.txt` - Validation report generated after migration
- `.gitignore` - Needs update to exclude `_migration/` folder
- `content/blog/` - Final destination for migrated posts

### Notes

- All migration code and artifacts are contained in `_migration/` for easy cleanup
- Run `python _migration/migrate.py --help` to see available commands
- Use `--dry-run` flag to validate without copying to `content/blog/`
- After successful migration, delete the entire `_migration/` folder with `rm -rf _migration/`
- The source commit is `73501339a618c162f2dded3ed7916716e03f77a1`

## Tasks

- [ ] 1.0 Set up migration environment and folder structure
  - [ ] 1.1 Create `_migration/` directory at project root
  - [ ] 1.2 Create subdirectories: `extracted/`, `output/`, `reports/`
  - [ ] 1.3 Create `requirements.txt` with dependencies: beautifulsoup4>=4.12.0, markdownify>=0.11.0, requests>=2.31.0, pyyaml>=6.0, python-dateutil>=2.8.0
  - [ ] 1.4 Add `_migration/` to `.gitignore` to prevent accidental commits
  - [ ] 1.5 Create `README.md` with usage instructions for the migration script
  - [ ] 1.6 Set up Python virtual environment (optional but recommended): `python -m venv _migration/venv`
  - [ ] 1.7 Install dependencies: `pip install -r _migration/requirements.txt`

- [ ] 2.0 Implement HTML extraction from Git history
  - [ ] 2.1 Create `migrate.py` with argument parsing (argparse) supporting commands: `extract`, `convert`, `validate`, `deploy`, and flags: `--dry-run`, `--verbose`
  - [ ] 2.2 Implement function to list all archived file paths from commit `73501339` using `git show <commit> --name-only | grep "^archive/"`
  - [ ] 2.3 Implement function to extract individual HTML file content using `git show <commit>:<path>`
  - [ ] 2.4 Save extracted HTML files to `_migration/extracted/` preserving the original path structure (e.g., `extracted/archive/2007/03/post-slug/index.html`)
  - [ ] 2.5 Log total count of files extracted and any extraction failures
  - [ ] 2.6 Test extraction on 5-10 sample posts to verify files are correctly saved

- [ ] 3.0 Implement HTML parsing (title, date, content extraction)
  - [ ] 3.1 Implement `parse_title()` function: extract from `<title>` tag, strip " - Wade Wegner" suffix
  - [ ] 3.2 Implement `parse_date()` function: extract from `<div class="post-data">`, parse "Mon DD, YYYY" format using dateutil.parser
  - [ ] 3.3 Implement date fallback: if parsing fails, use 1st of month from file path (e.g., `archive/2007/03/...` → `2007-03-01`)
  - [ ] 3.4 Implement `parse_content()` function: extract inner HTML from `<div class="container markdown top-pad">`
  - [ ] 3.5 Implement `parse_description()` function: extract from `<meta name="description">` if present
  - [ ] 3.6 Create `ParseResult` dataclass/dict to hold: title, date, date_is_fallback, content_html, description, original_path
  - [ ] 3.7 Implement error handling: log parsing failures with file path, continue processing other files
  - [ ] 3.8 Test parsing on 10 sample posts from different years to verify extraction accuracy

- [ ] 4.0 Implement HTML to Markdown conversion
  - [ ] 4.1 Configure markdownify with appropriate options: heading_style="ATX", bullets="-", strip=["script", "style", "nav", "footer"]
  - [ ] 4.2 Implement `convert_html_to_markdown()` function using markdownify
  - [ ] 4.3 Implement post-processing: decode HTML entities (&ldquo; → ", &amp; → &, etc.)
  - [ ] 4.4 Implement post-processing: normalize whitespace (remove excessive blank lines, trim trailing whitespace)
  - [ ] 4.5 Implement post-processing: ensure images convert to `![alt](src)` format correctly
  - [ ] 4.6 Implement post-processing: ensure links convert to `[text](href)` format correctly
  - [ ] 4.7 Implement post-processing: ensure code blocks are properly fenced with triple backticks
  - [ ] 4.8 Test conversion on 10 sample posts with varying content (lists, code, images, links) to verify quality

- [ ] 5.0 Implement frontmatter generation and file organization
  - [ ] 5.1 Implement `generate_frontmatter()` function: create YAML frontmatter with title, date (ISO 8601 with -08:00 timezone), draft: false, description
  - [ ] 5.2 Implement title escaping: wrap in double quotes, escape internal quotes
  - [ ] 5.3 Implement `sanitize_slug()` function: lowercase, replace spaces with hyphens, remove special characters, truncate to 50 chars
  - [ ] 5.4 Implement `generate_folder_name()` function: format as `YYYY-MM-DD-slug`
  - [ ] 5.5 Implement duplicate slug detection: if folder exists, append `-2`, `-3`, etc.
  - [ ] 5.6 Implement `write_post()` function: create folder in `_migration/output/`, write `index.md` with frontmatter + content
  - [ ] 5.7 Implement full conversion pipeline: extract → parse → convert → generate frontmatter → write file
  - [ ] 5.8 Test full pipeline on 10 sample posts, manually verify output in `_migration/output/`

- [ ] 6.0 Implement validation suite and reporting
  - [ ] 6.1 Implement `validate_frontmatter()` function: check title exists, date is parseable, required fields present
  - [ ] 6.2 Implement `extract_links()` function: find all markdown links `[text](url)` in content
  - [ ] 6.3 Implement `extract_images()` function: find all markdown images `![alt](url)` in content
  - [ ] 6.4 Implement `check_url()` function: HTTP HEAD request with timeout, return status code (use requests library)
  - [ ] 6.5 Implement batch URL checking with rate limiting (add small delay between requests to avoid overwhelming servers)
  - [ ] 6.6 Implement `generate_report()` function: create detailed report with counts, warnings, errors, broken links list
  - [ ] 6.7 Write report to `_migration/reports/migration-report.txt` in the format specified in PRD
  - [ ] 6.8 Implement `--dry-run` flag: run full validation without copying files to `content/blog/`
  - [ ] 6.9 Implement Hugo build test: run `hugo build` and capture exit code and any errors
  - [ ] 6.10 Test validation on converted sample posts before running on full set

- [ ] 7.0 Run full migration and finalize
  - [ ] 7.1 Run extraction on all 271 posts: `python _migration/migrate.py extract`
  - [ ] 7.2 Run conversion on all extracted posts: `python _migration/migrate.py convert`
  - [ ] 7.3 Run validation in dry-run mode: `python _migration/migrate.py validate --dry-run`
  - [ ] 7.4 Review validation report in `_migration/reports/migration-report.txt`
  - [ ] 7.5 Fix any critical errors identified in the report (if any)
  - [ ] 7.6 Copy staged posts from `_migration/output/` to `content/blog/`: `python _migration/migrate.py deploy`
  - [ ] 7.7 Run `hugo server` locally and spot-check 5-10 random posts in browser
  - [ ] 7.8 Verify Hugo builds without errors: `hugo build`
  - [ ] 7.9 Delete migration folder: `rm -rf _migration/`
  - [ ] 7.10 Stage and commit new blog posts: `git add content/blog/ && git commit -m "Resurrect 271 archived blog posts from 2006-2018"`
