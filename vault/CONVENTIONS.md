# Notes — Authoring conventions

This file + CLAUDE.md together are the source of truth for the notes. CLAUDE.md covers structure and navigation; this file covers authoring conventions. When the setup evolves — new types, new folder patterns, new conventions — update the relevant file and any affected docs (README, sortspec, templates) and skills (notes-ingest, notes-lint, notes-query, wrap) as part of the same change. Don't let the schema drift from reality.

## Placement rule

Every entry has a category. Where it lives depends on whether it's project-scoped:

- **Project-scoped** → `wiki/projects/[project]/[category]/` (e.g., `project-alpha/references/`, `project-beta/prototypes/`)
- **Standalone** → `wiki/[category]/` (e.g., `wiki/prototypes/`, `wiki/articles/`)

The same category can exist in both places. Project context wins over category — a prototype for Project Alpha goes under the project, not in `wiki/prototypes/`. Sources follow the same rule: `sources/projects/[project]/` vs `sources/[category]/`.

## Folder descriptions

**lists/** — Browsing views. `.md` files with embedded ```` ```base ```` code blocks. One per browsable category. When a new category appears, create the `.md` and add it to the Bookmarks Lists group.

**wiki/projects/** — Each project is a `.md` with frontmatter (category: project, context, area, status) and embedded bases. Project-specific entries live in category subfolders (`dods/`, `references/`, `prototypes/`, etc.). Create subfolders as needed per project.

**wiki/categories/** — Hub pages for sub-category nodes: genres, product types, prolific authors (3+ books), prolific directors (3+ films). Minimal pages — just `category` linking to the parent hub and a heading. These create Graph View hierarchy: e.g., Products → Keyboards → individual keyboards. Genre pages have no `category` — they bridge across media types (Science Fiction connects both sci-fi books and movies). Create new hub pages when a sub-category value appears on 3+ entries.

**wiki/notes/** — Living documents about AI workflows, setup, and meta-wiki topics. Each has `category: note, context: ai` in frontmatter.

**sources/** — Raw source material archive. Immutable — never edited after capture.

**log/** — Session recaps. Each session gets a `YYYY-MM-DD-HHMM.md` file with frontmatter (`category: "[[Session Logs]]"`, `tags`) and curated highlights: main topics, learnings, mistakes to avoid, decisions made. Not a transcript — only wiki-worthy content. Tags and wikilinks in body text connect logs to the Graph View mesh.

**wiki-index.md** — Auto-maintained by Claude. Update whenever entries are added or removed.

**log-index.md** — One line per session. Read this first to find relevant past sessions.

**wiki-log.md** — Append-only. One line per change event. Never edit existing entries, only append.

**Bookmarks** — Obsidian Bookmarks (`.obsidian/bookmarks.json`) provide sidebar shortcuts grouped as AI, Lists, and Projects. Claude maintains this file.

**To review/** — Unsorted items. Process periodically.

## Entry format

Every entry in wiki/ is a `.md` file with frontmatter properties:

**Universal properties:**
- `category` — Required. A `[[wikilink]]` to the category name (e.g., `category: "[[Books]]"`, `category: "[[Movies]]"`, `category: "[[DODs]]"`). Creates a graph connection to the category hub node. Bases filters match on the display name: `category == "Books"`. See templates for the exact value per category.

**Grouping properties:**
- `context` — Structured taxonomy. Define a small fixed set for your life (e.g., `work`, `personal`, `ai`, or whatever "worlds" apply). Powers Bases views for filtering by world. Only add when the entry clearly belongs to one. Not needed for inherently personal types (books, movies, restaurants) or entries that don't fit a single world.
- `area` — More specific grouping within a context (e.g., a project codename, a hobby name). Used by project bases.
- `tags` — Required on every entry in a content folder (`wiki/[category]/`, `log/`). Hub pages in `wiki/categories/` are exempt — they stay minimal by design. Think of tags like Twitter hashtags: each is a lightweight facet, and the combination creates specificity. Use as many as genuinely apply, no more — don't pad to hit a number, don't trim a real facet to stay under one. Cover whichever are relevant: domain (`work`, `personal`), type/instrument, geography, topic (`cooking`, `ai`, `travel`), strategy, tools, cross-cutting themes. For session logs, typical facets are domain, project area, activity (`ingest`, `lint`, `bugs`), tools, and topic. Syntax: plain lowercase kebab-case in an inline YAML list — `tags: [ai, prototype, research]`. No wikilinks, no hashtags, no spaces. Obsidian renders these as real tag nodes in Graph View when the "Tags" filter is enabled. Check existing vocabulary before adding new terms (`grep -rh "^tags:" wiki/ log/`), but new tags are fine — the vocabulary grows organically.

**Category-specific properties** — Defined in each template in `templates/`. Use the matching template when creating new entries.

**Wikilinks in frontmatter:** Any property whose value references an entity that could be its own page — people, places, genres, makers, shows — must use `[[wikilinks]]`. Obsidian treats frontmatter wikilinks as real graph connections. This is how entries connect to shared nodes (genres, authors, directors, cities) without needing body text.

Examples: `author: "[[Liu Cixin]]"`, `genre: ["[[Science Fiction]]", "[[Thriller]]"]`, `director: "[[Denis Villeneuve]]"`, `city: "[[Barcelona]]"`, `destination: "[[Berlin]]"`. Plain strings (`author: "Liu Cixin"`) are invisible to Graph View — always use wikilinks for entity references.

### Linking

Links are the wiki's primary connective tissue — they power Graph View, backlinks, and serendipitous rediscovery. Link profusely at write time, not as a separate maintenance step.

**Rules:**
- Use Obsidian `[[wikilinks]]` for all internal links.
- Use `[[page-name]]` without directory paths — Obsidian resolves vault-wide.
- **Link the first mention** of anything that is or could be its own page — people, places, books, projects, concepts, other entries of the same category (e.g., series neighbors, same-author works).
- **Unresolved links are fine.** `[[Restaurant Name]]` for a page that doesn't exist yet is a useful breadcrumb, not an error. It shows up in Graph View and signals a future page.
- Don't over-link: skip trivial mentions (a person named once in passing, a generic concept). Link where the connection is meaningful enough that you'd want to follow it.

**What to link by type:**
- **Books** → other books in the same series (previous/next), other works by the same author if they exist in the wiki, film adaptations.
- **Movies / TV** → source material (book, article), other entries by the same director if notable.
- **Trips** → `[[place]]` destinations, `[[restaurant]]` entries, linked people if traveling with someone with a wiki page.
- **Projects / DODs** → parent project (`[[Project Name]]`), related DODs, referenced prototypes, people.
- **Articles / Ideas** → related wiki concepts, projects, or notes they connect to.
- **Products** → related products of the same type when the connection is useful (e.g., keyboard switches → keyboard frames).

**When creating or updating an entry**, always check: are there existing wiki pages this should link to? For direct parent/sibling pages (project pages, series neighbors, author pages), update them to link back. Don't scan the whole wiki for looser connections at write time — periodic lint catches broader missing backlinks.

### Writing style

- Clear, concise, factual. Not conversational.
- Sentence case for headings.
- Lists and short entries are the default. Prose only when ideas genuinely flow together (project overviews, synthesis).

## Workflows

### Capture

When the user shares a thought, note, or quick idea: determine the category, context, and destination using the routing rules below. No taxonomy decisions from the user.

**Placement confidence check:** Before placing any content, self-assess confidence on a 0–10 scale (10 = certain, 0 = no idea). If confidence is **below 7**, ask the user where it should go instead of guessing. When the user corrects or confirms a placement, treat it as a learning signal for future routing.

**Routing rules:**

| Input | Category | Destination |
|-------|----------|-------------|
| A person | person | `wiki/people/` |
| A book | book | `wiki/books/` |
| A movie | movie | `wiki/movies/` |
| A TV show or series | tv-show | `wiki/tv-shows/` |
| A restaurant | restaurant | `wiki/restaurants/` |
| A place to visit | place | `wiki/places/` |
| A trip (planned or taken) | trip | `wiki/trips/` — link to `[[place]]`, restaurants, POIs |
| A product, gadget, or gear item | product | `wiki/products/` (one file per item, with type) |
| A stock or investment | stock | `wiki/stocks/` |
| An idea for improving your AI workflow | idea (context: ai) | `wiki/ideas/` |
| An idea for a work project | idea (context: work) | `wiki/ideas/` |
| A decision | — | `## Decisions` section on the relevant DOD or project page |
| A prototype, tool, or built artifact | prototype | Per placement rule |
| An article, blog post, or external content | article | `wiki/articles/` |
| An actionable task | — | A task app (e.g., Things, Todoist), not the wiki |
| Don't know where it goes | — | `To review/` |

When a new entry category doesn't fit existing categories, create a new wiki/ subfolder and template.

**Brainstorming → tasks:** When actionable items emerge during wiki brainstorming, push them to your task app — don't leave them as wiki text.

**Placeholder entries:** Some categories have placeholder entries (e.g., "Example Place", "Example Restaurant"). When adding the first real entry to a category, delete the placeholder file and remove it from `wiki-index.md`.

### Project ingest

When the user provides a work document, project source, or external content in a project context:

1. **Save the raw source** to `sources/projects/[project]/` (immutable archive). For standalone content, save to `sources/[category]/`.
2. **Create wiki entry** in the appropriate category subfolder per the placement rule, with frontmatter (category, context, area, `source:` wikilink to raw).
3. **Add body sections**: content + "See also" for related topics + "Sources" for raw source links.
4. **Link from the project page** — add a `[[wikilink]]` in the project's .md.
5. If it's a **new project**, create `wiki/projects/[name].md` with frontmatter (category: project, context, area, status) and embedded bases, and add it to the Bookmarks Projects group.
6. If the project has **DODs**, create individual DOD pages in `wiki/projects/[project]/dods/` — one `.md` per DOD with frontmatter (`category: dod`, `context`, `area`, `dod` number, `team`). The project page uses an embedded base to surface them. Never list DODs only as a table — individual pages allow notes, design requirements, and decisions per DOD.

### Decisions

Decisions are always inline — never standalone files. When the user shares a decision:

1. **Find the right page** — the DOD page in `wiki/projects/[project]/dods/` if it's DOD-scoped, or the project page in `wiki/projects/` if it's a project-level decision.
2. **Add a `## Decisions` section** (if not already present) after the description.
3. **Add the decision** as a bullet: bold label with date, then concise description. Link to source material (`[[wikilink]]` to sources/ or external URLs) when available.

### Session log

Run `/wrap` to end a session. The skill writes the log file, updates `log-index.md`, `wiki-log.md`, and `wiki-index.md`, then prompts to `/clear`.

### Index maintenance

- **wiki-index.md** — Update whenever entries are added or removed. Maintain alphabetical order within each category section. Update counts.
- **wiki-log.md** — Append one line per change event: `- YYYY-MM-DD — action: description`. Never edit existing entries.
- **log-index.md** — Append one row per session log.

## Obsidian settings

- **Wikilinks:** Enabled (shortest path).
- **Excluded files:** `CLAUDE.md`, `README.md`, `sortspec.md`, `wiki-index.md`, `wiki-log.md`, `log-index.md` — hidden from sidebar but accessible on disk.
- **Templates folder:** `templates/`
- **Attachments for projects:** inside each project's subfolder.
