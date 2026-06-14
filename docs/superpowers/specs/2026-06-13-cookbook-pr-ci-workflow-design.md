# Cookbook PR CI Workflow Scenario Design

Date: 2026-06-13

## Goal

Add a Chinese cookbook scenario for using Claude Code or Codex CLI through a PR and CI workflow.

The scenario should help readers coordinate agent work from pre-PR checks through PR update, CI failure handling, review comment handling, and pre-merge verification. It should act as a lifecycle orchestration page that points to existing cookbook scenarios at the right moments instead of duplicating them.

## Scope

Create one scenario page and link it from the cookbook entrypoint:

```text
zh/cookbook/pr-ci-workflow.md
zh/cookbook/README.md
```

The new page should follow the existing cookbook style used by:

- `zh/cookbook/repo-orientation.md`
- `zh/cookbook/task-handoff.md`
- `zh/cookbook/review-and-fix.md`
- `zh/cookbook/context-refresh.md`

## Non-Goals

This scenario will not:

- Become a full GitHub, GitLab, or `gh` command reference.
- Require a specific forge provider.
- Replace `review-and-fix.md` for concrete feedback handling.
- Replace `context-refresh.md` for interrupted or stale context recovery.
- Design an automatic CI-fix loop.
- Cover multi-agent delegation in depth.
- Add scripts or executable automation.
- Add English translation beyond existing English stub files.
- Create the formal `zh/cases/` module.

The page may include example commands, but commands must remain subordinate to the workflow scenario and should be framed as project-dependent examples.

## Scenario Definition

Create:

```text
zh/cookbook/pr-ci-workflow.md
```

Purpose:

Help a human guide an agent through PR and CI work without letting the agent overreach. The page should make clear what context the agent needs, when it should only summarize, when it may prepare a bounded fix, when it should use another cookbook scenario, and when it must stop for human confirmation.

The scenario should be useful when:

- preparing a branch for PR,
- asking an agent to draft or update a PR summary,
- checking whether a branch is ready to push,
- reading CI status or failure logs,
- turning CI failures or review comments into bounded fix tasks,
- deciding whether a PR is ready for human review or merge,
- resuming PR work after interruption or agent handoff.

## Boundary With Existing Cookbook Pages

`pr-ci-workflow.md` must make these boundaries explicit:

- Compared with `task-handoff.md`: PR / CI workflow coordinates the lifecycle. Concrete implementation work still needs a bounded task handoff.
- Compared with `review-and-fix.md`: PR / CI workflow may identify feedback, but concrete CI, test, lint, typecheck, or review fixes should be converted into Feedback Fix tasks.
- Compared with `context-refresh.md`: PR / CI workflow should use context refresh when branch state, recent commits, CI status, or review state is stale or unclear.
- Compared with `repo-orientation.md`: PR / CI workflow assumes the repo is already understood enough to reason about branch, diff, checks, and review state.
- Compared with `zh/loops/README.md`: PR / CI workflow is not an automatic fix loop. A CI loop requires explicit loop boundaries.

## Required Sections

The page should include:

- Purpose
- When to Use
- When Not to Use
- Inputs
- PR CI Workflow Template
- Workflow Stages
- Claude Code Prompt
- Codex CLI Prompt
- Expected Output
- Verification
- Stop and Escalate
- When This Becomes A Loop
- Skill Capture Signals
- Related Docs

## Inputs

The scenario should ask the human to provide:

- project level,
- branch or PR identifier,
- target branch if known,
- PR goal or change summary,
- files or areas changed,
- verification commands or required checks,
- CI status or logs if available,
- review comments if available,
- allowed actions,
- forbidden actions,
- push, PR update, and merge permissions,
- human confirmation gates,
- state record location.

The page should make clear that a PR or CI task without branch state, diff scope, and verification signal is under-specified.

## PR CI Workflow Template

Include a copyable template with fields:

- Workflow goal
- Project level
- Branch or PR
- Target branch
- Change summary
- Changed files or areas
- Required checks
- Current CI status or logs
- Review comments
- Allowed actions
- Forbidden actions
- Push permission
- PR update permission
- Merge permission
- Human confirmation gates
- State record location
- Open questions

The template should be suitable for `STATE.md`, issue comments, PR comments, or direct prompts.

## Workflow Stages

The page should describe a staged lifecycle:

1. **Pre-PR readiness**: confirm branch, diff, scope, verification commands, residual risk, and whether a PR summary can be drafted.
2. **PR summary and test plan**: ask the agent to produce a concise summary, test plan, risk notes, and reviewer notes from real diff evidence.
3. **CI status triage**: classify status as passing, failing with logs, pending, unavailable, or unknown.
4. **CI failure handling**: convert concrete failures into `Review And Fix` input instead of letting the agent auto-repair indefinitely.
5. **Review comment handling**: convert actionable review comments into bounded feedback tasks; ask humans to clarify subjective or architectural comments.
6. **Context refresh**: use `Context Refresh` when PR state, branch state, CI state, or agent context is stale.
7. **Pre-merge check**: verify branch state, required checks, unresolved comments, local diff, and human merge permission.

## Prompt Behavior

Claude Code and Codex CLI prompts should ask the agent to:

- inspect branch and working tree state,
- inspect diff and recent commits,
- read PR description, CI status, and review comments if available,
- summarize the PR goal and changed areas from evidence,
- identify required verification commands or checks,
- classify CI or review state,
- recommend the next action as prepare PR summary, push after verification, wait for CI, handle feedback, refresh context, stop, or ask human,
- avoid inventing commands, tests, CI checks, review comments, deployment flows, team rules, or risk assumptions,
- avoid pushing, updating PR metadata, merging, rerunning remote checks, deploying, migrating, changing permissions, or accessing external services unless explicitly authorized,
- avoid automatic repair loops.

The prompts should distinguish read-only PR/CI triage from authorized local fixes. If a fix is needed, the prompt should ask the agent to produce a `Feedback Fix` handoff or use `review-and-fix.md` with explicit scope.

## Expected Output

The agent output should include:

- branch and worktree state,
- PR or branch goal,
- changed files or areas,
- verification commands or required checks,
- CI status classification,
- review status classification,
- recommended next action,
- proposed PR summary or test plan when requested,
- feedback items that should become `Review And Fix` tasks,
- risks and unknowns,
- human confirmation gates before push, PR update, merge, deployment, migration, or external service access.

## Verification

Verification should cover both local and PR/CI state, while staying project-dependent.

Suggested local checks:

- `git status --short --branch`
- `git diff --check`
- `git log --oneline -5`
- project verification command from README, package scripts, CI config, or task handoff

Suggested PR/CI checks:

- required checks are passing, failing, pending, unavailable, or unknown,
- CI logs are cited when used,
- review comments are quoted or summarized accurately,
- unresolved comments are not ignored,
- PR summary matches the actual diff,
- push or merge permission is explicit.

The page should require the agent to say when remote PR/CI state cannot be read.

## Stop And Escalate

The page should tell the agent to stop and ask the human when:

- branch or PR identity is unclear,
- target branch is unclear and the action depends on it,
- working tree has unexpected local changes,
- CI status cannot be read but the next action depends on it,
- failure logs are missing, truncated, or ambiguous,
- review comments are subjective, architectural, or contradictory,
- required checks are failing and no bounded fix scope exists,
- push, PR update, merge, remote rerun, deployment, migration, permission change, or external service access is needed but not explicitly authorized,
- changes touch production, authentication, authorization, payment, sensitive data, security, compliance, migrations, or infrastructure,
- project may be Level 4 and risk boundaries are not confirmed,
- the agent cannot explain the recommended next action from evidence.

## Loop Boundary

The page should include a section named `什么时候转成 loop / When This Becomes A Loop`.

It should say:

- Do not start PR / CI work as an automatic loop by default.
- One-shot PR/CI triage is enough when the branch state, CI status, feedback, and verification command are clear.
- A CI-fix loop can be considered only when goal, allowed actions, forbidden actions, verification signal, stop conditions, escalation conditions, remote permissions, and maximum iterations are explicit.
- A review-fix loop can be considered only when review comments are concrete and the maximum fix scope is bounded.
- Use `../loops/README.md` before designing the loop.
- If any loop boundary is missing, continue as human-confirmed single steps.

## Skill Capture Signals

The page should explain that this scenario may become a skill candidate when:

- a project has a stable PR template,
- CI checks and verification commands are stable,
- review comment handling follows a repeated pattern,
- the same push, PR update, merge, or stop/escalate gates recur,
- agents repeatedly need the same PR summary and test plan format,
- the workflow works across several PRs.

Project-level skill capture should come first. System-level skill promotion is only a candidate when the pattern works across multiple projects and has been reviewed by a human.

## Cookbook Entrypoint Update

Update `zh/cookbook/README.md`:

- Add `PR CI Workflow` to current scenarios.
- Remove or adjust the planned scenario bullet for fuller PR / CI workflow.
- Keep multi-agent delegation as a planned scenario.

The entrypoint should not imply that this page is a full GitHub or CI provider manual.

## Related Docs

The new page should link to:

- `../guides/project-levels.md`
- `../guides/infra-by-level.md`
- `../guides/infra-checklists.md`
- `../playbooks/start-a-new-project.md`
- `../loops/README.md`
- `../skill-workflow/README.md`
- `repo-orientation.md`
- `task-handoff.md`
- `review-and-fix.md`
- `context-refresh.md`

## Acceptance Criteria

- `zh/cookbook/pr-ci-workflow.md` exists.
- The page follows the cookbook scenario structure.
- The page includes a copyable PR CI Workflow Template.
- The page includes staged lifecycle guidance.
- The page includes Claude Code and Codex CLI prompts.
- Prompts distinguish read-only PR/CI triage from authorized local fixes.
- Prompts forbid invented commands, tests, CI checks, review comments, deployment flows, team rules, and risk assumptions.
- Prompts do not ask agents to push, update PR metadata, merge, rerun remote checks, deploy, migrate, change permissions, or access external services without explicit authorization.
- Prompts do not ask agents to run automatic loops.
- The page includes verification, stop/escalate, loop boundary, and skill capture sections.
- The page distinguishes itself from repo orientation, task handoff, review-and-fix, context refresh, and loops.
- `zh/cookbook/README.md` links the new scenario as current.
- The planned scenarios list no longer says PR / CI workflow is future work.
- English stub files are unchanged.
- Related links point to existing files.
- Markdown whitespace checks pass.
