# Repository Maintenance Guide

This file records temporary buildout rules for the handbook.

## Content Boundaries

- `zh/` and `en/` are reader-facing handbook content.
- `docs/repo/` is temporary repo working material.
- `docs/superpowers/` stores design specs and implementation plans.
- `.superpowers/` is local brainstorm tooling output and must stay untracked.

## Adding New Handbook Content

When adding a new reader-facing document:

1. Put the Chinese source under the matching `zh/` section.
2. Add or update the matching English placeholder under `en/`.
3. Link the document from `README.zh.md` if it is part of the main reading path.
4. Update `docs/repo/translation-status.md`.

## Commit Style

Use Conventional Commits.

Examples:

- `docs: add project level guide`
- `docs: add loop readiness checklist`
- `docs: add skill workflow examples`
- `chore: update translation status`
