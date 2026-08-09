# Personal Academic Profile Website Design

## Scope

Redesign the existing Academic Pages/Jekyll site into a focused personal academic profile for Yipeng (Harry) Wang. The first version will prioritize clarity, easy reading, and straightforward maintenance over visual complexity.

The site is for academic readers, including researchers, collaborators, students, and visitors looking for publications, teaching information, or a CV.

## Confirmed identity and tone

- Name: Yipeng (Harry) Wang
- Role: Instructor at Princeton University
- Advising history: advised by Simon Brendle at Columbia
- Research: various aspects of differential geometry
- Voice: first person
- Tone: warm, restrained, scholarly, and direct
- Portrait: not included in the initial design

## Information architecture

The primary navigation contains four destinations:

1. Home
2. Publications
3. Teaching
4. CV

The expanded academic menu also provides direct links to:

- Email
- Princeton profile
- Google Scholar
- ORCID

### Home

The homepage opens with the user’s name, role, Princeton affiliation, and a concise first-person research introduction. It then provides clear links to publications and preprints, teaching, and the CV.

The homepage does not use a large slogan, portrait, hero image, dashboard cards, or decorative animation.

### Publications

Show a complete, readable list of publications and preprints. Entries are ordered newest first and use simple text rows containing the title, authors, venue or preprint information, year, and available links such as PDF, DOI, or arXiv.

Publications and preprints may be separated into labeled sections, but the initial version does not add filters, search, or complex cards.

### Teaching

Group courses by academic year and semester. Each course entry may include the course title, term, role, and relevant links or notes when available.

### CV

Provide a clear link to the current CV PDF. Future CV maintenance will be isolated in a `cv/` area containing the `.tex`, `.cls`, and generated PDF files. The supplied `cv_style.cls` and `CV_template.tex` are not copied or modified during this redesign unless requested later.

## Visual direction

Use the selected modern-and-warm direction with a simple academic presentation:

- Background: warm ivory
- Text: dark brown-black rather than pure black
- Accent: muted brick red for active navigation, links, and small labels
- Typography: a readable system font stack; no external font dependency is required
- Layout: generous whitespace, restrained line separators, and simple text lists
- Surfaces: avoid excessive cards, shadows, badges, and ornamental panels

The design should feel personal and approachable while remaining appropriate for a mathematics academic profile.

## Sidebar interaction

The profile navigation is a non-blocking floating sidebar:

- Resting state: a narrow `MENU · HW` rail sits at the edge of the content area.
- Desktop hover/focus state: the rail expands into a sidebar and the main content shifts right to make room.
- The sidebar never overlays or obscures the page content.
- The sidebar contains the user identity, primary navigation, and academic links.
- On small screens, the menu expands within normal page flow so content remains visible and readable.
- The interaction uses lightweight CSS transitions and accessible focus behavior. It does not require a large JavaScript bundle.

## Technical approach

Keep the existing Jekyll and GitHub Pages build pipeline. Reuse the existing collections and content conventions for publications, teaching, and pages. Customize only the configuration, page layouts, navigation, and Sass/CSS needed for the new design.

Do not introduce React, Vue, a new application framework, or additional large client-side dependencies. Existing Academic Pages scripts may remain where they are required by the theme, but the redesign should not add unnecessary script behavior.

Replace the template’s sample identity, sample publications, sample talks, sample teaching entries, and placeholder links with Harry’s real academic information. Content that is not part of the agreed initial profile should not be invented.

## Implementation inputs

Before implementation, collect the authoritative values for:

- Email address
- Princeton profile URL
- Google Scholar URL
- ORCID URL
- Current publication and preprint entries
- Teaching history grouped by year and semester
- Current CV PDF URL or repository file location

## Acceptance criteria

- A visitor can identify Harry, his Princeton role, and his research area immediately.
- Publications and preprints are visible as a complete, simple list.
- Teaching is organized by academic year and semester.
- The CV is easy to find and download.
- Academic links are available from the expanded navigation.
- No portrait is required.
- The sidebar expansion does not cover or obscure content.
- The layout works on desktop and mobile.
- The implementation adds no large framework or unnecessary JavaScript.
- Template sample content is no longer presented as Harry’s information.

## Deferred enhancements

After the baseline is complete and reviewed, consider optional enhancements based on actual use, such as a more detailed research page, publication metadata improvements, search, accessibility refinements, analytics, or improved CV automation. These are intentionally outside the first implementation pass.
