# Chinese V1 Stabilization Audit

Date: 2026-06-16

This audit checks whether the Chinese source content is stable enough to become the source of truth for the next English structure and translation pass.

## Scope

- `README.zh.md`
- `zh/**/*.md`
- `docs/repo/*.md`
- `docs/repo/decisions/*.md`
- `docs/superpowers/specs/*.md`
- `docs/superpowers/plans/*.md`

## Result

Chinese v1 is ready for the English structure and translation pass.

No blocking issues were found in reader-facing Chinese documents. The audit found a few stale working-record or navigation details, which were fixed as part of this pass.

## Commands Run

```bash
git status --short --branch
find zh -name '*.md' -type f | sort
find docs/superpowers/specs -name '*.md' -type f | sort
find docs/superpowers/plans -name '*.md' -type f | sort
find docs/repo -name '*.md' -type f | sort
rg -n "TODO|TBD|待定|占位|FIXME|lorem" README.zh.md zh docs/repo || true
rg -n "后续场景|Planned Scenarios" zh/cookbook/README.md || true
```

A local Markdown link checker was also run against `README.zh.md`, `zh/**/*.md`, and `docs/superpowers/specs/*.md`.

## Findings

### Blocking

None.

### Important

None after the fixes below.

### Minor

- `zh/cookbook/README.md` still used "后续场景 / Planned Scenarios" even though the planned cookbook scenarios had already been added.
- `docs/repo/translation-status.md` still described several Chinese sections as planned or in progress.
- `README.zh.md` listed `zh/cases/` without linking the cases entrypoint.

## Fixes Applied

- Reworded the cookbook future-scenario section into an extension principle for adding new real-world scenarios.
- Updated translation status to mark Chinese sections as v1 stabilized, while keeping English as placeholders.
- Added a README link to the `zh/cases/` entrypoint and template.

## Link Integrity

Reader-facing Chinese documents and current specs have no broken local Markdown links.

Historical implementation plans contain embedded Markdown examples and future file snippets. A naive link checker may report these snippets as broken links relative to the plan file path. Treat `docs/superpowers/plans/` as working records, not as reader-facing navigation.

## Placeholder Review

The only remaining placeholder marker in the current reader-facing entrypoint is intentional: `README.zh.md` states that English directories remain mirrored placeholders and should now be translated from the stabilized Chinese source.

Template files intentionally contain empty fields for users or agents to fill in later.

## Boundary Review

The Chinese docs consistently keep human confirmation gates around risky or remote actions, including push, merge, deployment, migration, permission changes, sensitive data handling, loop execution, and system-level skill promotion.

No contradiction was found where the docs authorize an agent to complete high-risk actions without human review.

## Translation Readiness

The next step should be an English structure pass:

1. Mirror the stabilized Chinese information architecture into English reader-facing files.
2. Translate `README.md` first because it is the public default entrypoint.
3. Translate core concepts before scenario docs: project levels, infra, loops, and skill workflow.
4. Translate templates and cookbook pages after terminology choices are stable.
5. Keep `docs/repo/translation-status.md` updated during the translation pass.

Before broad public sharing, decide whether `docs/repo/` and `docs/superpowers/` should remain visible as development records, move to an archive, or be excluded from public navigation.
