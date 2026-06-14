# Cookbook Context Refresh Scenario Design

Date: 2026-06-13

## Goal

Add a fourth Chinese cookbook scenario for refreshing context after a long session, interruption, agent switch, or context compaction.

The scenario should help readers ask Claude Code or Codex CLI to reconstruct the current project state before continuing work. The agent should compare the last known goal with the current repository evidence, identify what is complete, what remains, what changed, and what assumptions may be stale. It should then recommend a safe next step without modifying files.

## Scope

Create one scenario page and link it from the cookbook entrypoint:

```text
zh/cookbook/context-refresh.md
zh/cookbook/README.md
```

The new page should follow the existing cookbook style used by:

- `zh/cookbook/repo-orientation.md`
- `zh/cookbook/task-handoff.md`
- `zh/cookbook/review-and-fix.md`

## Non-Goals

This scenario will not:

- Continue implementation after refreshing context.
- Fix tests, lint, review, or CI feedback.
- Create an automatic loop.
- Add scripts or executable automation.
- Add English translation beyond existing English stub files.
- Create the formal `zh/cases/` module.

The scenario may recommend moving into `task-handoff.md` or `review-and-fix.md`, but it should not perform those actions itself.

## Scenario Definition

Create:

```text
zh/cookbook/context-refresh.md
```

Purpose:

Help a human ask an agent to recover working context when the previous conversation, local state, or task history may no longer be fresh.

The scenario should be useful when:

- a long AI coding session was interrupted,
- context was compacted or summarized,
- work is handed from one agent to another,
- the human returns to a repo after a short pause and wants to know whether the previous task is complete,
- there are local changes or recent commits and the next safe action is unclear,
- the agent needs to decide whether to resume, verify, stop, or ask for human confirmation.

It should not be used as permission to modify files.

## Boundary With Existing Cookbook Pages

`context-refresh.md` must make these boundaries explicit:

- Compared with `repo-orientation.md`: context refresh starts from a previous goal or last known state. Repo orientation starts from general project understanding.
- Compared with `task-handoff.md`: context refresh decides whether a task can be resumed or needs a new handoff. Task handoff is for executing a bounded task.
- Compared with `review-and-fix.md`: context refresh may discover failed verification or feedback, but it should hand that to review-and-fix instead of fixing directly.
- Compared with `zh/loops/README.md`: context refresh is not a loop. Repeated context drift may be a signal that a loop needs better state recording, stop conditions, or human review gates.

## Required Sections

The page should include:

- Purpose
- When to Use
- When Not to Use
- Inputs
- Context Refresh Template
- Claude Code Prompt
- Codex CLI Prompt
- Expected Output
- Verification
- Stop and Escalate
- Relationship To Other Cookbook Scenarios
- Loop Boundary
- Skill Capture Signals
- Related Docs

## Inputs

The scenario should ask the human to provide:

- last known goal,
- last completed step,
- current branch or expected branch,
- known changed files or areas,
- last verification command or result,
- state record location such as `STATE.md`, issue, PR, plan, or conversation summary,
- allowed actions,
- forbidden actions,
- human confirmation gates,
- known blockers or concerns.

The page should make clear that missing input is allowed, but the agent must label it as unknown instead of inventing state.

## Context Refresh Template

Include a copyable template with fields:

- Last known goal
- Last completed step
- Expected branch or workspace
- Known changed files or areas
- Last verification command or result
- State record location
- Allowed actions
- Forbidden actions
- Human confirmation gates
- Known blockers or concerns
- Question to answer before resuming

The template should be suitable for `STATE.md`, a task comment, a pull request comment, or a direct prompt.

## Prompt Behavior

Claude Code and Codex CLI prompts should ask the agent to:

- operate in read-only mode,
- read the provided state record and relevant repo files,
- inspect `git status --short --branch`,
- inspect recent commits with `git log --oneline -5`,
- inspect changed files when local changes exist,
- compare current repo evidence against the last known goal,
- identify completed work, incomplete work, changed files, stale assumptions, and risks,
- classify the recommended next action as resume, verify, create a new handoff, handle feedback, stop, or ask the human,
- avoid modifying files,
- avoid installing dependencies,
- avoid running deployment, migration, permission, authentication, payment, production, sensitive data, security, compliance, or external service commands,
- avoid inventing commands, tests, deployment flows, team rules, or risk assumptions.

The prompts should not tell the agent to continue implementation after the refresh.

## Expected Output

The agent output should include:

- current repo state,
- branch and dirty worktree summary,
- last known goal as understood from input,
- evidence for completed work,
- evidence for incomplete work,
- changed files or recent commits,
- stale assumptions or contradictions,
- verification status,
- recommended next action,
- questions requiring human confirmation.

The output should cite files, commits, commands, or explicit user-provided context where possible.

## Verification

Verification should check the refresh itself, not product behavior.

Suggested checks:

- `git status --short --branch`
- `git log --oneline -5`
- `git diff --check` if local changes exist
- `rg` or file reads for referenced state records

The page should ask the human to confirm:

- branch state is correctly reported,
- changed files are not omitted,
- unknowns are marked as unknown,
- the agent did not modify files,
- the recommended next action follows from evidence.

## Stop And Escalate

The page should tell the agent to stop and ask the human when:

- the current branch or workspace does not match the expected one,
- local changes exist and ownership is unclear,
- recent commits contradict the stated last known goal,
- state records conflict with each other,
- verification status is missing or ambiguous,
- the next action would require changing files,
- the next action would require installing dependencies, deployment, migration, permission changes, authentication, payment, production, sensitive data, security, compliance, or external service access,
- the project may be Level 4 and risk boundaries are not confirmed,
- the agent cannot explain the current state from evidence.

## Loop Boundary

The page should state:

- Context refresh is not an automatic loop.
- If context refresh repeatedly happens because an agent loses state, the project may need better `STATE.md`, plan tracking, commit cadence, or stop conditions.
- If context refresh happens inside a designed loop, the loop must define what state is read, what evidence is trusted, what counts as completion, and when the loop stops or escalates.
- If those loop boundaries are not explicit, refresh once and hand control back to the human.

## Skill Capture Signals

The page should explain that this scenario may become a skill candidate when:

- every resumed session needs the same refresh checklist,
- agents repeatedly miss the same state records or branch checks,
- the project has stable state files, branch rules, verification commands, and completion signals,
- context compaction or agent handoff is common,
- the refresh pattern is useful across several tasks.

Project-level skill capture should come first. System-level skill promotion is only a candidate when the pattern works across multiple projects and has been reviewed by a human.

## Cookbook Entrypoint Update

Update `zh/cookbook/README.md`:

- Add `Context Refresh` to current scenarios.
- Remove or adjust the planned scenario bullet for long session or interruption context refresh.
- Keep PR / CI and multi-agent delegation as planned scenarios.

The entrypoint should not imply that context refresh covers full task execution or feedback fixing.

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

## Acceptance Criteria

- `zh/cookbook/context-refresh.md` exists.
- The page follows the cookbook scenario structure.
- The page includes a copyable Context Refresh Template.
- The page includes Claude Code and Codex CLI prompts.
- Prompts require read-only context recovery before any resume decision.
- Prompts forbid invented commands, tests, deployment flows, team rules, and risk assumptions.
- Prompts do not ask agents to continue implementation or run automatic loops.
- The page includes verification, stop/escalate, loop boundary, and skill capture sections.
- The page distinguishes context refresh from repo orientation, task handoff, and review-and-fix.
- `zh/cookbook/README.md` links the new scenario as current.
- English stub files are unchanged.
- Related links point to existing files.
- Markdown whitespace checks pass.
