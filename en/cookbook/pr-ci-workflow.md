# PR CI Workflow

Use this scenario to organize Claude Code or Codex CLI collaboration across a PR and CI lifecycle.

The goal is not to turn the agent into a GitHub or CI autopilot. The goal is to keep PR preparation, CI reading, review handling, status updates, and merge readiness grounded in real evidence, explicit permission, and verifiable results.

## Purpose

PR CI Workflow separates the parts of PR and CI work that an agent can safely judge or execute from the parts that remain human gates.

After reading the workflow, the agent should know whether the current branch or PR is ready for a summary, waiting on CI, blocked by feedback, due for context refresh, ready for a merge-readiness check, or should stop / escalate.

## When To Use

Use this scenario when:

- Preparing a branch to become a PR.
- Drafting or updating a PR summary, test plan, risk notes, or reviewer notes.
- Checking whether a branch is ready to push.
- Reading CI status or failure logs.
- Turning a CI failure into a bounded feedback fix.
- Turning a review comment into a bounded feedback fix.
- Deciding whether a PR is ready for human review or merge readiness.
- PR work was interrupted, moved to another agent, or may have stale context.

Do not use it when:

- This is the first time understanding an unfamiliar repo; use [Repo Orientation](repo-orientation.md).
- The task is a concrete implementation change; use [Task Handoff](task-handoff.md).
- There is already concrete test, lint, typecheck, review, or CI feedback; use [Review And Fix](review-and-fix.md).
- Long-session state is unclear; start with [Context Refresh](context-refresh.md).
- You want an automatic CI-fix loop but have not designed loop boundaries.
- The task requires push, PR metadata updates, merge, rerunning remote checks, deployment, migration, permission changes, or external service access without explicit authorization.
- The work may involve Level 4 risk without a human gate.

This scenario is not a GitHub, GitLab, CI provider, or `gh` command reference.

## Required Inputs

Provide at least:

- Project level.
- Branch or PR identifier.
- Target branch.
- PR goal or change summary.
- Changed files or directories.
- Required checks or verification commands.
- Current CI status or logs.
- Review comments.
- Allowed actions.
- Forbidden actions.
- Push permission.
- PR update permission.
- Merge permission.
- Human confirmation gates.
- State record location.

If branch state, diff scope, CI status, review status, or verification signal is missing, the task is incomplete. The agent should mark unknowns instead of inventing them.

## PR CI Workflow Template

Copy this template into `STATE.md`, an issue, a PR comment, a task description, or the agent conversation.

```markdown
## PR CI Workflow

- Workflow goal:
- Project level:
- Branch or PR:
- Target branch:
- Change summary:
- Changed files or areas:
- Required checks:
- Current CI status or logs:
- Review comments:
- Allowed actions:
- Forbidden actions:
- Push permission:
- PR update permission:
- Merge permission:
- Human confirmation gates:
- State record location:
- Open questions:
```

Fill it carefully:

- `Workflow goal` should say whether this is PR preparation, CI reading, review handling, PR update, or pre-merge check.
- `Branch or PR` and `Target branch` help confirm the agent is working in the right branch or workspace.
- `Required checks` must come from the real project. If none are known, write "no real required checks currently known."
- `Current CI status or logs` should name the specific failing check or log when available, not just "CI failed."
- `Allowed actions` and `Forbidden actions` must distinguish read-only checks, local edits, push, PR update, merge, and remote actions.
- `Push permission`, `PR update permission`, and `Merge permission` should default to no unless the human explicitly grants them.

## Workflow Stages

### 1. Pre-PR Readiness

Confirm current branch, dirty worktree, diff scope, recent commits, verification commands, and residual risk.

Useful outputs:

- Whether a PR summary can be prepared.
- Whether local uncommitted changes remain.
- Whether a verification signal is missing.
- Whether [Context Refresh](context-refresh.md) should happen first.

### 2. PR Summary And Test Plan

Ask the agent to draft only from real diff, commits, verification results, and user-provided context:

- Summary.
- Test plan.
- Risk notes.
- Reviewer notes.
- Known limitations.

The agent must not claim tests, CI checks, or review outcomes that have not happened.

### 3. CI Status Triage

Classify CI status as:

- `passing`
- `failing with logs`
- `pending`
- `unavailable`
- `unknown`

If remote CI cannot be read, the agent should say that directly. It should not assume CI passed or failed.

### 4. CI Failure Handling

If the failure log is specific, convert it into the [Review And Fix](review-and-fix.md) `Feedback Fix Template`.

Do not enter an unlimited repair loop. After one bounded fix, if the same failure repeats, stop / escalate or re-evaluate loop boundaries with a human.

### 5. Review Handling

Convert clear, actionable review comments into bounded feedback fixes.

If review comments are subjective preferences, architecture direction, product decisions, or contradictory requests, ask the human to decide. Unclear review decisions are human gates.

### 6. Context Refresh

Use [Context Refresh](context-refresh.md) first when:

- PR work was interrupted.
- The agent changed.
- Branch, commit, CI, or review state is unclear.
- Local changes or recent commits have unclear ownership.

### 7. Merge Readiness

Before merge, confirm:

- Current branch and target branch are correct.
- Required checks are clearly passing, failing, pending, unavailable, or unknown.
- Review comments are handled or intentionally left unresolved by human decision.
- Local diff and PR summary match.
- Merge permission is explicitly granted by the human.
- No unconfirmed production, permission, auth, payment, sensitive data, security, compliance, migration, infra, or deployment risk remains.

## Claude Code Prompt

```text
Please evaluate the PR / CI workflow described in the PR CI Workflow below.

Requirements:
- First read the provided branch, PR, state record, README, STATE, SPEC, AGENTS, CI config, review comments, or logs.
- Check the current branch and worktree: git status --short --branch.
- Check recent commits: git log --oneline -5.
- Check local diff scope. If local changes exist, use diff and file reads to understand them; do not modify files unless explicitly allowed.
- If PR description, CI status, or review comments are available, summarize only their real contents.
- Classify the current stage: pre-PR readiness / PR summary / CI triage / CI failure handling / review comment handling / context refresh / pre-merge check.
- Recommend the next step as one of: prepare PR summary / push after verification / wait for CI / handle feedback / refresh context / stop / ask human.
- If concrete CI, test, lint, typecheck, or review feedback needs repair, output a Feedback Fix suitable for Review And Fix instead of starting an unlimited fix loop.
- Base claims only on real files, command output, commits, CI status, review comments, and user-provided context.
- Do not invent commands, tests, CI checks, review comments, deployment flows, team rules, ownership, or risk assumptions.
- Without explicit authorization, do not push, update PR metadata, merge, rerun remote checks, deploy, migrate, change permissions, or access external services.
- Do not touch production, permissions, auth, payment, sensitive data, security, or compliance boundaries unless the workflow explicitly authorizes it and includes a human gate.
- Do not automatically enter a loop. If a loop seems needed, output only the loop judgment and the human-confirmed boundaries required.

Output:
- Current branch / PR state
- Changed files or changed areas
- Current workflow stage
- Verification commands or required checks
- CI status classification
- Review status classification
- Recommended next step
- PR summary / test plan / risk notes, if needed
- Feedback Fix draft, if needed
- Risks, unknowns, and questions needing human confirmation

PR CI Workflow:
<paste the PR CI Workflow template content>
```

## Codex CLI Prompt

```text
Please evaluate the following PR CI Workflow in the current repo.

Requirements:
- First read the state record, branch, PR information, CI logs, review comments, and necessary project context listed in the workflow.
- Check the current branch and worktree: git status --short --branch.
- Check recent commits: git log --oneline -5.
- Check local diff scope. If local changes exist, do not modify them unless explicitly allowed.
- If remote PR or CI state cannot be read, say remote state unavailable. Do not assume.
- Classify the current stage: pre-PR readiness / PR summary / CI triage / CI failure handling / review comment handling / context refresh / pre-merge check.
- Recommend the next step: prepare PR summary / push after verification / wait for CI / handle feedback / refresh context / stop / ask human.
- If concrete feedback needs repair, organize it as Review And Fix input instead of automatically fixing it.
- Base claims only on real files, command output, commits, CI status, review comments, and user-provided context.
- Do not invent commands, tests, CI checks, review comments, deployment flows, team rules, ownership, or risk assumptions.
- Without explicit authorization, do not push, update PR metadata, merge, rerun remote checks, deploy, migrate, change permissions, or access external services.
- Do not automatically loop. If a loop is needed, output only loop boundaries and questions needing human confirmation.

Output:
- Branch / PR state
- Changed files or changed areas
- Workflow stage
- Verification commands or required checks
- CI status classification
- Review status classification
- Recommended next step
- PR summary / test plan / risk notes, if this workflow goal needs them
- Feedback Fix draft, if concrete feedback is found
- Risks, unknowns, and questions needing human confirmation

PR CI Workflow:
<paste the PR CI Workflow template content>
```

## Verification Signals

Local verification can include:

```bash
git status --short --branch
git diff --check
git log --oneline -5
```

Project verification commands must come from README, package scripts, CI config, a task handoff, or human-provided context.

PR / CI verification should confirm:

- Required checks are passing, failing, pending, unavailable, or unknown.
- CI logs include a concrete failure signal when used.
- Review comments are accurately summarized, including unresolved comments.
- PR summary matches the actual diff.
- Push, PR update, and merge permission are explicit.
- Remote PR / CI state is clearly marked unavailable when it cannot be read.

## Stop / Escalate Conditions

The agent must stop and return control to the human when:

- Branch or PR identity is unclear.
- Target branch is unclear and the next step depends on it.
- The working tree has unexpected local changes.
- CI state cannot be read and the next step depends on it.
- Failure logs are missing, truncated, or vague.
- Review comments are subjective preferences, architecture direction, product decisions, or contradictory.
- Required checks fail, but the repair scope is not bounded.
- The next step requires push, PR metadata updates, merge, rerunning remote checks, deployment, migration, permission changes, or external service access without explicit authorization.
- The change involves production, permissions, auth, payment, sensitive data, security, compliance, migration, deployment, or infra risk.
- The project may be Level 4 and the risk boundary has not been confirmed.
- The agent cannot explain the recommended next step from evidence.

## Loop Or Skill Capture Signals

Do not make PR / CI work an automatic loop by default.

One-time PR / CI triage is enough when branch state, CI status, feedback, verification signal, and push / PR update / merge permissions are clear.

Only consider a CI-fix loop when all of these are explicit:

- Goal.
- Allowed actions.
- Forbidden actions.
- Verification signal.
- Stop conditions.
- Escalation conditions.
- Remote action permissions.
- Maximum rounds.

Review-fix loops are only appropriate for concrete, actionable, narrow review comments. Read [Loops](../loops/README.md) before designing one. If any boundary is unclear, continue as a human-confirmed single step.

Consider a project-level skill when the same PR template, CI checks, review handling pattern, stop / escalate gates, and PR summary / test plan format repeat across multiple PRs. Only record a system-level skill candidate when the workflow works across projects and has human review.

## Related Docs

- [Project Levels](../guides/project-levels.md)
- [Infra By Level](../guides/infra-by-level.md)
- [Infra Checklists](../guides/infra-checklists.md)
- [Start A New Project Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
- [Repo Orientation](repo-orientation.md)
- [Task Handoff](task-handoff.md)
- [Review And Fix](review-and-fix.md)
- [Context Refresh](context-refresh.md)
