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

Process a source into the personal notes. **CONVENTIONS.md is the source of truth** for placement, frontmatter, linking, tags, indexes, and safety — read it first and follow it exactly. This skill defines the workflow (what to do, in what order); the rules live in CONVENTIONS.md.

## Before you start

The user's wiki location is in their global `~/.claude/CLAUDE.md` under "Knowledge base" (or similar). Resolve it from there. All file references below are relative to the wiki root.

1. Read `CONVENTIONS.md` at the wiki root
2. Read `wiki-index.md`
3. Identify the source. The user may provide:
   - A file path (in `sources/` or elsewhere)
   - A URL to fetch
   - Pasted text or a screenshot
   - A PDF to read

## Workflow

### 1. Read the source fully

Read or fetch the entire source. Understand the full content before proceeding.

- **Google Docs:** Always check for multiple tabs first (`list_document_tabs`). If the document has more than one tab, read each tab individually — don't assume all content is in the default tab.
- **Pasted snippets / screenshots:** if the input has no obvious home on disk yet, you'll create the source file in step 3.

### 2. Assess and route

Apply the **placement confidence check** from CONVENTIONS.md → Capture (0–10 scale):

- **7 or above:** Proceed directly — archive, create entries, update indexes, all in one shot. No confirmation needed.
- **Below 7:** Pause and ask the user where it should go or what to extract before writing anything.

When proceeding directly, briefly mention what you created at the end (entries, connections, placement) so the user can course-correct. Misfiles get caught by weekly lint — speed matters more than perfection.

### 3. Archive the raw source

Save the original to `sources/` per **CONVENTIONS.md → Source grouping rule** (three independent dimensions: placement, aggregation, grouping). Ingest-specific notes:

- `sources/` is immutable after capture — keep original wording intact; strip only HTML/nav/ads on web fetches.
- **4th-source migration:** if this is the 4th source for an entity currently flat in `sources/[category]/`, move the earlier 3 into a new `sources/[category]/[entity-slug]/` subfolder in the same ingest, prefix every file with the entity slug, and update the wiki entry's `source:` wikilinks.

### 4. Create wiki entries

Per **CONVENTIONS.md → Placement rule, Entry format, and Capture routing table:**

- Pick the category, subfolder, and matching `templates/` template.
- Fill frontmatter from the template. Use `[[wikilinks]]` for any property that references an entity (author, director, genre, city, project, etc.) — plain strings are invisible to Graph View.
- Link to the raw source in the body and via the `source:` frontmatter property.
- **Projects with DODs:** always create one `.md` per DOD in `wiki/projects/[project]/dods/`. Never use a DOD table on the project page as the only surface.

### 5. Cross-link

Linking happens at ingest time — don't defer it. Full rules in **CONVENTIONS.md → Linking**.

- **Outward:** add `[[wikilinks]]` on first mention of anything that is or could be its own page. Unresolved links are fine — they're breadcrumbs.
- **Inward (cheap cases only):** update direct parent/sibling pages that obviously need the new entry — project ↔ DOD, series neighbors, author pages. Don't scan the whole wiki; periodic lint handles broader backlinks.
- Skip trivial mentions. Only link where the connection is meaningful enough that you'd want to follow it.

### 6. Update indexes

Per **CONVENTIONS.md → Index maintenance:**

- Add new entries to `wiki-index.md` (alphabetical within category, update counts).
- Prepend one line to `wiki-changelog.md` at the top of the entry list (below the header and `---`). Budget ~120 chars, hard cap ~200. Lead with `Ingested`. See the `/wrap` skill for examples and drift smells.

## Hub and list pages

- **New category** (first entry of a category): create the matching `lists/` page with an embedded base, and add it to the Lists bookmark group in `.obsidian/bookmarks.json`.
- **New sub-category node** (genre, product type, prolific author, prolific director): if a hub doesn't exist in `wiki/hubs/`, create a minimal stub — just `category` linking to the parent hub plus a `# Title` heading. Full rules in CONVENTIONS.md → Folder descriptions.

## Guidelines

- Don't create entries for minor mentions. A person named once in passing doesn't need a page.
- Prefer updating existing pages over creating new ones when the content fits.
- Flag contradictions if the new source conflicts with existing wiki content.
- Keep entries focused and short per the wiki's style.
