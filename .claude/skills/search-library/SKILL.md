---
name: search-library
description: Search across the knowledge library for sources, synthesis notes, or progression entries matching a query. Use when the user asks "do we have anything on X?", "what have I read about Y?", or "find sources tagged Z".
---

# search-library

Searches `library/` for content matching a query. Returns a ranked list of hits with
file paths and a one-line preview each.

## Invocation

The caller provides:
- `query` — free-text search string (concept, author, tag, slug fragment)
- (optional) `domain` — limit search to one domain
- (optional) `kind` — `source` | `synthesis` | `progression`

## Workflow

1. Decide search scope:
   - If `domain` is given, search only `library/<domain>/`.
   - Else, search all of `library/` excluding `INDEX.md`.
2. Use `rg` (ripgrep) for the text scan. Search both file content and frontmatter.
3. For each hit, return:
   - File path
   - Domain (parent folder under `library/`)
   - Kind (source / synthesis / progression — inferred from path)
   - A 1-line preview around the match
4. Rank hits by:
   - Frontmatter match (title, tags, id) > body match
   - Synthesis files > sources (more curated)
   - Recency (newer `added` / `last_updated` first as tiebreaker)

## Rules

- **Read-only.** This skill never writes.
- **Do not fabricate hits.** If nothing matches, say so and suggest broader terms.
- **Respect `domain` scope strictly** when provided — no cross-domain leakage.
