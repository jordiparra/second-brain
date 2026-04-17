# second-brain

A starter kit for a personal knowledge base built as an Obsidian vault, with four Claude Code skills for AI-assisted capture, querying, linting, and session logging. No content included — just the scaffolding, conventions, templates, and skills.

Assumes you already use [Claude Code](https://claude.com/claude-code) and [Obsidian](https://obsidian.md/).

## What's in the box

```
second-brain/
├── vault/                ← The Obsidian vault (open this in Obsidian)
│   ├── CLAUDE.md         ← Structure and navigation (auto-loaded by Claude Code)
│   ├── CONVENTIONS.md    ← Authoring rules (frontmatter, linking, placement, routing)
│   ├── lists/            ← 16 Base views (Books, Movies, Trips, …)
│   ├── templates/        ← 32 entry templates
│   ├── wiki/             ← Empty — your entries go here
│   ├── sources/          ← Empty — raw source archive
│   ├── log/              ← Empty — session recaps
│   ├── wiki-index.md     ← Seed scaffold
│   ├── wiki-changelog.md ← Seed scaffold
│   ├── sessions-index.md ← Seed scaffold
│   └── .obsidian/        ← Obsidian settings: Bases, Bookmarks, plugins
└── skills/               ← 4 Claude Code skills
    ├── notes-ingest/
    ├── notes-query/
    ├── notes-lint/
    └── wrap/
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

### 4. Work from the vault

When you want Claude to write into the wiki, start the session with `vault/` as the working directory so the vault's `CLAUDE.md` auto-loads and relative paths resolve:

```sh
cd ~/second-brain/vault && claude
```

For quick queries from anywhere, `notes-query` still works — it just reads the path from your global `CLAUDE.md`.

## The four skills

| Skill | Purpose |
|-------|---------|
| `notes-ingest` | Process a source (article, URL, PDF, screenshot, pasted text) into wiki entries. Archives the raw source, creates entries, cross-links, updates the index. |
| `notes-query` | Answer questions from the wiki. Reads `wiki-index.md` + `sessions-index.md`, drills into relevant pages, synthesizes. |
| `notes-lint` | Audit the wiki for broken links, index drift, missing frontmatter, tag gaps, orphan pages. |
| `wrap` | End-of-session skill. Writes a curated `log/YYYY-MM-DD-HHMM.md` with learnings, decisions, and course corrections, updates indexes, then prompts `/clear`. |

## Customising for yourself

- Edit `vault/CONVENTIONS.md` to define your own `context` values (the "worlds" your life splits into — `work`, `personal`, `ai`, whatever fits).
- Delete or rename template categories you don't need. Add new ones as they appear.
- The `lists/` Base views ship with sensible defaults. Tune sort orders, filters, and columns to taste.
- The skills are generic — they read from `CONVENTIONS.md` at runtime, so your schema changes flow through automatically.

## Credits

Based on the personal Obsidian + Claude Code setup used by [@jordiparra](https://github.com/jordiparra). Inspired by:

- [**Why 2026 Is the Year to Build a Second Brain** — Nate B Jones](https://www.youtube.com/watch?v=0TpON5T-Sw4) — capture-and-route methodology, "one behavior from the human", design-for-restart.
- [**How I use Obsidian** — Steph Ango (kepano)](https://stephango.com/vault) — properties as the primary structure, Bases for browsing, one file per entity.
- [**LLM Wiki** — Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — the LLM as a persistent, compounding knowledge base rather than re-deriving context on every query.

## License

MIT — see `LICENSE`.
