# teacher_agent

Versatile, scalable platform for exploring, synthesizing, and teaching scientific domains.
Drives a growing knowledge library where sources are either fetched from the web by agents
or added manually.

## Repository layout

```
.claude/
  agents/                 # subagent definitions (one .md per agent)
  skills/                 # user-invocable skills (one dir per skill, SKILL.md inside)
  settings.json           # project permissions / env / hooks

library/                  # knowledge library, organized per-domain
  <domain>/
    sources/              # raw imports (markdown notes referencing URLs, PDFs, etc.)
    pdfs/                 # optional PDF copies — Git LFS, opaque to agents
    synthesis/            # derived, structured notes
    progression.md        # long-term learning progression for this topic
```

## Core agents

- **explorer** — surveys a scientific domain on the web, proposes sources to ingest.
- **synthesizer** — turns a set of sources into structured notes (concepts, key papers, open questions).
- **course-author** — turns synthesized notes into a course outline / lesson plan.
- **learning-progression** — maintains `library/<domain>/progression.md`: what's been learned,
  what's next, open questions. One progression file per domain — never mix topics.

## Core skills

- **add-source** — register a new source under a domain (URL or manual note).
- **download-source-pdf** — optional follow-up to `add-source`; downloads the PDF
  and stores it under `library/<domain>/pdfs/` (Git LFS).
- **index-library** — refresh the library index after additions.
- **search-library** — search across the library by domain / concept / source.

## PDFs and Git LFS

PDFs under `library/<domain>/pdfs/` are tracked via Git LFS (`.gitattributes` pattern:
`library/**/pdfs/*.pdf`). Before working with this repo, install LFS:

```bash
brew install git-lfs           # or: apt install git-lfs
git lfs install                # one-time per machine
git lfs pull                   # after cloning, to materialize PDFs
```

**Agents never open, read, or parse PDFs.** They are an offline-preservation layer for
humans. The text agents consume always lives in `sources/*.md`.

## Conventions

- One domain = one folder under `library/`. Never cross-pollinate progression files.
- Sources are content-addressable when possible: keep the original URL/citation in frontmatter.
- Synthesis files link back to the source IDs they were derived from.
- Agents must write their long-term findings into the library, not just chat.
