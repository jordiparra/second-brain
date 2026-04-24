---
name: wrap
description: |
  This skill should be used to wrap up a session by writing a curated session log to the Notes wiki / knowledge base, then prompting to /clear. It should activate at the end of any session that involved the Notes wiki or produced wiki-worthy learnings. Triggers include "/wrap", "wrap up", "let's wrap", "wrap this session", "log and clear", "done for today", "end session", or "log this session".

  Examples:
  - "/wrap"
  - "wrap up"
  - "let's wrap"
  - "done for today"
---

# Wrap up the session

You are writing a curated session recap for the personal notes. Run this from the vault's working directory — paths below are relative to the vault root.

## Before you start

1. Read the authoring conventions: `CONVENTIONS.md` (at the vault root).
2. Read `sessions-index.md` to see existing sessions.

## What to capture

Review the full conversation and extract **only what would help a future session pick up context**. Not a transcript — curated highlights.

### Relevance criteria

**Always capture:**
- Decisions made and their rationale (especially non-obvious ones)
- Course corrections — moments where the user redirected your approach ("not like that", "actually do X instead"). These are the highest-value signals.
- Learnings — new information discovered, insights, useful things found
- Mistakes and dead ends worth avoiding next time
- Wiki/vault changes made during the session

**Skip:**
- Routine back-and-forth
- Implementation details that are in the code or commit history
- Things already captured as wiki/ entries or Claude memory
- Anything the user could re-derive by reading the current state of files
- **Secrets of any kind** — see Safety below

### Safety

Session logs live under `log/` — same vault, same git history. The Safety rules in `CONVENTIONS.md` apply here too: **never write API keys, tokens, passwords, recovery phrases, card numbers, or any other credential value into a session log.**

If the session involved credentials (debugging an auth flow, setting up an MCP server, discussing `.env` contents, pasting a token to inspect it):

- **Reference by location, not value.** Write `used the token from gh auth token` — not the token itself. Preferred whenever the secret has a known home.
- **Describe the pattern, not the payload.** Write `JWT with 1h TTL, signed with RS256` — not the encoded JWT.
- **Redact in quoted output.** If a session log quotes a command's output that contained a secret, replace the value with `<REDACTED>` (or typed: `<REDACTED:api-key>`, `<REDACTED:jwt>`, `<REDACTED:card>****1234`). Lowercase kebab-case after the colon. Don't use `<YOUR_API_KEY>`-style placeholders for redaction — those are only for template/example code.

See `CONVENTIONS.md` → Safety for the full rules. When unsure whether something is a secret, treat it as one.

### Course corrections specifically

These are the most valuable thing to log. Watch for:
- User saying "no", "not that", "stop", "actually", "instead"
- User shortening or redirecting after a long Claude response
- Approach changes — something was tried, failed or was rejected, and a different path was taken
- Preference signals — user choosing one option over another

Log the **what** and **why**: "Switched from X to Y because Z."

## Write the log

### 1. Create `log/YYYY-MM-DD-HHMM.md`

Use the current date and time. Format:

```markdown
---
category: "[[Session Logs]]"
tags: [tag1, tag2]
---

# YYYY-MM-DD — One-line session summary

## Topics

- Brief list of what the session covered

## Learnings

- New information, insights, useful discoveries

## Decisions

- Choices made and rationale

## Course corrections

- Redirections, "not like that" moments, approach changes

## Mistakes to avoid

- Things that went wrong or patterns not to repeat

## Vault changes

- What was added/modified in the wiki (if anything)
```

Skip any section that has nothing for this session. Keep each section concise — bullet points, not paragraphs.

### Frontmatter and linking

**Tags:** Think of tags like Twitter hashtags — use several per entry to create micro-clusters. Each tag is a lightweight facet; the combination creates specificity. Aim for 3–6 tags per log covering:
- **Domain:** `work`, `personal`, `ai`
- **Project/area:** project codename, hobby name, focus area
- **Activity:** `ingest`, `lint`, `refactor`, `bugs`, `QA`, `setup`, `research`, `review`
- **Tool:** `obsidian`, `claude-code`, `things`, `raycast`
- **Topic:** `trips`, `products`, `wiki-structure`, `graph-view`, `prototypes`, `templates`, `docs`

Grep existing log tags (`grep -h "^tags:" log/*.md`) to stay consistent, but create new tags freely when needed — the vocabulary grows organically.

**Wikilinks in body text:** Link first mentions of wiki entities — projects, prototypes, trips, places, people, and concepts. Unresolved links are fine (they become breadcrumbs in Graph View). Don't over-link — skip generic terms and only link where the connection is meaningful.

### 2. Prepend to `sessions-index.md`

Insert one paragraph at the **top** of the entry list (below the header and `---` separator), so newest appears first. Never edit existing entries.

```
**[[YYYY-MM-DD-HHMM]]** — One-line summary.
```

Blank line between entries.

### 3. If wiki entries were added or modified

- Prepend to `wiki-changelog.md` — one paragraph per change at the **top** of the entry list (below the header and `---` separator), newest first. Format: `**YYYY-MM-DD** — [Capitalized verb] [what changed and why, with wikilinks and paths].` Blank line between entries. Lead with an action verb (`Added`, `Updated`, `Ingested`, `Fixed`, `Renamed`, `Created`). Never edit previous entries.
- Update `wiki-index.md` — add/remove entries in the relevant category section

## After writing

Confirm what was logged in one short line, then:

```
Type `/clear` to start fresh.
```
