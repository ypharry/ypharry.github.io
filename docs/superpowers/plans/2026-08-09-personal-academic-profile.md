# Personal Academic Profile Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Transform the Academic Pages template into a lightweight, warm academic profile for Yipeng (Harry) Wang with a non-blocking expandable sidebar, complete publications and preprints, semester-grouped teaching, and a clear CV link.

**Architecture:** Keep the existing Jekyll/GitHub Pages pipeline and collection-based content model. Replace template identity/content and add a small profile layout layer that wraps the existing single/archive layouts, renders a sidebar using native disclosure semantics, and uses CSS grid to push content aside when the menu expands. Publication and teaching archive items will use focused includes rather than the template’s generic archive cards.

**Tech Stack:** Jekyll, Liquid, Markdown, Sass/CSS, existing Academic Pages theme, GitHub Pages build, native HTML `<details>`/`<summary>` interaction; no new framework or client-side dependency.

## Global Constraints

- Use the existing Jekyll and GitHub Pages build pipeline.
- Use a warm ivory background, dark brown-black text, and muted brick-red accents.
- Use first-person homepage copy for Yipeng (Harry) Wang, Instructor at Princeton University.
- Do not include a portrait in the initial version.
- Keep the sidebar lightweight, accessible, and non-blocking; it must push content instead of covering it.
- On small screens, expand the sidebar in normal document flow so content remains visible.
- Do not introduce React, Vue, a new application framework, or unnecessary JavaScript.
- Replace template sample identity, publications, talks, teaching, posts, portfolio items, and default template links with real information or remove them from the public site.
- Use the custom repository metadata invocation in this workspace: `git --git-dir=.site-git --work-tree=.`.
- Keep visual-companion files under `.superpowers/` out of commits.

---

## Task 1: Collect the authoritative academic content inputs

**Files:**
- Read: `docs/superpowers/specs/2026-08-09-personal-academic-profile-design.md`
- Inputs from user: contact URLs, publications/preprints, teaching history, current CV PDF location

**Interfaces:**
- Produces the exact values used by Tasks 2, 3, 6, and 7.

- [ ] **Step 1: Request the contact values**

  Collect the exact email address, Princeton profile URL, Google Scholar URL, and ORCID URL. Use the provided values verbatim in `_config.yml`; do not infer URLs from a username.

- [ ] **Step 2: Request the publication and preprint inventory**

  For each entry, collect title, author list, publication/preprint category, date or year, venue or arXiv identifier, and available PDF/DOI/arXiv links. Keep the user’s author ordering and publication wording.

- [ ] **Step 3: Request the teaching inventory**

  For each course, collect academic year, semester, course title, role, institution/department, and any public course link or short note.

- [ ] **Step 4: Confirm the current CV artifact**

  Confirm the PDF file to link from `/cv/` and whether it should be copied to `files/cv.pdf`. Do not copy `cv_style.cls` or `CV_template.tex` until the user explicitly supplies them for the separate CV workflow.

---

## Task 2: Replace global identity and navigation configuration

**Files:**
- Modify: `_config.yml`
- Modify: `_data/navigation.yml`
- Modify: `_pages/about.md`

**Interfaces:**
- Consumes: Task 1’s identity and contact values.
- Produces: The site-wide author object, homepage copy, and four-link primary navigation used by the sidebar. Task 7 owns the CV page and generated PDF so that the page is never committed with a broken link.

- [ ] **Step 1: Add a pre-change validation command**

  Run:

  ```bash
  rg -n "Your Name|academicpages|Red Brick University|none@example.org|GitHub University|Skill 1" _config.yml _data/navigation.yml _pages/about.md _pages/cv.md
  ```

  Expected: matches are present before the replacement, proving the validation detects template content.

- [ ] **Step 2: Configure the site identity**

  In `_config.yml`, set the title/name to `Yipeng (Harry) Wang`, set the description to a concise first-person differential-geometry profile, set the site URL/repository for `ypharry.github.io`, remove the avatar dependency, and populate only the confirmed email, Princeton profile, Google Scholar, and ORCID values. Remove default sample employer/location/bio values.

- [ ] **Step 3: Reduce navigation to the approved destinations**

  Replace `_data/navigation.yml` with four entries in this order: Home (`/`), Publications (`/publications/`), Teaching (`/teaching/`), and CV (`/cv/`). Do not include Talks, Portfolio, Blog Posts, Guide, or the JSON CV page.

- [ ] **Step 4: Replace the homepage source**

  Rewrite `_pages/about.md` with root permalink `/`, `layout: single`, `author_profile: true`, no rendered page title, and first-person copy that introduces Harry’s Princeton role, Columbia advising history, and differential-geometry research. Link to publications, teaching, and CV using normal site links.

- [ ] **Step 5: Validate configuration content**

  Run:

  ```bash
  ! rg -n "Your Name|academicpages|Red Brick University|none@example.org|GitHub University|Skill 1" _config.yml _data/navigation.yml _pages/about.md
  ```

  Expected: exit code 0 with no output.

- [ ] **Step 6: Commit the identity and navigation change**

  ```bash
  git --git-dir=.site-git --work-tree=. add _config.yml _data/navigation.yml _pages/about.md
  git --git-dir=.site-git --work-tree=. commit -m "Replace template identity and navigation"
  ```

---

## Task 3: Replace template collections and remove demo pages

**Files:**
- Delete: all sample files in `_publications/`, `_teaching/`, `_talks/`, `_posts/`, and `_portfolio/`
- Delete: `_data/comments/`, `_data/cv.json`, and template-only pages `_pages/archive-layout-with-content.md`, `_pages/category-archive.html`, `_pages/collection-archive.html`, `_pages/cv-json.md`, `_pages/markdown.md`, `_pages/non-menu-page.md`, `_pages/page-archive.html`, `_pages/portfolio.html`, `_pages/tag-archive.html`, `_pages/talkmap.html`, `_pages/talks.html`, `_pages/terms.md`, and `_pages/year-archive.html`
- Preserve: `_pages/404.md` and `_pages/sitemap.md`
- Create: real publication files under `_publications/`
- Create: real teaching files under `_teaching/`

**Interfaces:**
- Consumes: Task 1’s publication/preprint and teaching inventories.
- Produces: Only real academic collection documents for the public site.

- [ ] **Step 1: Identify all template content before removal**

  Run:

  ```bash
  rg -n "Paper Title|Teaching experience|Your Name|GitHub University|Blog post|Portfolio" _publications _teaching _talks _posts _portfolio _pages _data
  ```

  Expected: the command identifies the sample content that will be removed; no user-authored content should match these template phrases.

- [ ] **Step 2: Remove sample collection entries and template-only pages**

  Delete only the files listed above. Do not delete theme partials, build configuration, the 404 page, or the sitemap page.

- [ ] **Step 3: Add publication and preprint documents**

  Create one Markdown file per real entry with this front matter contract:

  ```yaml
  title: "Exact publication title"
  collection: publications
  category: publications # or preprints
  date: YYYY-MM-DD
  authors: "Exact author list"
  venue: "Journal, conference, or Preprint"
  paperurl: "https://..." # omit when unavailable
  doi: "https://doi.org/..." # omit when unavailable
  arxiv: "https://arxiv.org/..." # omit when unavailable
  permalink: /publication/slug
  ```

  Use only links confirmed in Task 1. Do not add invented abstracts, citations, or venues.

- [ ] **Step 4: Add teaching documents**

  Create one Markdown file per course with this front matter contract:

  ```yaml
  title: "Course title"
  collection: teaching
  academic_year: "2025–26"
  semester: "Fall"
  role: "Instructor"
  venue: "Princeton University"
  date: YYYY-MM-DD
  permalink: /teaching/YYYY-YY-semester-course-slug
  ```

  Put only confirmed course details and notes in the Markdown body.

- [ ] **Step 5: Validate that template content is gone**

  Run:

  ```bash
  ! rg -n "Paper Title|Teaching experience|Your Name|GitHub University|Blog post|Portfolio|Academic Pages is a ready-to-fork" _publications _teaching _talks _posts _portfolio _pages _data 2>/dev/null
  ```

  Expected: exit code 0 with no output.

- [ ] **Step 6: Commit the content replacement**

  ```bash
  git --git-dir=.site-git --work-tree=. add -A _publications _teaching _talks _posts _portfolio _pages _data
  git --git-dir=.site-git --work-tree=. commit -m "Replace template content with academic profile data"
  ```

---

## Task 4: Implement the non-blocking expandable sidebar

**Files:**
- Modify: `_includes/sidebar.html`
- Modify: `_layouts/single.html`
- Modify: `_layouts/archive.html`
- Modify: `_includes/masthead.html`
- Create: `_sass/layout/_profile.scss`
- Modify: `assets/css/main.scss`

**Interfaces:**
- Consumes: `site.author` and `site.data.navigation.main` from Task 2.
- Produces: A reusable `.profile-sidebar` and `.profile-layout` structure used by homepage, archive pages, and CV content.

- [ ] **Step 1: Add a structural smoke check for the sidebar contract**

  After the first markup draft, verify it contains a native disclosure trigger and the expected links:

  ```bash
  rg -n "profile-sidebar|profile-rail|profile-panel|details|summary|site.data.navigation.main|googlescholar|orcid" _includes/sidebar.html
  ```

  Expected: all selectors/data sources appear in the sidebar include.

- [ ] **Step 2: Replace the sidebar markup**

  Build `_includes/sidebar.html` around:

  ```html
  <details class="profile-sidebar">
    <summary class="profile-rail">MENU · HW</summary>
    <div class="profile-panel">
      <!-- name, role, affiliation, navigation links, and confirmed academic links -->
    </div>
  </details>
  ```

  Render the identity from `site.author`, render navigation from `site.data.navigation.main`, and render only the confirmed Email, Princeton profile, Google Scholar, and ORCID links. Do not render an avatar or Follow button.

- [ ] **Step 3: Add the push-open layout rules**

  In `_sass/layout/_profile.scss`, make `.profile-layout` a grid with a narrow first column for the rail and a flexible content column. Expand the first column when `.profile-sidebar` is hovered, focused, or open; the content column must shrink/reflow rather than be covered. Keep the transition limited to grid columns and panel opacity/transform.

- [ ] **Step 4: Add mobile flow behavior**

  At the mobile breakpoint, switch `.profile-layout` to one column. Keep the panel in normal document flow when `details[open]` is active. Do not position the panel over the article or archive content.

- [ ] **Step 5: Remove duplicate global navigation**

  Replace `_includes/masthead.html` with a minimal non-duplicating header, or remove its rendered navigation while preserving the default layout shell. The only primary navigation should be the profile sidebar. Do not add a replacement large menu script.

- [ ] **Step 6: Apply the layout wrapper consistently**

  Add `class="profile-layout"` to the `#main` wrappers in `_layouts/single.html` and `_layouts/archive.html`. Keep article/archive semantics and existing collection rendering intact.

- [ ] **Step 7: Import the profile stylesheet**

  Add `"layout/profile"` to `assets/css/main.scss` after the existing layout imports so the focused profile rules load after the theme’s base sidebar rules.

- [ ] **Step 8: Validate the no-overlap invariant**

  Build the site and inspect the generated markup/styles:

  ```bash
  bundle exec jekyll build --trace
  rg -n "profile-layout|profile-sidebar|profile-panel|profile-rail" _site
  ```

  Expected: the build succeeds and the generated pages contain the profile layout; the panel is not positioned with `position: fixed` or `position: absolute` over the main content.

- [ ] **Step 9: Commit the sidebar layout**

  ```bash
  git --git-dir=.site-git --work-tree=. add _includes/sidebar.html _layouts/single.html _layouts/archive.html _includes/masthead.html _sass/layout/_profile.scss assets/css/main.scss
  git --git-dir=.site-git --work-tree=. commit -m "Add non-blocking profile navigation"
  ```

---

## Task 5: Apply the warm ivory visual system

**Files:**
- Modify: `_sass/theme/_default_light.scss`
- Modify: `_sass/layout/_profile.scss`
- Modify: `_sass/layout/_page.scss` only where needed to remove template-specific spacing/title treatment
- Modify: `_sass/layout/_archive.scss` only where needed to make lists quiet and readable

**Interfaces:**
- Consumes: The profile classes produced by Task 4.
- Produces: The approved warm ivory, restrained academic visual system shared by home, publications, teaching, and CV pages.

- [ ] **Step 1: Set the approved palette**

  Update the light theme variables to use warm ivory for `--global-bg-color`, dark brown-black for text, a soft warm border, and muted brick red for links. Keep contrast readable and do not add gradients or decorative image backgrounds.

- [ ] **Step 2: Define focused profile typography and spacing**

  Use the existing system font variables, moderate heading sizes, generous section spacing, and thin separators. Remove the default oversized homepage title treatment; the homepage’s visible heading must be the user’s name with a compact research label.

- [ ] **Step 3: Style publication and teaching rows**

  Add styles for `.publication-list`, `.publication-item`, `.teaching-list`, and `.teaching-item` using text rows, subtle borders, and inline links. Do not introduce card grids, badges, filter controls, or decorative icons.

- [ ] **Step 4: Validate palette and responsive CSS**

  Run:

  ```bash
  bundle exec jekyll build --trace
  git diff --check
  ```

  Expected: successful build and no whitespace errors.

- [ ] **Step 5: Commit the visual system**

  ```bash
  git --git-dir=.site-git --work-tree=. add _sass/theme/_default_light.scss _sass/layout/_profile.scss _sass/layout/_page.scss _sass/layout/_archive.scss
  git --git-dir=.site-git --work-tree=. commit -m "Apply warm academic visual system"
  ```

---

## Task 6: Build complete publications and semester-grouped teaching pages

**Files:**
- Modify: `_pages/publications.html`
- Create: `_includes/publication-list-item.html`
- Modify: `_pages/teaching.html`
- Create: `_includes/teaching-list-item.html`

**Interfaces:**
- Consumes: publication and teaching front matter from Task 3.
- Produces: Complete list pages with stable, simple markup and no filters.

- [ ] **Step 1: Define descending collection order**

  In each page, sort the collection by `date` and reverse it:

  ```liquid
  {% assign entries = site.publications | sort: "date" | reverse %}
  ```

  Use the equivalent expression for teaching. Do not rely on filesystem order.

- [ ] **Step 2: Render publication sections**

  Group entries by `category`, with headings `Publications` and `Preprints`. For each entry, render title, authors, venue/year, and only the confirmed available links. Use `publication-list-item.html` to keep the row markup in one place.

- [ ] **Step 3: Render teaching groups**

  Iterate over sorted teaching entries and emit an academic-year heading when `academic_year` changes. Under each year, render semester, course title, role, institution, and optional confirmed link/note using `teaching-list-item.html`.

- [ ] **Step 4: Validate generated list content**

  Run:

  ```bash
  bundle exec jekyll build --trace
  rg -n "Publications|Preprints|Teaching|academic_year|Paper Title|Teaching experience" _site/publications _site/teaching
  ```

  Expected: real section headings and entries appear; template phrases do not.

- [ ] **Step 5: Commit the archive pages**

  ```bash
  git --git-dir=.site-git --work-tree=. add _pages/publications.html _includes/publication-list-item.html _pages/teaching.html _includes/teaching-list-item.html
  git --git-dir=.site-git --work-tree=. commit -m "Render publications and teaching archives"
  ```

---

## Task 7: Establish the separate CV workspace and link

**Files:**
- Create: `cv/README.md`
- Modify: `_pages/cv.md`
- Create or copy only after confirmation: `files/cv.pdf`

**Interfaces:**
- Consumes: Task 1’s confirmed current CV PDF decision.
- Produces: A clean CV page and a reserved source area for future `.tex`/`.cls` maintenance.

- [ ] **Step 1: Create the CV workspace guide**

  Add `cv/README.md` documenting that the future source set is `cv/CV_template.tex` and `cv/cv_style.cls`, with generated output at `files/cv.pdf`. State that generated auxiliary files should not be committed.

- [ ] **Step 2: Add the confirmed PDF**

  If Task 1 confirms the PDF, copy it to `files/cv.pdf` and link it from `_pages/cv.md`. If the PDF is not yet ready, stop before adding a broken link and report the single missing input to the user.

- [ ] **Step 3: Validate the CV link**

  Run:

  ```bash
  bundle exec jekyll build --trace
  test -f _site/files/cv.pdf
  rg -n "files/cv.pdf" _site/cv/index.html
  ```

  Expected: the PDF exists in the generated site and the CV page links to it.

- [ ] **Step 4: Commit the CV structure**

  ```bash
  git --git-dir=.site-git --work-tree=. add cv _pages/cv.md files/cv.pdf
  git --git-dir=.site-git --work-tree=. commit -m "Add CV page and source workspace"
  ```

---

## Task 8: Run full build and visual/accessibility QA

**Files:**
- Read: all modified layouts, includes, Sass, content pages, and collection entries
- Generated: `_site/` (ignored; do not commit)

**Interfaces:**
- Consumes: Tasks 2–7.
- Produces: Verified static site ready for user review.

- [ ] **Step 1: Run the full build**

  ```bash
  bundle exec jekyll build --trace
  ```

  Expected: exit code 0 and generated pages for `/`, `/publications/`, `/teaching/`, and `/cv/`.

- [ ] **Step 2: Check generated routes and content**

  ```bash
  test -f _site/index.html
  test -f _site/publications/index.html
  test -f _site/teaching/index.html
  test -f _site/cv/index.html
  ! rg -n "Your Name|Academic Pages is a ready-to-fork|GitHub University|Paper Title Number|Teaching experience" _site
  ```

  Expected: all route checks pass and no template identity/content remains in generated output.

- [ ] **Step 3: Check source hygiene**

  ```bash
  git diff --check
  git --git-dir=.site-git --work-tree=. status --short
  ```

  Expected: no whitespace errors; only intentionally untracked `.superpowers/` preview files remain outside the committed implementation.

- [ ] **Step 4: Perform browser QA at desktop and mobile widths**

  Start the local Jekyll server and inspect the four routes at approximately 1024px, 736px, and 360px widths. Verify the warm ivory background, readable text, compact heading, sidebar rail, push-open behavior, and no content overlap.

- [ ] **Step 5: Perform keyboard interaction QA**

  Tab to the sidebar summary, open it with the keyboard, tab through Home/Publications/Teaching/CV and all confirmed academic links, and confirm visible focus styling. Confirm that the sidebar remains in normal flow on mobile.

- [ ] **Step 6: Review the final diff against the design spec**

  Confirm every acceptance criterion in `docs/superpowers/specs/2026-08-09-personal-academic-profile-design.md` is covered. If a missing user input prevents a criterion, stop and request that exact input rather than inventing content.

---

## Execution order and checkpoints

Run Tasks 1–3 first to establish real content, then Tasks 4–5 for the shared layout and visual system, then Tasks 6–7 for the content pages and CV area. Task 8 is the final verification gate. Each task ends with its own focused commit so a later issue can be isolated without rewriting unrelated content.
