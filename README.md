# second-brain

A tailored approach to a personal knowledge wiki: Obsidian for structure, Claude Code for capture and query. Inspired by kepano, Karpathy, and Nate B Jones. No content included — just the scaffolding, conventions, templates, and skills.

Assumes you already use [Claude Code](https://claude.com/claude-code) and [Obsidian](https://obsidian.md/).

## What's in the box

```
second-brain/
├── vault/                  ← The Obsidian vault (open this in Obsidian)
│   ├── CLAUDE.md           ← Structure and navigation (auto-loaded by Claude Code)
│   ├── CONVENTIONS.md      ← Authoring rules (frontmatter, linking, placement, routing)
│   ├── lists/              ← 16 Base views for browsing (Books, Movies, Trips, …)
│   ├── templates/          ← 32 entry templates (one per category)
│   ├── wiki/               ← Empty — entries go here, grouped by category or project
│   ├── sources/            ← Empty — raw source archive (immutable after capture)
│   ├── log/                ← Empty — session recaps written by the /wrap skill
│   ├── wiki-index.md       ← Directory of all entries, grouped by category
│   ├── wiki-changelog.md   ← Append-only record of wiki changes (newest-first)
│   ├── log-index.md        ← Directory of session logs with dates and summaries (newest-first)
│   └── .obsidian/          ← Obsidian settings: Bases, Bookmarks, plugins
└── skills/                 ← 4 Claude Code skills
    ├── notes-ingest/       ← Capture sources into wiki entries
    ├── notes-query/        ← Answer questions from the wiki
    ├── notes-lint/         ← Audit structure, links, frontmatter, tags
    └── wrap/               ← End-of-session recap + /clear prompt
```

## How it works

- **Everything is a markdown file.** Entries live in `wiki/`, grouped into category subfolders (`books/`, `trips/`, …) or project subfolders (`projects/[name]/dods/`, …).
- **Frontmatter is the structure.** `category`, `context`, `tags`, plus category-specific properties (`author`, `destination`, `director`, …) drive Obsidian's Bases views and Graph View connections.
- **Wikilinks everywhere.** Entries cross-link through the body and through frontmatter (`author: "[[Liu Cixin]]"`). Unresolved links are fine — they're breadcrumbs for future pages.
- **Claude handles the filing.** You capture, the skills classify and route per `CONVENTIONS.md`.

See `vault/CLAUDE.md` and `vault/CONVENTIONS.md` for the full schema.

## Setup

### 1. Clone the starter and open the vault in Obsidian

```sh
git clone https://github.com/jordiparra/second-brain.git ~/second-brain
```

In Obsidian: `Open folder as vault` → select `~/second-brain/vault/`.

Enable the community plugins (`custom-sort`, `obsidian-hide-sidebars-when-narrow`) when prompted. The Bases views in `lists/` will render immediately.

### 2. Install the skills globally

Symlink the four skill folders into `~/.claude/skills/` so Claude Code picks them up in every session:

```sh
for skill in notes-ingest notes-query notes-lint wrap; do
  ln -s ~/second-brain/skills/$skill ~/.claude/skills/$skill
done
```

Verify:

```sh
ls -la ~/.claude/skills/ | grep -E "notes-|wrap"
```

### 3. Tell your global `~/.claude/CLAUDE.md` about the wiki

So Claude checks your notes before answering personal questions. Add a section like this to `~/.claude/CLAUDE.md`:

```markdown
## Knowledge base

- I maintain a personal wiki at `~/second-brain/vault/` — my second brain for travel, books, restaurants, gear, projects, prototypes, people, and more.
- **Any question about my personal life, plans, preferences, history, or past decisions should check the wiki first.** This includes questions that sound like calendar lookups, booking details, project status, or recommendation requests — the answer is often there. Use the `notes-query` skill, which handles the search workflow.
- The wiki's full schema is in its CLAUDE.md — read that before making any changes.
```

Adjust the path if you cloned somewhere else.

### 4. Adding content, querying, linting

**With [auto-mode](https://docs.claude.com/en/docs/claude-code/settings#permission-modes) on** (Claude runs autonomously without stopping for routine permission prompts), you can add notes, ask questions, and run the lint from any directory. The global `CLAUDE.md` snippet from step 3 tells Claude where the vault lives, and the skills `cd` into it themselves. Drop a PDF in `~/Downloads`, paste a URL mid-conversation, or ask a personal question from any project — it all routes correctly.

For example, mid-session in an unrelated project directory, paste a link and ask Claude to save it:

```
> save this to the wiki: https://stephango.com/vault
```

Claude recognises the soft-launch trigger, spawns `notes-ingest`, fetches the article, routes it to `wiki/articles/`, archives the raw HTML under `sources/articles/`, adds wikilinks (author, topics), updates `wiki-index.md`, and prepends a line to `wiki-changelog.md` — all without you leaving your current directory. Querying (*"any restaurant recs in Barcelona?"*) and linting (*"lint the wiki"*) work the same way — ask from anywhere.

**Without auto-mode**, launch Claude from the vault — less convenient, but avoids extra permission prompts:

```sh
cd ~/second-brain/vault && claude
```

See **Using it** below for full details on each skill.

## The four skills

| Skill | Purpose |
|-------|---------|
| `notes-ingest` | Process a source (article, URL, PDF, screenshot, pasted text) into wiki entries. Archives the raw source, creates entries, cross-links, updates the index. |
| `notes-query` | Answer questions from the wiki. Reads `wiki-index.md` + `log-index.md`, drills into relevant pages, synthesizes. |
| `notes-lint` | Audit the wiki for broken links, index drift, missing frontmatter, tag gaps, orphan pages. |
| `wrap` | End-of-session skill. Writes a curated `log/YYYY-MM-DD-HHMM.md` with learnings, decisions, and course corrections, updates indexes, then prompts `/clear`. |

## Using it

### Ingesting a source

Two ways — both work from inside the vault (`cd ~/second-brain/vault && claude`):

- **Explicit:** `/notes-ingest <path-or-url-or-pasted-content>`. Call the skill directly when you want deterministic routing.
- **Soft launch:** just share the source in conversation — paste a URL, drop a file path, send a screenshot, or say *"save this"* / *"add this to the wiki"*. The skill's description triggers it automatically and Claude routes the content to the right category.

Either way, Claude reads the source end-to-end, picks a category, creates the wiki entry with frontmatter and wikilinks, archives the raw source under `sources/`, updates `wiki-index.md`, and prepends an entry to `wiki-changelog.md`. If routing confidence is low, it asks before filing.

### Querying the wiki

- **Explicit:** `/notes-query <question>`.
- **Soft launch:** ask any personal question from any working directory — *"where am I staying in Berlin?"*, *"what books have I read about design?"*, *"any restaurant recs in Barcelona?"*. The global `CLAUDE.md` snippet from setup step 3 tells Claude to check the wiki first for personal questions, so `notes-query` triggers automatically. No need to `cd` anywhere for quick lookups.

Claude answers with wikilink citations (`According to [[page-name]], …`) and says so clearly when the wiki has nothing relevant.

### Wrapping a session

Run `/wrap` at the end of a working session. It captures non-obvious context — why you made a choice, what didn't work, course corrections — so the next session can pick up cold. Then it prompts `/clear`.

### Linting

Run `/notes-lint` **weekly**, or after a burst of ingestion (e.g. 10+ new entries in a sitting). It catches broken wikilinks, index drift, missing tags, orphan pages, and missing hub pages. Weekly is enough for a wiki under ~500 entries; stretch to biweekly once the structure settles and ingestion slows.

## Viewing connections in Obsidian

Press **⌘G** inside Obsidian to open Graph View. Every entry is a node, every `[[wikilink]]` an edge — books cluster around shared genres, trips pull in their places and restaurants, projects connect to their DODs and references, session logs mesh with the entries they touched. The conventions' insistence on wikilinks *inside frontmatter* (not plain strings) exists specifically to make this graph dense and navigable. Enable the **Tags** filter in Graph View to also see shared tags as connecting nodes.

## Customising for yourself

- Edit `vault/CONVENTIONS.md` to define your own `context` values (the "worlds" your life splits into — `work`, `personal`, `ai`, whatever fits).
- Delete or rename template categories you don't need. Add new ones as they appear.
- The `lists/` Base views ship with sensible defaults. Tune sort orders, filters, and columns to taste.
- The skills are generic — they read from `CONVENTIONS.md` at runtime, so your schema changes flow through automatically.

## Credits

Inspired by:

- [**Why 2026 Is the Year to Build a Second Brain** — Nate B Jones](https://www.youtube.com/watch?v=0TpON5T-Sw4) — capture-and-route methodology, "one behavior from the human", design-for-restart.
- [**How I use Obsidian** — Steph Ango (kepano)](https://stephango.com/vault) — properties as the primary structure, Bases for browsing, one file per entity.
- [**LLM Wiki** — Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — the LLM as a persistent, compounding knowledge base rather than re-deriving context on every query.

## License

MIT — see `LICENSE`.
