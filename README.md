# teacher_agent

Versatile, scalable platform for exploring, synthesizing, and teaching scientific domains
using Claude Code agents and skills. See [`CLAUDE.md`](./CLAUDE.md) for the conventions
Claude follows in this repo.

## Setup

This repo uses **Git LFS** to version PDF copies of sources under `library/<domain>/pdfs/`.
You need Git LFS installed locally before cloning or pulling, otherwise PDFs will appear
as LFS pointer files instead of real binaries.

### One-time install (per machine)

```bash
# macOS
brew install git-lfs

# Debian / Ubuntu
sudo apt install git-lfs

# Fedora / RHEL
sudo dnf install git-lfs

# Arch
sudo pacman -S git-lfs
```

Then, once per user account:

```bash
git lfs install
```

### After cloning

```bash
git clone <repo-url>
cd teacher_agent
git lfs pull            # materialize the actual PDF bytes
```

If you forgot `git lfs install` before cloning, run `git lfs pull` afterwards — it will
fetch the binaries and replace the pointer files.

### Verifying

```bash
git lfs ls-files        # lists every LFS-tracked file in the repo
git lfs track           # lists the patterns being LFS-tracked (should include library/**/pdfs/*.pdf)
```

## Layout

```
.claude/
  agents/        explorer, synthesizer, course-author, learning-progression
  skills/        add-source, download-source-pdf, index-library, search-library
  settings.json  permissions + a PreToolUse hook that blocks Read on PDF paths
library/
  <domain>/
    sources/     .md files (one per source) — agent-readable
    pdfs/        binary copies (Git LFS) — humans only
    synthesis/   derived notes
    progression.md
```

## Notes

- Agents are wired with explicit "never read PDFs" rules in their system prompts AND
  a project-level hook (`.claude/settings.json` → `PreToolUse` on `Read`) that blocks
  the tool when the path matches `library/**/pdfs/*.pdf`.
- Open `/hooks` in Claude Code at least once after pulling new changes to settings.json —
  the harness only reloads hook config when the menu is opened (or on session restart).
