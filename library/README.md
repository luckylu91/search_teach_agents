# Library

Knowledge library for this platform. Each immediate subfolder is one **scientific domain**.

## Per-domain layout

```
<domain>/
  sources/           # raw imports — one .md per source (see add-source skill)
  pdfs/              # optional binary copies of sources (see download-source-pdf skill)
                     # tracked via Git LFS — NEVER read by agents
  synthesis/         # derived structured notes (see synthesizer agent)
  progression.md     # long-term learning log for this domain (see learning-progression agent)
  course.md          # optional: course outline (see course-author agent)
```

## PDF preservation (Git LFS)

PDFs live under `library/<domain>/pdfs/` and are versioned via **Git LFS**, not plain Git.
The pattern `library/**/pdfs/*.pdf` is declared in the repo's `.gitattributes`.

**Agents never read PDF files.** They are opaque, human-only artifacts. Every PDF has a
sibling markdown file under `sources/` — that's where agents read content from.

Anyone cloning this repo needs Git LFS installed (`git lfs install`) before PDFs will
materialize on checkout.

## Top-level files

- `INDEX.md` — auto-generated snapshot of all domains. Refresh via the `index-library` skill.

## Rules

- **One domain = one folder.** Domain folders use kebab-case (`diffusion-models`, not `Diffusion Models`).
- **Never mix domains in a single file.** Progression and synthesis files are strictly per-domain.
- **Sources are append-only-ish.** You may correct metadata, but don't silently rewrite an
  ingested source's content — add notes instead.
