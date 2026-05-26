---
name: add-source
description: Register a new source (URL, PDF, or manual note) under a domain in the knowledge library. Use when the user says "add this paper", "save this URL", "ingest this PDF", or when an agent (typically explorer) wants to persist a discovered source.
---

# add-source

Registers a single source under `library/<domain>/sources/<slug>.md`.

## When invoked

The caller provides:
- `domain` — kebab-case scientific domain (e.g. `diffusion-models`)
- `source` — one of: URL, file path to a PDF, or raw text/notes
- (optional) `title`, `authors`, `year`, `kind`

If `domain` doesn't exist yet, create the skeleton:
```
library/<domain>/
  sources/
  synthesis/
  pdfs/            (created on demand by the download-source-pdf skill)
  progression.md   (empty stub, see learning-progression agent for format)
```

To also preserve a PDF copy of the source, the caller invokes the `download-source-pdf`
skill **after** this one. This skill itself never downloads binaries.

## Source file format

Every source file MUST use this frontmatter:

```markdown
---
id: <kebab-case-slug>            # unique within the domain
title: <full title>
authors:
  - <name>
year: <YYYY>
kind: paper | survey | blog | tutorial | code | book | manual-note
url: <canonical URL if any>
added: YYYY-MM-DD
tags:
  - <tag>
---

## Why it matters
<one paragraph, 1-3 sentences>

## Abstract / excerpt
<verbatim quote from the source, if available>

## Notes
<manual notes, if any — keep separate from the verbatim excerpt>
```

## Rules

- **One source = one file.** Never bundle.
- **Slug must be unique within the domain.** If a collision exists, append `-2`, `-3`, etc.
- **Verbatim quotes go under `Abstract / excerpt`**, never paraphrased silently.
- **Do not download PDFs.** Record the URL; binary preservation is the job of the
  `download-source-pdf` skill.
