---
name: download-source-pdf
description: Download a source PDF and store it under library/<domain>/pdfs/, then link it from the corresponding source markdown file. Use only when the user (or an agent) explicitly asks to preserve a PDF copy of a source. The PDF itself is never opened, parsed, or read by any agent.
---

# download-source-pdf

Fetches a PDF from a URL and stores it under `library/<domain>/pdfs/<source-id>.pdf`,
then updates the matching `library/<domain>/sources/<source-id>.md` file to reference it.

PDFs are versioned via **Git LFS** (see `.gitattributes` — pattern `library/**/pdfs/*.pdf`).

## Invocation

The caller provides:
- `domain` — the kebab-case domain
- `source_id` — the slug used in `library/<domain>/sources/<source_id>.md`
- `url` — direct URL to the PDF

## Workflow

1. Verify `library/<domain>/sources/<source_id>.md` exists. If not, stop and tell the
   caller to run `add-source` first.
2. Create `library/<domain>/pdfs/` if missing.
3. Download with `curl -L -o library/<domain>/pdfs/<source_id>.pdf <url>`.
   Verify the file starts with the `%PDF-` magic bytes — if not, delete and report.
4. Add the path to the source's frontmatter under a `pdf:` field:
   ```yaml
   pdf: pdfs/<source_id>.pdf
   ```
5. Stage with `git add` so LFS picks it up. Do **not** commit — leave that to the user.

## Hard rules

- **Never open, read, parse, or extract text from the PDF.** Not with `Read`, not with
  `pdftotext`, not with any tool. The PDF is an opaque artifact for human consumption
  and offline preservation only.
- **Never `Read` files under `library/<domain>/pdfs/`.** If you need the source content,
  read the corresponding `.md` file under `sources/` — that's the human-readable layer.
- **One PDF per source.** Do not store multi-paper PDFs as a single file.
- **Storage is Git LFS.** Do not bypass it (no `--no-lfs`, no force-add of LFS pointers).
