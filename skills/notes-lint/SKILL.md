---
name: notes-lint
description: |
  This skill should be used to health-check the personal Notes wiki / knowledge base / second brain. It validates structure, indexes, frontmatter, links, and conventions. Triggers include "lint the wiki", "check wiki health", "audit the notes", "audit my knowledge base", "review wiki quality", "is my wiki in good shape?", "is my second brain in good shape?", "anything to fix in the wiki?", "clean up the notes", "fix my notes", "check the wiki for problems", or any request to find structural or content issues in the notes, wiki, or knowledge base.

  Examples:
  - "lint the wiki"
  - "is my wiki in good shape?"
  - "any issues in my notes?"
  - "audit my knowledge base"
  - "/notes-lint"
---

# Lint the Notes wiki

Health-check the personal notes. Structure is in the vault's `CLAUDE.md` (auto-loaded when the vault is the working directory). Authoring conventions (frontmatter schemas, linking rules, folder creation) are in `CONVENTIONS.md` at the vault root — read it first and validate against it, not against hardcoded rules in this skill.

## Before you start

1. Read the authoring conventions: `CONVENTIONS.md` (at the vault root).
2. Read `wiki-index.md`.
3. Read `log-index.md` (newest-first).
4. List all `.md` files in `wiki/` recursively.

## Checks

Run all checks and report findings.

### 1. Index completeness

Compare `wiki-index.md` against actual files in `wiki/`. Every file should be listed, every listing should have a corresponding file. Check all category subfolders including `wiki/notes/` and all project subfolders under `wiki/projects/`.

### 2. Frontmatter compliance

Validate every wiki entry's frontmatter against the schema in CONVENTIONS.md:
- `category` is required on every entry
- Category-specific properties match the corresponding template in `templates/`
- No unknown or deprecated properties
- **Wikilinks in entity properties:** Properties that reference entities (author, director, genre, city, maker, artist, cuisine, etc.) must use `[[wikilinks]]`, not plain strings. Check against templates — any property with `"[[]]"` as its template default expects a wikilink. Flag plain-string values like `author: "Liu Cixin"` (should be `author: "[[Liu Cixin]]"`) or `genre: [science fiction]` (should be `genre: ["[[Science Fiction]]"]`).

### 3. Orphan pages

Pages in `wiki/` with no inbound links from other pages, the index, or any list page. Every page should be reachable.

### 4. Missing pages and hub nodes

- **Missing pages** — `[[wikilinks]]` that point to pages that don't exist. Also check if frequently mentioned concepts deserve their own page.
- **Missing hub pages** — Check `wiki/categories/` for sub-category nodes. Any genre, product type, author (3+ books), or director (3+ films) that appears in frontmatter across multiple entries should have a hub page in `wiki/categories/`. Hub pages connect sub-categories to their parent hub in Graph View. Flag missing ones and create them — minimal format: `category` linking to parent + `# Title` heading.

### 5. Linking health

Assess the wiki's link density and find missed connections:

- **Orphan entries** — pages with zero outgoing `[[wikilinks]]` to other wiki entries. Every substantive entry (books, articles, ideas, projects, trips) should have at least one meaningful outgoing link. Exceptions: minimal metadata entries (products, quotes) are OK without links.
- **Broken cross-references** — pages that reference each other's topics but don't link. Shared context, area, or tags that suggest a connection.
- **Series and clusters** — books in the same series that don't link to each other, multiple works by the same author without cross-references, book/film adaptation pairs that don't link.
- **Trips without place/restaurant links** — trip pages with empty or unlinked Places/Restaurants sections, or that mention venues by name without `[[wikilinks]]`.
- **DODs without parent links** — DOD pages that don't link back to their parent project page.
- **One-directional links** — page A links to page B but B doesn't link back, where a bidirectional link would be natural (e.g., project ↔ DOD, book ↔ sequel).
- **Tag gaps** — tags are required on every entry in `wiki/[category]/` and `log/`. Hub pages in `wiki/categories/` are exempt. Scan all entries and flag: tagless entries (outside hub pages), entries missing an obvious tag (e.g., a book about AI without `ai`), clusters of entries sharing a theme but no common tag (suggest a new one), and near-duplicate tags that should be consolidated (e.g., `bike` vs `cycling`). No minimum or maximum count — use as many facets as genuinely apply. Also flag syntax drift: wikilink-style tags, uppercase or spaces (valid tag chars: letters, digits, `_`, `-`, `/`), or block-list YAML when the vault convention is inline. Check existing vocabulary (`grep -rh "^tags:" wiki/ log/`) for consistency, but recommend new tags freely when existing ones don't fit. Session logs follow the same rules.

### 6. Lists consistency

Every category subfolder in `wiki/` that has browsable entries should have a corresponding `.md` in `lists/` with an embedded base. Check that base filters match the category correctly. Verify all list pages reference categories that still exist.

### 7. Bookmarks consistency

Read `.obsidian/bookmarks.json`. Verify that:
- All `lists/` pages are in the Lists bookmark group
- All project pages in `wiki/projects/` are in the Projects bookmark group
- No bookmarks point to deleted or moved files

### 8. Convention compliance

Per CONVENTIONS.md:
- File naming: natural casing with spaces
- Wikilinks: `[[page-name]]` without directory paths
- Writing style: sentence case headings, concise, factual
- Templates: entries match their category's template structure

### 9. Sources integrity

Check `sources/` folder:
- Source files referenced from wiki entries actually exist
- Source files are not orphaned (have at least one wiki entry pointing to them)

### 10. Stale content

Flag pages with `updated` dates significantly older than related pages, or where content may have been superseded.

## Output

Group findings by severity:

- **Fix now** — broken links, index mismatches, frontmatter errors, missing list pages. Fix these directly.
- **Consider** — missing pages, weak cross-references, structural suggestions. Discuss with user.
- **Fine for now** — minor imperfections appropriate for the wiki's current size.

Fix straightforward issues directly. Ask about judgment calls.

## After fixes

Prepend to `wiki-changelog.md` (newest-first) **one cohesive paragraph covering the whole lint pass**:

```
**YYYY-MM-DD** — Fixed [summary of findings and fixes, enumerating affected files].
```

Use a capitalized verb leading the sentence. Enumerate affected files inside the paragraph rather than writing one entry per fix. Blank line between entries. Never edit existing entries.
