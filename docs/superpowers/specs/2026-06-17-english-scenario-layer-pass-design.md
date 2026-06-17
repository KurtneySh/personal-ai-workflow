# English Scenario Layer Pass Design

## Context

The English core concepts pass is complete for `README.md`, `en/guides/`, `en/loops/`, and `en/skill-workflow/`. The remaining English tree still contains placeholder sections for scenario-heavy content.

This pass translates the scenario usage layer: the end-to-end playbook and the concrete cookbook scenarios that show how to use Claude Code and Codex CLI in real work.

## Goal

Make English readers able to move from understanding the workflow model to applying it in common development scenarios.

The pass should help an English reader answer:

- how to start a new project with appropriate AI workflow infra,
- how to orient an agent in an existing repo,
- how to hand off a bounded task,
- how to handle review, test, lint, CI, or failure feedback,
- how to refresh context after a long session, interruption, agent switch, or context compaction,
- how to organize PR / CI / review work,
- how to delegate work across multiple agents without losing ownership or final review.

## Non-Goals

- Do not translate `en/templates/` beyond keeping placeholder status accurate.
- Do not translate `en/reference/`.
- Do not create real case studies under `en/cases/`.
- Do not change the Chinese source of truth unless an objective link or status inconsistency is discovered.
- Do not add scripts or automation for translation.

## Scope

### Reader-Facing Files

- `README.md`
- `en/playbooks/README.md`
- `en/playbooks/start-a-new-project.md`
- `en/cookbook/README.md`
- `en/cookbook/repo-orientation.md`
- `en/cookbook/task-handoff.md`
- `en/cookbook/review-and-fix.md`
- `en/cookbook/context-refresh.md`
- `en/cookbook/pr-ci-workflow.md`
- `en/cookbook/multi-agent-delegation.md`

### Working Records

- `docs/repo/translation-status.md`

## Translation Strategy

The English text should be idiomatic and directly usable. It should not read like a literal line-by-line translation, but it must preserve the structure, intent, and safety boundaries of the Chinese v1 source.

Keep these workflow terms stable:

- `agent`
- `infra`
- `loop`
- `skill`
- `handoff`
- `verification signal`
- `human gate`
- `stop / escalate`
- `ownership`

Prompt examples should be usable as practical Claude Code or Codex CLI prompts. They must not imply that an agent may bypass human confirmation, especially for risky commands, remote actions, push, merge, deployment, migrations, permission changes, sensitive data, CI ownership, review decisions, or multi-agent integration.

## Content Design

### README.md

Update the repository map and reading path so `en/playbooks/` and `en/cookbook/` are no longer described as planned English sections after this pass.

The entrypoint should make clear that:

- core concepts are already in English v1 draft,
- playbooks and cookbook are now English v1 draft scenario guidance,
- `en/templates/`, `en/reference/`, and `en/cases/` remain later English passes.

### en/playbooks/

`en/playbooks/README.md` should become the English playbook entrypoint and link to `start-a-new-project.md`.

`en/playbooks/start-a-new-project.md` should translate/adapt `zh/playbooks/start-a-new-project.md`. It should preserve:

- project-level judgment before infra choices,
- minimal useful infra by level,
- staged agent collaboration,
- human decisions around scope, risk, dependencies, release, and maintenance,
- explicit verification before treating the project as ready.

### en/cookbook/

`en/cookbook/README.md` should become the English cookbook entrypoint and link to all scenario pages.

The scenario pages should translate/adapt:

- `zh/cookbook/repo-orientation.md`
- `zh/cookbook/task-handoff.md`
- `zh/cookbook/review-and-fix.md`
- `zh/cookbook/context-refresh.md`
- `zh/cookbook/pr-ci-workflow.md`
- `zh/cookbook/multi-agent-delegation.md`

Each scenario should include:

- when to use it,
- required inputs,
- what the agent should do,
- what the human must still decide,
- prompt examples where present in Chinese source,
- verification or stop conditions,
- when to consider a future loop or skill.

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
- stale language status such as "planned English section" for `en/playbooks/` or `en/cookbook/`
- stale wording such as "Translation is planned in a later English pass" inside `en/playbooks/` or `en/cookbook/`

Run `git diff --check`.

## Acceptance Criteria

- `README.md` accurately presents playbooks and cookbook as English v1 draft content.
- `en/playbooks/` has an entrypoint and translated `start-a-new-project.md`.
- `en/cookbook/` has an entrypoint and translated scenario pages matching the Chinese v1 scenario set.
- Prompt examples are usable but preserve human gates and risky-action boundaries.
- `docs/repo/translation-status.md` marks `playbooks` and `cookbook` as English `v1 draft`.
- `en/templates/`, `en/reference/`, and `en/cases/` remain later English passes.
- Local Markdown link checking reports no broken links.
- No English document contradicts the Chinese v1 safety and responsibility boundaries.
