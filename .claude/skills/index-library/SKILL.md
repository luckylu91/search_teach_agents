---
name: index-library
description: Refresh the top-level library index (library/INDEX.md) by scanning every domain folder and listing sources, syntheses, and progression status. Use after adding sources, after a synthesis pass, or when the user asks "what's in the library?".
---

# index-library

Walks `library/` and rewrites `library/INDEX.md` with a current snapshot.

## Output format

```markdown
# Library index

_Last refreshed: YYYY-MM-DD_

## <domain-1>
- **Target level:** <from progression.md>
- **Sources:** <N> ([list](./<domain-1>/sources/))
- **Synthesis files:** <N> ([list](./<domain-1>/synthesis/))
- **Last progression update:** YYYY-MM-DD
- **In progress:** <count> items
- **Open questions:** <count>

## <domain-2>
...
```

## Workflow

1. List immediate subdirectories of `library/`. Each is a domain.
2. For each domain:
   - Count files in `sources/` and `synthesis/`.
   - Read `progression.md` frontmatter for `target_level` and `last_updated`.
   - Count items in `In progress` and `Open questions` sections.
3. Overwrite `library/INDEX.md` with the rendered snapshot.

## Rules

- **This skill is read-mostly.** The only file it writes is `library/INDEX.md`.
- **Never modify domain content** (sources, synthesis, progression).
- **Skip hidden / dotfiles** and any folder without a `progression.md` (treat as
  not-yet-initialized and flag it in the index under a `## Uninitialized` section).
