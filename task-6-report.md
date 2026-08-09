# Task 6 report

## Application files

- `_pages/publications.html`
- `_includes/publication-list-item.html`
- `_pages/teaching.html`
- `_includes/teaching-list-item.html`

Publications use the authoritative `sort_order`, are grouped under `Publications` and `Preprints`, and render only present metadata and confirmed link fields. Teaching entries use descending `date` order and are grouped by `academic_year`.

## Checks

- Static Liquid contract: passed; required classes, fields, includes, sort controls, and balanced Liquid delimiters verified.
- Front matter: passed; 13 publication files and 10 teaching files parsed, with unique `sort_order` values 1–13 and required teaching fields present.
- `git diff --check`: passed.
- Jekyll build: skipped at the user’s request. `bundle check` could not satisfy the Gemfile dependencies; the known missing `jekyll-sitemap` dependency remains the build blocker.

## Commit

- `7a30862` — `Render publications and teaching archives`
