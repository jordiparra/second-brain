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

You are writing a curated session recap for the user's personal notes wiki. The wiki location is in their global `~/.claude/CLAUDE.md` under "Knowledge base" (or similar). All file references below are relative to the wiki root.

## Before you start

1. Read the authoring conventions: `CONVENTIONS.md` at the wiki root
2. Read `sessions-index.md` to see existing sessions

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

Session logs live under `log/` — same vault, same iCloud sync, same git history. The Safety rules in `CONVENTIONS.md` apply here too: **never write API keys, tokens, passwords, recovery phrases, card numbers, or any other credential value into a session log.**

If the session involved credentials (debugging an auth flow, setting up an MCP server, discussing `.env` contents, pasting a token to inspect it):

- **Reference by location, not value.** Write `used the API token from the keychain` — not the token itself. Preferred whenever the secret has a known home.
- **Describe the pattern, not the payload.** Write `JWT with 1h TTL, signed with RS256` — not the encoded JWT.
- **Redact in quoted output.** If a session log quotes a command's output that contained a secret, replace the value with `<REDACTED>` (or typed: `<REDACTED:api-key>`, `<REDACTED:jwt>`, `<REDACTED:card>****1234`). Lowercase kebab-case after the colon. Don't use `<YOUR_API_KEY>`-style placeholders for redaction — those are only for template/example code.

See `~/.claude/CLAUDE.md` → Security for the full canonical redaction forms (by-location, `<REDACTED>`, `<YOUR_*>`). When unsure whether something is a secret, treat it as one.

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

**Tags:** Think of tags like Twitter hashtags — each is a lightweight facet, and the combination creates specificity. Use as many as genuinely apply, no more — don't pad to hit a number, don't trim a real facet to stay under one. Cover whichever of these are relevant:
- **Domain:** `work`, `personal`, `ai`
- **Project/area:** `redesign`, `onboarding`, `second-brain`, `side-project`, etc.
- **Activity:** `ingest`, `lint`, `refactor`, `bugs`, `QA`, `setup`, `research`, `review`
- **Tool:** `obsidian`, `claude-code`, `things`, `figma`, `slack`, `notion`
- **Topic:** `trips`, `products`, `wiki-structure`, `graph-view`, `prototypes`, `templates`, `docs`

Grep existing log tags (`grep -h "^tags:" log/*.md`) to stay consistent, but create new tags freely when needed — the vocabulary grows organically.

**Wikilinks in body text:** Link first mentions of wiki entities — projects (`[[Project Alpha]]`), prototypes (`[[Dashboard Prototype]]`), trips (`[[Tokyo October 2026]]`), places, people, and concepts. Unresolved links are fine (they become breadcrumbs in Graph View). Don't over-link — skip generic terms and only link where the connection is meaningful.

### 2. Prepend to `sessions-index.md`

Insert one paragraph at the **top** of the entry list (below the header and `---` separator), so newest appears first. Never edit existing entries.

```
**[[YYYY-MM-DD-HHMM]]** — One-line summary.
```

Blank line between entries.

**Budget: ~120 characters, hard cap ~200.** The index is a finder, not a log — its job is to help future sessions scan and pick the right log file to open. Detail belongs in the log file body, not here.

Shape: `verb + project/scope + the one thing that makes this session findable`. Include the key wikilinks (project, person, place). Leave out: course corrections, sub-decisions, rationale narratives, multi-sentence recaps — those are already in the log body.

**Good:**
- `Renamed \`log-index.md\` → \`sessions-index.md\` across vault, skills, and Obsidian config.`
- `Ingested [[Onboarding PRD]] (Growth team) under [[Project Alpha]] references; scope = new-user flow + trial conversion.`

**Drift smell — stop and condense:** em-dashes stacking, multiple sentences, parenthetical after parenthetical, "Course correction:", "Follow-up:", or anything resembling a paragraph. If the line needs a second sentence to survive, the second sentence belongs in the log body.

### 3. If wiki entries were added or modified

Prepend to `wiki-changelog.md` — one entry per cohesive change thread at the **top** of the entry list (below the header and `---` separator), newest first. Format: `**YYYY-MM-DD** — [Capitalized verb] [what changed].` Blank line between entries. Lead with an action verb (`Added`, `Updated`, `Ingested`, `Fixed`, `Renamed`, `Created`). Never edit previous entries.

**Budget: ~120 characters, hard cap ~200.** The changelog is a finder, not a log — its job is to let future sessions scan and locate the right change. Full narrative belongs in the session log body, not here.

Shape: `verb + entity (with wikilinks) + the one detail that makes this change findable`. Include key wikilinks (page, project, person). Leave out: context/motivation, sub-decisions, file lists, multi-clause rationale — the session log already carries those.

**Good:**
- `Added [[Obsidian]] under Software, replacing the empty stray root file from an unresolved wikilink.`
- `Renamed Apps category → Software (folder, list, template, 5 entries); subtype tags carry the distinction.`
- `Ingested [[Onboarding PRD]] (Alex Chen, Sam Rivera) under [[Project Alpha]] references.`

**Drift smell — stop and condense:** "Context:", "Why:", "Motivation:", em-dashes stacking, parenthetical-after-parenthetical, multi-sentence narrative, file enumeration ("updated A, B, C, D, E, F"), lists of sub-decisions. If the line needs a paragraph to survive, the paragraph belongs in the session log body.

Then update `wiki-index.md` — add/remove entries in the relevant category section.

## After writing

Confirm what was logged in one short line, then:

```
Type `/clear` to start fresh.
```
