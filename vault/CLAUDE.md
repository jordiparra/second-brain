# Notes

Personal notes maintained collaboratively between the user and Claude. Rendered in Obsidian. One file per entry, properties for structure, Bases for browsing. The user captures, the AI classifies and routes.

CLAUDE.md covers structure and navigation; `CONVENTIONS.md` covers authoring conventions (frontmatter schemas, linking rules, placement, routing, templates). When the setup evolves, update the relevant file and any affected docs (README) and skills as part of the same change.

## Directory structure

```
vault/                        ← Obsidian vault root
├── lists/                    ← Browsing views (one .md per category)
│   ├── All Projects.md
│   ├── Books.md
│   ├── Prototypes.md
│   └── ...                   ← New .md when first entry of that category is created
├── templates/                ← One template per category
├── wiki/                     ← All entries
│   ├── [category]/           ← Standalone entries by category (books/, articles/, prototypes/, etc.)
│   ├── categories/           ← Hub pages for sub-categories (genres, product types, authors, directors)
│   ├── notes/                ← Living docs (AI workflow, setup, etc.)
│   └── projects/             ← Project overviews + project-scoped content
│       ├── [Project Name].md ← Project overview with embedded bases
│       └── [project-name]/   ← Project-specific entries by category
│           ├── dods/         ← Definitions of Done (one .md per DOD)
│           ├── references/   ← Project-specific references
│           └── prototypes/   ← Project-specific prototypes
├── sources/                  ← Raw source material (immutable archive)
│   ├── [category]/           ← Standalone sources by category
│   └── projects/[project]/   ← Project-specific sources
├── log/                      ← Session recaps (YYYY-MM-DD-HHMM.md)
├── wiki-index.md             ← Directory of all wiki entries by category (read this, don't glob)
├── wiki-changelog.md         ← Append-only record of wiki changes (newest-first)
├── log-index.md             ← Session directory with dates and summaries (newest-first)
```

## Navigating the notes

- **wiki-index.md** — The authoritative directory of all entries, organized by category. Start here for any lookup.
- **log-index.md** — Session log directory with dates and one-line summaries, newest-first. Check this for past decisions, discussions, or session history.
- **wiki-changelog.md** — Append-only record of all wiki changes, newest-first. Check this to see what changed and when.

## Entry basics

Every entry is a `.md` file with frontmatter properties:

- `category` — Required. A `[[wikilink]]` to the category name (e.g., `"[[Books]]"`, `"[[Movies]]"`).
- `context` — Optional. Structured taxonomy for which "world" the entry belongs to (e.g., `work`, `personal`, `ai`). Define your own small fixed set.
- `tags` — Optional. Lightweight facets for micro-clustering (e.g., `ai`, `music`, `design`).

Entries also have category-specific properties visible in frontmatter — `author`, `genre`, `director`, `city`, `cuisine`, `type`, etc. — defined in the matching template in `templates/`. These properties use `[[wikilinks]]` for entity references to power Graph View connections.

### File naming

Natural casing with spaces: `The Three-Body Problem.md`, `Ada Lovelace.md`. Keep names short but unambiguous. If names collide across types, disambiguate: `Dune (book).md`, `Dune (film).md`.

## Query workflow

The notes are the user's second brain. **When the user asks any personal question, check the notes before answering or saying "I don't know."** This includes questions that sound like calendar, booking, or preference lookups — the answer is often in an entry.

**Principle: notes for personal context, web for real-world facts, often both together.**

- **Notes** are the source of truth for the user's plans, opinions, history, and decisions.
- **Web** fills in live or factual details the notes can't store (schedules, hours, weather, prices, directions).
- **Combine** when a question starts personal but needs real-world data: e.g., "what time does dinner start in Berlin?" → notes for the trip/restaurant, web for opening hours.

**Lookup order:**
1. `wiki-index.md` — scan for relevant entries by type.
2. Read matching pages in `wiki/[category]/` (frontmatter + body).
3. `log-index.md` → recent session logs if the question is about past decisions or discussions.
4. If the notes fully answer the question, stop. Only search the web when the question needs real-world facts the notes can't store (live schedules, hours, weather, current prices) — and use notes context (destination, restaurant name, etc.) to make the search specific.
5. Only after checking: say the info isn't available.

**No automatic filing** — only file new content if the user asks.

## Safety

Never write secrets anywhere in the vault — API keys, tokens, passwords, recovery phrases, card numbers, or anything that would be a breach if the vault leaked. This applies to session logs and changelog entries too. Redact before archiving external sources. When referencing a secret, name its location (e.g., `key in .env`) or use `<REDACTED:type>`. See `CONVENTIONS.md` → Safety for the full rules.
