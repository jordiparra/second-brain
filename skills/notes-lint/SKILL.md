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
3. Read `sessions-index.md`.
4. List all `.md` files in `wiki/` recursively.

## Checks

Run all checks and report findings.

### 1. Index completeness

Compare `wiki-index.md` against actual files in `wiki/`. Every file should be listed, every listing should have a corresponding file. Check all category subfolders including `wiki/notes/` and all project subfolders under `wiki/projects/`.

### 2. Frontmatter compliance

Validate every wiki entry's frontmatter against the schema in CONVENTIONS.md:
- `category` and `added` are required on every entry
- Category-specific properties match the corresponding template in `templates/`
- No unknown or deprecated properties
- **Wikilinks in entity properties:** Properties that reference entities (author, director, genre, city, maker, artist, cuisine, etc.) must use `[[wikilinks]]`, not plain strings. Check against templates — any property with `"[[]]"` as its template default expects a wikilink. Flag plain-string values like `author: "Liu Cixin"` (should be `author: "[[Liu Cixin]]"`) or `genre: [science fiction]` (should be `genre: ["[[Science Fiction]]"]`).

### 3. Orphan pages

Pages in `wiki/` with no inbound links from other pages, the index, or any list page. Every page should be reachable.

### 4. Missing pages and hub nodes

- **Missing pages** — `[[wikilinks]]` that point to pages that don't exist. Also check if frequently mentioned concepts deserve their own page.
- **Missing hub pages** — Check `wiki/hubs/` for sub-category nodes. Any genre, product type, author (3+ books), or director (3+ films) that appears in frontmatter across multiple entries should have a hub page in `wiki/hubs/`. Hub pages connect sub-categories to their parent hub in Graph View. Flag missing ones and create them — minimal format: `category` linking to parent + `# Title` heading.

### 5. Linking health

Assess the wiki's link density and find missed connections. **Linting is the place to add links** — ingest keeps backlinking cheap (parent + obvious siblings only), so broader backlink audits belong here. When you find a missing link that should clearly exist, add it directly; reserve questions for judgment calls where the connection is ambiguous.

- **Orphan entries** — pages with zero outgoing `[[wikilinks]]` to other wiki entries. Every substantive entry (books, articles, ideas, projects, trips) should have at least one meaningful outgoing link. Exceptions: minimal metadata entries (products, quotes) are OK without links.
- **Broken cross-references** — pages that reference each other's topics but don't link. Shared context, project, or tags that suggest a connection.
- **Series and clusters** — books in the same series that don't link to each other, multiple works by the same author without cross-references, book/film adaptation pairs that don't link.
- **Trips without place/restaurant links** — trip pages with empty or unlinked Places/Restaurants sections, or that mention venues by name without `[[wikilinks]]`.
- **DODs without parent links** — DOD pages that don't link back to their parent project page.
- **One-directional links** — page A links to page B but B doesn't link back, where a bidirectional link would be natural (e.g., project ↔ DOD, book ↔ sequel).
- **Stale file references** — prose mentions of renamed files (e.g., old path/filename in a note after a rename). Update to the current name so links don't break. **Exclude `log/` entries and existing `wiki-changelog.md` entries** — both are append-only historical records; stale filenames in them are accurate history, not errors.
- **Tag gaps** — tags are required on every entry in `wiki/[category]/` and `log/`. Hub pages in `wiki/hubs/` are exempt. Scan all entries and flag: tagless entries (outside hub pages), entries missing an obvious tag (e.g., a book about AI without `ai`), clusters of entries sharing a theme but no common tag (suggest a new one), and near-duplicate tags that should be consolidated (e.g., `bike` vs `cycling`). No minimum or maximum count — use as many facets as genuinely apply. Check existing vocabulary (`grep -rh "^tags:" wiki/ log/`) for consistency, but recommend new tags freely when existing ones don't fit. Session logs follow the same rules. See also the Tag audit section below for style enforcement and report-only signals.

### 6. Tag audit

Enforce the one tag style rule (kepano's: always pluralize; our corollary: lowercase kebab-case) and surface tag-quality signals for the user to review. **The guiding principle: lint reports, never auto-removes on judgment calls.** Only unambiguous rule violations auto-fix; anything requiring judgment is collected into the recap.

**Auto-fix (deterministic, safe — apply silently):**
- **Casing violations** — uppercase or mixed-case tags → lowercase (`UX` → `ux`, `ML` → `ml`). Valid chars remain: letters, digits, `_`, `-`, `/`.
- **Pluralization violations** — singular nouns that are clearly countable → plural (`tool` → `tools`, `keyboard` → `keyboards`, `idea` → `ideas`, `prototype` → `prototypes`). Skip ambiguous cases (mass nouns, acronyms, proper-noun-ish single-word tags) and report them instead.
- **Syntax drift** — wikilink-style tags (`[[something]]`), hash-prefixed (`#something`), tags with spaces, block-list YAML when the vault convention is inline — normalize to inline `tags: [a, b, c]` with plain lowercase kebab-case values.

**Report only (no auto-changes — collect into recap for the user):**
- **Singletons** — tags used exactly once in the vault. List with the entry where they appear. The user decides: promote (add to related entries), drop, or leave as a breadcrumb.
- **Persistent singletons** — tags that showed up as singletons in 2+ consecutive lint passes. Stronger nudge to resolve, still the user's call.
- **Cross-category promotion candidates** — tags that could plausibly apply to more entries based on content cues (e.g., a `research` tag on a DOD page but not on a related reference). Suggest where it could spread; don't apply.
- **Potential context-echo tags** — entries where `context:` and a tag carry the same value (e.g., `context: personal` + tag `personal`). List them; don't strip. The user decides whether the tag is redundant or load-bearing.
- **Ambiguous pluralization** — singular tags the auto-fixer skipped (mass nouns, acronyms like `ai`, or proper-noun-ish tags). Flag for the user.
- **Near-duplicates** — tag pairs that look like they mean the same thing (`bike` / `cycling`, `js` / `javascript`). Suggest a canonical form; don't merge.

**Why report-only:** Automatic pruning risks deleting signal before it matures. A tag used once today may be the exact connector for a future ingest. Same applies to unresolved wikilinks and sparse content. The user reviews the recap and decides what to promote, merge, or drop; lint only executes a batch after explicit approval.

### 7. Lists consistency

Every category subfolder in `wiki/` that has browsable entries should have a corresponding `.md` in `lists/` with an embedded base. Check that base filters match the category correctly. Verify all list pages reference categories that still exist.

### 8. Bookmarks consistency

Read `.obsidian/bookmarks.json`. Verify that:
- All `lists/` pages are in the Lists bookmark group
- All project pages in `wiki/projects/` are in the Projects bookmark group
- No bookmarks point to deleted or moved files

### 9. Convention compliance

Per CONVENTIONS.md:
- File naming: natural casing with spaces
- Wikilinks: `[[page-name]]` without directory paths
- Writing style: sentence case headings, concise, factual
- Templates: entries match their category's template structure

### 10. Sources integrity

Check `sources/` folder:
- Source files referenced from wiki entries actually exist
- Source files are not orphaned (have at least one wiki entry pointing to them)

### 11. Stale content

Flag pages with `updated` dates significantly older than related pages, or where content may have been superseded.

## Output

Group findings by severity:

- **Fix now** — broken links, index mismatches, frontmatter errors, missing list pages. Fix these directly.
- **Consider** — missing pages, weak cross-references, structural suggestions. Discuss with user.
- **Fine for now** — minor imperfections appropriate for the wiki's current size.

Fix straightforward issues directly. Ask about judgment calls.

## After fixes

Prepend to `wiki-changelog.md` at the top of the entry list (below the header and `---`), newest-first:

```
**YYYY-MM-DD** — Lint: summary of findings and fixes.
```

Blank line between entries. Never edit previous entries.
