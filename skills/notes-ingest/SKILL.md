---
name: notes-ingest
description: |
  This skill should be used to ingest, capture, or save a source into the personal Notes wiki / knowledge base / second brain. Triggers include "ingest this", "add this to the wiki", "add this to my notes", "add this to my knowledge base", "process this article", "save this to the wiki", "capture this", "file this", "note this down", "save this link", or when a source (article, clipping, screenshot, PDF, URL, pasted content) is provided for processing into wiki pages — even without an explicit command. If the user shares an article, link, or document in the context of their notes or wiki, this skill should activate.

  Examples:
  - "ingest this article about design tokens"
  - "add this to the wiki" (with a file path or pasted content)
  - "save this to my knowledge base"
  - "here's an article I found" (implicit ingest — user provides a source without a command)
  - "/notes-ingest sources/articles/some-article.md"
---

# Ingest a source into the Notes wiki

Process a source into the personal notes. Structure is in the vault's `CLAUDE.md` (auto-loaded when the vault is the working directory). Authoring conventions (naming, categories, frontmatter, linking) are in `CONVENTIONS.md` at the vault root — read it first and follow it exactly. Don't hardcode folder paths or entry categories.

## Before you start

1. Read the authoring conventions: `CONVENTIONS.md` (at the vault root).
2. Read `wiki-index.md`.
3. Identify the source. The user may provide:
   - A file path (in `sources/` or elsewhere)
   - A URL to fetch
   - Pasted text or a screenshot
   - A PDF to read

## Workflow

### 1. Read the source fully

Read or fetch the entire source. Understand the full content before proceeding.

- **Google Docs:** Always check for multiple tabs first (`list_document_tabs`). If the document has more than one tab, read each tab individually — don't assume all content is in the default tab.

### 2. Assess and route

Apply the placement confidence check from CONVENTIONS.md (0–10 scale):

- **7 or above:** Proceed directly — archive, create entries, update indexes, all in one shot. No confirmation needed.
- **Below 7:** Pause and ask the user where it should go or what to extract before writing anything.

When proceeding directly, briefly mention what you created at the end (entries, connections, placement) so the user can course-correct if needed. Misfiles get caught by weekly lint — speed matters more than perfection.

### 3. Archive the raw source

Save the original source to `sources/` following the structure in CONVENTIONS.md:
- Determine if it belongs to a project or stands alone
- Project-scoped sources go under `sources/projects/[project]/`
- Standalone sources go under `sources/[category]/` (e.g., `sources/articles/`)
- Keep the original content intact — `sources/` is immutable after filing
- **Scan for secrets before archiving.** If the source contains API keys, tokens, passwords, recovery phrases, card numbers, or anything that would be a breach if the vault leaked: redact before archiving (crop screenshots, blank cells in PDFs, strip credential lines), or extract just the useful text into a new file and discard the original. Reference secrets by location (`key in .env`, `token in 1Password`) or with `<REDACTED:type>` — never by value. See `CONVENTIONS.md` → Safety.

### 4. Create wiki entries

Based on the discussion, create or update entries in `wiki/`:

- Determine the correct category and subfolder from CONVENTIONS.md's routing rules
- Use the matching template from `templates/`
- Set frontmatter properties per the schema. `category` and `tags` are required on every entry in a content folder (`wiki/[category]/`). Hub pages in `wiki/categories/` are exempt from tags — they stay minimal by design. `context` and `area` are optional and only added when they clearly apply.
- **Wikilinks in frontmatter:** Any property referencing an entity (author, director, genre, city, maker, etc.) must use `[[wikilinks]]` — e.g., `author: "[[Liu Cixin]]"`, `genre: ["[[Science Fiction]]"]`, `city: "[[Barcelona]]"`. Plain strings are invisible to Graph View. Check the template for which properties take wikilinks.
- **Thematic tags:** Think of tags like Twitter hashtags — each tag is a lightweight facet, and the combination creates specificity. Use as many as genuinely apply, no more. A quote with four natural facets gets four; a fund with seven gets seven. Don't pad to hit a number, don't trim a real facet to stay under one. Cover whichever of these are relevant: domain (`work`, `personal`), topic (`travel`, `ai`, `music`), type/instrument, geography, strategy, theme, cross-cutting connections. Syntax: plain lowercase kebab-case in an inline YAML list — `tags: [ai, prototype, research]`. No wikilinks, no hashtags, no spaces (Obsidian renders these as real tag nodes via the Graph View "Tags" filter). Check existing vocabulary (`grep -rh "^tags:" wiki/ log/`) to reuse terms, but create new tags freely when existing ones don't fit — the vocabulary grows organically.
- Project-scoped entries go under `wiki/projects/[project]/[category]/` per CONVENTIONS.md (e.g., `references/`, `prototypes/`, `dods/`)
- Link to the raw source in the body and via `source:` frontmatter field if applicable
- **Projects with DODs:** Always create individual DOD pages in `[project]/dods/` — one `.md` per DOD with an embedded base on the project page. Never use only a table on the project page — individual pages allow notes, design requirements, and decisions per DOD.

### 5. Cross-link

Linking happens at ingest time — don't defer it. For every entry created or updated:

1. **Link outward** — scan the entry for mentions of things that are or could be wiki pages (people, places, books, projects, concepts, other entries of the same category). Add `[[wikilinks]]` on first mention. Unresolved links (pages that don't exist yet) are fine — they're breadcrumbs.
2. **Link inward (cheap cases only)** — update direct parent/sibling pages that obviously need to know about the new entry. Examples: a project page linking to its new DOD, a series neighbor linking to the new book, an author page listing the new title. Don't scan the whole wiki for looser connections — periodic lint handles the broader backlink audit.
3. **Don't force connections** — only link where the connection is meaningful enough that you'd want to follow it. A person named once in passing doesn't need a link.

### 6. Update the index and log

- Add new entries to `wiki-index.md` in the correct category section, maintaining alphabetical order and updating counts
- Prepend to `wiki-changelog.md` (newest-first) **one cohesive paragraph per ingest** covering the entire operation: `**YYYY-MM-DD** — Ingested [source title] into [pages created/updated], because [reason].` Use a capitalized verb (Ingested, Updated, Added, Created) leading the sentence. Enumerate affected files inside the paragraph rather than writing one entry per file. Never edit existing entries.

### 7. Create hub pages if needed

- **New category:** If this is the first entry of a new category, create a `.md` in `lists/` with an embedded base filtering for that category. Add it to the Lists bookmark group in `.obsidian/bookmarks.json`.
- **New sub-category nodes:** If the entry introduces a new genre, product type, author (3+ books), director (3+ films), or other frontmatter value that multiple entries share, check if a hub page exists in `wiki/categories/`. If not, create one — minimal format: just `category` linking to the parent hub and a `# Title` heading. This connects the sub-category to the parent in Graph View. Examples: a new genre → `wiki/categories/Genre Name.md`, a new product type → `wiki/categories/Type Name.md` with `category: "[[Products]]"`.

## Guidelines

- Don't create entries for minor mentions. A person named once in passing doesn't need a page.
- Check existing tags for consistency, but create new ones freely when needed — the vocabulary grows organically.
- Flag contradictions if the new source conflicts with existing wiki content.
- Prefer updating existing pages over creating new ones when the content fits.
- Keep entries focused and short per the wiki's style.
