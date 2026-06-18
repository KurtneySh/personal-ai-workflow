# English Templates Pass Design

## Context

Chinese v1 is the source of truth for this handbook. English v1 draft content already covers the public entrypoint, core concepts, loops, skill workflow, the new-project playbook, and cookbook scenarios.

The remaining English `templates` section is still a placeholder, while the Chinese project workspace templates under `zh/templates/project/` are stabilized. This pass turns the English templates section into usable project workbench documentation without expanding the scope into cases or reference material.

## Goal

Translate the stabilized Chinese project workspace templates into English while preserving the Chinese information architecture and intent.

The English templates should help readers create the smallest useful AI-readable project workspace for a given Level 1-4 project, including project entrypoint, project specification, project state, and agent operating rules.

## Approach

Use a strict mirror approach:

- Preserve the Chinese v1 file structure and section semantics.
- Translate content into clear English with light phrasing improvements where needed.
- Keep Chinese v1 as the source of truth.
- Avoid adding English-only concepts, fields, scripts, or workflow requirements.

This avoids letting the English translation become a competing source of truth.

## Scope

Create or update:

- `en/templates/README.md`
- `en/templates/project/README.md`
- `en/templates/project/README.template.md`
- `en/templates/project/SPEC.template.md`
- `en/templates/project/STATE.template.md`
- `en/templates/project/AGENTS.template.md`
- `README.md`
- `docs/repo/translation-status.md`

## Out of Scope

- No changes to Chinese source files.
- No scripts or automation.
- No translation of `reference`.
- No expansion of `cases`.
- No new template categories beyond the existing project workspace templates.
- No English-only loop, skill-capture, or review-gate fields unless they already exist in the Chinese source templates.

## Content Requirements

The English template section must preserve these ideas from the Chinese source:

- Templates are examples and starting points, not mandatory standards.
- Project level determines which templates are required or optional.
- `README.md` and `STATE.md` are required for Level 1-4.
- `SPEC.md` is optional for Level 1 and required for Level 2-4.
- `AGENTS.md` is optional for Level 1 and required for Level 2-4.
- Level 3 needs clearer collaboration, review, ownership, and handoff rules.
- Level 4 needs explicit risk constraints, permission boundaries, human approval points, and rollback requirements.
- AI-generated drafts must be grounded in real repository files and scripts.
- Missing information should be marked as needing confirmation instead of invented.

## Acceptance Criteria

- English templates are structurally aligned with `zh/templates/project/`.
- All placeholder wording in `en/templates/` is removed.
- English README links point to the new English templates.
- Translation status marks `templates` as English v1 draft.
- No broken relative Markdown links are introduced.
- `git diff --check` passes.

## Risks

- Translation may accidentally strengthen optional guidance into mandatory rules. Mitigation: preserve required and optional labels from the Chinese source.
- English wording may drift from Chinese v1. Mitigation: keep the file structure and section intent mirrored.
- Template links may point to files that do not exist. Mitigation: verify links after implementation.
