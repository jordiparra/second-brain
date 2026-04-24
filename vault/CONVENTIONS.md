# Notes — Authoring conventions

This file + CLAUDE.md together are the source of truth for the notes. CLAUDE.md covers structure and navigation; this file covers authoring conventions. When the setup evolves — new types, new folder patterns, new conventions — update the relevant file and any affected docs (README, sortspec, templates) and skills (notes-ingest, notes-lint, notes-query, wrap) as part of the same change. Don't let the schema drift from reality.

## Placement rule

Every entry has a category. Where it lives depends on whether it's project-scoped:

- **Project-scoped** → `wiki/projects/[project]/[category]/` (e.g., `project-alpha/references/`, `project-beta/prototypes/`)
- **Standalone** → `wiki/[category]/` (e.g., `wiki/prototypes/`, `wiki/articles/`)

The same category can exist in both places. Project context wins over category — a prototype for Project Alpha goes under the project, not in `wiki/prototypes/`. Sources follow the same rule: `sources/projects/[project]/` vs `sources/[category]/`.

## Folder descriptions

**lists/** — Browsing views. `.md` files with embedded ```` ```base ```` code blocks. One per browsable category. When a new category appears, create the `.md` and add it to the Bookmarks Lists group.

**wiki/projects/** — Each project is a `.md` with frontmatter (category: project, context, status) and embedded bases. Project-specific entries live in category subfolders (`dods/`, `references/`, `prototypes/`, etc.). Create subfolders as needed per project. Entries under a project reference it via the `project:` wikilink property.

**wiki/hubs/** — Hub pages for sub-category nodes: genres, product types, prolific authors (3+ books), prolific directors (3+ films). Minimal pages — just `category` linking to the parent hub (when applicable) and a heading. These create Graph View hierarchy: e.g., Products → Keyboards → individual keyboards. Genre pages have no `category` — they bridge across media types (Science Fiction connects both sci-fi books and movies). Create new hub pages when a sub-category value appears on 3+ entries.

**wiki/notes/** — Living documents about AI workflows, setup, and meta-wiki topics. Each has `category: note, context: ai` in frontmatter.

**sources/** — Raw source material archive. Immutable — never edited after capture. For web-fetched content: save clean prose with the original wording intact — strip HTML, navigation, ads, and formatting noise, but preserve the author's sentences and structure. Include the source URL at the top. Never summarize or rephrase; the wiki entry is the synthesis, the source is the record.

**log/** — Session recaps. Each session gets a `YYYY-MM-DD-HHMM.md` file with frontmatter (`category: "[[Session Logs]]"`, `tags`) and curated highlights: main topics, learnings, mistakes to avoid, decisions made. Not a transcript — only wiki-worthy content. Tags and wikilinks in body text connect logs to the Graph View mesh.

**wiki-index.md** — Auto-maintained by Claude. Update whenever entries are added or removed.

**sessions-index.md** — One paragraph per session, newest-first. Read this first to find relevant past sessions.

**wiki-changelog.md** — Append-only. One paragraph per change event, newest at top. Never edit existing entries, only prepend new ones.

**Bookmarks** — Obsidian Bookmarks (`.obsidian/bookmarks.json`) provide sidebar shortcuts grouped as AI, Lists, and Projects. Claude maintains this file.

**To review/** — Unsorted items. Process periodically.

## Entry format

Every entry in wiki/ is a `.md` file with frontmatter properties:

**Universal properties:**
- `category` — Required. A `[[wikilink]]` to the category name (e.g., `category: "[[Books]]"`, `category: "[[Movies]]"`, `category: "[[DODs]]"`). Creates a graph connection to the category hub node. Bases filters match on the display name: `category == "Books"`. See templates for the exact value per category.
- `added` — Required. Date the entry was added to this wiki. Format: `YYYY-MM-DD`.

**Grouping properties:**
- `context` — Structured taxonomy. Define a small fixed set for your life (e.g., `work`, `personal`, `ai`, or whatever "worlds" apply). Powers Bases views for filtering by world. Only add when the entry clearly belongs to one. Not needed for inherently personal types (books, movies, restaurants) or entries that don't fit a single world. Usually a single value; can be a list when an entry genuinely spans two worlds — e.g., a work trip extended with leisure days: `context: [work, personal]`.
- `project` — Wikilink (or wikilink list), optional. Points to project page(s) under `wiki/projects/`. Example: `project: "[[Project Name]]"`, or a list: `project: ["[[Project Alpha]]", "[[Project Beta]]"]`. Powers project bases and creates Graph View edges to project hubs. Use wikilinks (not plain strings) so the connection is visible in Graph View.
- `tags` — Required on every entry in a content folder (`wiki/[category]/`, `log/`). Hub pages in `wiki/hubs/` are exempt — they stay minimal by design. Think of tags like Twitter hashtags: each is a lightweight facet, and the combination creates specificity. Use as many as genuinely apply, no more — don't pad to hit a number, don't trim a real facet to stay under one. Cover whichever are relevant: domain (`work`, `personal`), type/instrument, geography, topic (`travel`, `ai`, `research`), strategy, tools, cross-cutting themes. For session logs, typical facets are domain, project area, activity (`ingest`, `lint`, `bugs`), tools, and topic. **Style (kepano's one rule):** tags are **lowercase kebab-case** and **always plural** (`tools`, not `tool`; `keyboards`, not `keyboard`; `ideas`, not `idea`). Syntax: plain inline YAML list — `tags: [ai, research, prototypes]`. No wikilinks, no hashtags, no spaces. For people, prefer `[[wikilinks]]` in body text; a first-name tag (e.g., `ana`) can coexist with a `[[Ana Lovelace]]` wikilink when it gives one-click cluster access to many entries. Obsidian renders tags as real tag nodes in Graph View when the "Tags" filter is enabled. Check existing vocabulary before adding new terms (`grep -rh "^tags:" wiki/ log/`), but new tags are fine — the vocabulary grows organically. Lint auto-fixes casing and pluralization drift and reports singletons for review.

**Category-specific properties** — Defined in each template in `templates/`. Use the matching template when creating new entries.

**Trip-specific properties** — Trips carry two properties beyond the universal set:
- `context` — `work`, `personal`, or both (`[work, personal]`) for work trips extended with leisure days.
- `travelers` — Wikilist of everyone who physically went. Always list every traveler, including the user on their own trips. Trips the user booked but didn't go on (e.g., flying a family member somewhere) omit them — that's how the property stays honest. Use `[[wikilinks]]` so each trip connects to the traveler's people page in Graph View. Unresolved wikilinks are fine for people who don't have a page yet.

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
| Software, a CLI tool, library, or service | software | `wiki/software/` — use tags to distinguish subtype: `app` (UI-based), `cli` (command-line), `library` (imported as dependency), `service` (hosted/cloud), `script` (standalone runnable) |
| A stock or investment | stock | `wiki/stocks/` |
| An idea for improving your AI workflow | idea (context: ai) | `wiki/ideas/` |
| An idea for a work project | idea (context: work) | `wiki/ideas/` |
| A decision | — | `## Decisions` section on the relevant DOD or project page |
| A prototype, tool, or built artifact | prototype | Per placement rule |
| An article, blog post, or external content | article | `wiki/articles/` |
| A social media post (tweet, IG, LinkedIn, Reddit thread) | — (artifact, not entity) | Archive raw to `sources/<attached-entity-folder>/` — e.g., a tweet digesting a book goes under `sources/books/`. Absorb the useful content into the attached entry's page and link the raw source. Do **not** create a standalone wiki entry unless the post is a long-form thread-essay that genuinely stands alone (then file as an article with a `tweets` tag or similar). Rationale: social posts are evidence, not durable nodes — the entity is what the post is about. |
| An actionable task | — | A task app (e.g., Things, Todoist), not the wiki |
| Don't know where it goes | — | `To review/` |

When a new entry category doesn't fit existing categories, create a new wiki/ subfolder and template.

**Brainstorming → tasks:** When actionable items emerge during wiki brainstorming, push them to your task app — don't leave them as wiki text.

**Placeholder entries:** Some categories have placeholder entries (e.g., "Example Place", "Example Restaurant"). When adding the first real entry to a category, delete the placeholder file and remove it from `wiki-index.md`.

### Project ingest

When the user provides a work document, project source, or external content in a project context:

1. **Save the raw source** to `sources/projects/[project]/` (immutable archive). For standalone content, save to `sources/[category]/`.
2. **Create wiki entry** in the appropriate category subfolder per the placement rule, with frontmatter (category, context, `project:` wikilink to project page, `source:` wikilink to raw).
3. **Add body sections**: content + "See also" for related topics + "Sources" for raw source links.
4. **Link from the project page** — add a `[[wikilink]]` in the project's .md.
5. If it's a **new project**, create `wiki/projects/[name].md` with frontmatter (category: project, context, status) and embedded bases, and add it to the Bookmarks Projects group.
6. If the project has **DODs**, create individual DOD pages in `wiki/projects/[project]/dods/` — one `.md` per DOD with frontmatter (`category: dod`, `context`, `project` wikilink to the project page, `dod` number, `team`). The project page uses an embedded base to surface them. Never list DODs only as a table — individual pages allow notes, design requirements, and decisions per DOD.

### Inline content on projects and DODs

Some content belongs inline on a project or DOD page rather than as its own file — decisions, open questions, design requirements, research notes, anything that isn't a standalone entity.

**Scope rule:**
- **DOD-scoped** (applies to one DOD: its requirements, design notes, decisions about it, notes on references used for it) → the DOD page in `wiki/projects/[project]/dods/`.
- **Project-scoped** (spans multiple DODs or applies to the project as a whole: goals, strategy, cross-cutting decisions) → the project page in `wiki/projects/`.

**Quick test when in doubt:** "If this DOD gets cut, does the content still make sense?" Yes → project page. No → DOD page.

Standalone entities (a reference, a prototype, an idea) get their own file under `wiki/projects/[project]/[category]/` per the placement rule — not inline.

#### Decisions

Decisions are always inline — never standalone files. When the user shares a decision:

1. **Find the right page** per the scope rule above.
2. **Add a `## Decisions` section** (if not already present) after the description.
3. **Add the decision** as a bullet: bold label with date, then concise description. Link to source material (`[[wikilink]]` to sources/ or external URLs) when available.

### Session log

Run `/wrap` to end a session. The skill writes the log file, updates `sessions-index.md`, `wiki-changelog.md`, and `wiki-index.md`, then prompts to `/clear`.

### Index maintenance

Both logs are append-only with **newest at the top**. New entries are prepended; existing entries are never edited.

- **wiki-index.md** — Update whenever entries are added or removed. Maintain alphabetical order within each category section. Update counts.
- **wiki-changelog.md** — Prepend one paragraph per **cohesive change thread** at the top of the entry list (below the header and `---` separator). Format: `**YYYY-MM-DD** — [Capitalized verb] [what changed and why, with wikilinks and paths as needed].` Blank line between entries. Lead with an action verb (`Added`, `Updated`, `Ingested`, `Fixed`, `Renamed`, `Created`, etc.) — no `action:` prefix.
  - **One entry per cohesive change, not per edit.** A single thread of work that touches many files across several tool calls is one entry — enumerate the affected files inside the paragraph. Unrelated changes in the same session get separate entries. Follow-on fixes directly caused by an earlier change in the same session fold into that change's entry rather than spawning their own.
  - **Append-only, never consolidate retroactively.** Once an entry is written, don't edit it to merge in later work — prepend a new entry instead. The "one entry per thread" rule applies at write time, not as a cleanup step.
- **sessions-index.md** — Prepend one paragraph per session log to the top of the entry list (below the header and `---` separator). Format: `**[[YYYY-MM-DD-HHMM]]** — One-line summary.` Blank line between entries.

## Safety

The vault is committed to git and ingests external material (screenshots, docs, PDFs, pasted content) that may carry credentials the sender didn't flag. **Treat the entire vault as not safe for secrets** — `wiki/`, `sources/`, `log/`, and the root-level index files (`wiki-index.md`, `wiki-changelog.md`, `sessions-index.md`) are all committed.

**Never write anywhere in the vault:**
- API keys, tokens, OAuth secrets, session cookies, bearer tokens
- Passwords, passphrases, recovery phrases, private keys, certificates
- Full credit card or bank account numbers
- Anything that would be a breach if the vault leaked

This applies to session logs and changelog entries too — when recapping a session that touched credentials, describe the action without echoing the value.

**When ingesting a source that contains such data:**
- **Redact before archiving.** `sources/` is immutable after capture — don't archive raw material with live credentials in it. Crop screenshots, blank cells in PDFs, strip token lines from pasted docs.
- **Reference by location, not value.** In wiki pages and logs, write `key in .env` or `token in 1Password` — don't echo the actual value. This is the preferred form whenever the secret has a known home.
- **Extract, don't archive whole.** If a mixed-content source (e.g., a screenshot with both a booking code and a card number) has useful info alongside secrets, extract just the useful text into a new file and redact or discard the original.

**Redaction syntax:**
- `<REDACTED>` or typed `<REDACTED:type>` (e.g. `<REDACTED:api-key>`, `<REDACTED:jwt>`, `<REDACTED:card>****1234`) when quoting output that contained a value.
- `<YOUR_API_KEY>` / `<YOUR_TOKEN>` only in example code where a reader substitutes their own value — never to hide a real one.

**When unsure:** ask before archiving. A minute of friction beats a credential in git history.

## Obsidian settings

- **Wikilinks:** Enabled (shortest path).
- **Excluded files:** `CLAUDE.md`, `CONVENTIONS.md`, `README.md`, `sortspec.md`, `wiki-index.md`, `wiki-changelog.md`, `sessions-index.md` — hidden from sidebar but accessible on disk.
- **Templates folder:** `templates/`
- **Attachments for projects:** inside each project's subfolder.
