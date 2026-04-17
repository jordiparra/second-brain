---
name: notes-query
description: |
  This skill should be used to query the personal Notes wiki / knowledge base / second brain to answer a question. Triggers include "check the wiki", "what do my notes say about", "search the wiki", "search my knowledge base", "look up in notes", "what have I read about", "what do I know about", "do I have anything on", "what was that thing about", "remind me about", "find that note about", "what books have I read", "what did we discuss last time", or any question that references the user's personal knowledge, notes, reading history, session logs, or wiki content.

  Also triggers on personal questions that the wiki might answer — travel plans, restaurant recommendations, booking details, product/gear lookups, project status, prototype URLs, "where am I staying", "have I been to", "what do I think of", "what's the status of", "where's the prototype for", or any question about the user's life, work projects, or preferences that isn't purely factual/real-time.

  Examples:
  - "what do my notes say about design systems?"
  - "have I read anything about spatial computing?"
  - "check the wiki for Vannevar Bush"
  - "what did we talk about last session?"
  - "any restaurant recs in Barcelona?"
  - "where am I staying in Berlin?"
  - "what's the status of my current project?"
  - "/notes-query"
---

# Query the notes

Search and synthesize information from the personal notes. Structure and navigation are in the vault's `CLAUDE.md` (auto-loaded when the vault is the working directory).

## Workflow

### 1. Understand the question

Clarify what the user is asking if ambiguous. Determine whether this is:
- A factual lookup ("what books have I read about X?")
- A synthesis question ("what do my notes say about Y?")
- A gap analysis ("do I have anything on Z?")

### 2. Search the wiki

1. Read `wiki-index.md` to identify which pages are likely relevant
2. Read those pages
3. If the index doesn't surface enough, use Grep to search across all `.md` files in `wiki/` and `sources/`
4. For session history questions, also check `sessions-index.md` (newest-first) and relevant `log/` entries

### 3. Synthesize an answer

- Answer the question directly, citing wiki pages with `[[wikilinks]]`
- Attribute claims to their sources: "According to [[page-name]], ..."
- If the wiki has partial coverage, say what it covers and what's missing
- If the wiki has nothing relevant, say so clearly

### 4. File only if asked

Don't offer to create wiki pages from query results. Only file new content if the user explicitly asks.

## Guidelines

- Don't hallucinate wiki content — only cite what's actually in the pages
- If the question would be better answered by a web search than the wiki, say so
