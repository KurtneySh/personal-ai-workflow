# English Core Concepts Pass Design

## Context

The Chinese v1 handbook has been stabilized and is the source of truth. The English tree currently mirrors the Chinese structure only at the top level and still contains placeholder content.

This pass creates the first useful English reader path without attempting a full handbook translation.

## Goal

Make the English default entrypoint and core concept modules useful enough for public readers to understand the handbook's workflow philosophy and decision model.

The pass should help an English reader answer:

- what this handbook is for,
- how to classify a project by AI workflow level,
- what infra is appropriate for each project level,
- how to use an infra checklist without overbuilding,
- when a task is suitable for an agent loop,
- when repeated work should become a reusable skill.

## Non-Goals

- Do not translate all Chinese v1 content.
- Do not translate cookbook scenario pages in this pass.
- Do not translate the new-project playbook in this pass.
- Do not translate project templates beyond keeping existing placeholders navigable.
- Do not translate the full reference appendix in this pass.
- Do not change the Chinese source of truth except if a link or status line must be kept consistent.

## Scope

### Reader-Facing Files

- `README.md`
- `en/guides/README.md`
- `en/guides/project-levels.md`
- `en/guides/infra-by-level.md`
- `en/guides/infra-checklists.md`
- `en/loops/README.md`
- `en/skill-workflow/README.md`

### Working Records

- `docs/repo/translation-status.md`

## Translation Strategy

The English text should be idiomatic and public-facing rather than literal line-by-line translation. The information architecture, decisions, and safety boundaries must stay aligned with the Chinese v1 source.

Keep these workflow terms stable in English:

- `agent`
- `infra`
- `loop`
- `skill`
- `handoff`
- `verification signal`
- `human gate`
- `stop / escalate`

When Chinese text describes human judgment, review gates, explicit confirmation, or risky remote actions, the English translation must preserve that boundary clearly. The result should not imply that agents may push, merge, deploy, migrate, change permissions, expose sensitive data, or install system-level skills without human review.

## Content Design

### README.md

`README.md` becomes the English-first public entrypoint.

It should include:

- a short description of the handbook,
- a 3-minute reading path equivalent to `README.zh.md`,
- a repository map,
- current language status,
- a link to the Chinese source entrypoint.

It should no longer say English content will be translated only after Chinese stabilizes. It should instead state that Chinese v1 is the source of truth and English core concepts are in v1 draft.

### en/guides/

`en/guides/README.md` should act as the English guide entrypoint and link to the three guide pages.

The guide pages should cover:

- project levels from personal small projects to multi-person larger projects,
- minimum useful infra by level,
- optional infra checklist categories that can be added as project risk and collaboration needs increase.

### en/loops/

`en/loops/README.md` should explain when loop automation is appropriate, how to design a loop, what stop conditions are required, and which decisions remain human-owned.

### en/skill-workflow/

`en/skill-workflow/README.md` should explain system-level skills, project-level skills, when to capture a workflow as a skill, and how loop vs non-loop workflows affect who notices skill opportunities.

## Verification

Run a local Markdown link check across:

- `README.md`
- `README.zh.md`
- `en/**/*.md`
- `zh/**/*.md`
- `docs/repo/**/*.md`
- current specs

Run placeholder and stale-status scans for:

- `TODO`
- `TBD`
- `待定`
- `FIXME`
- `lorem`
- stale language status such as "after Chinese stabilizes"

Run `git diff --check`.

## Acceptance Criteria

- `README.md` is a useful English-first entrypoint.
- English links to all translated core concept docs resolve locally.
- `en/guides/`, `en/loops/`, and `en/skill-workflow/` no longer read as placeholders.
- `docs/repo/translation-status.md` marks the translated English core modules accurately.
- Cookbook, playbooks, templates, cases, and reference remain clearly out of scope for this pass.
- No English document contradicts the Chinese v1 safety and responsibility boundaries.
