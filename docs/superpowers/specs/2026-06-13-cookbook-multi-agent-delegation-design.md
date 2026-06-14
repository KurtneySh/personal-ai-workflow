# Cookbook Multi Agent Delegation Scenario Design

Date: 2026-06-13

## Goal

Add a Chinese cookbook scenario for delegating work across multiple agents without losing control of scope, ownership, review, and final verification.

The scenario should help readers decide when multi-agent work is useful, how to split tasks safely, what each agent owns, how the controller collects results, how conflicts are handled, and when the work must stop or return to a human.

## Scope

Create one scenario page and link it from the cookbook entrypoint:

```text
zh/cookbook/multi-agent-delegation.md
zh/cookbook/README.md
```

The new page should follow the existing cookbook style used by:

- `zh/cookbook/repo-orientation.md`
- `zh/cookbook/task-handoff.md`
- `zh/cookbook/review-and-fix.md`
- `zh/cookbook/context-refresh.md`
- `zh/cookbook/pr-ci-workflow.md`

## Non-Goals

This scenario will not:

- Treat multi-agent work as the default.
- Create an automatic loop.
- Replace human judgment for task decomposition.
- Replace `task-handoff.md` for bounded task execution.
- Replace `review-and-fix.md` for feedback fixes.
- Replace `context-refresh.md` for stale or interrupted state recovery.
- Define a provider-specific subagent API.
- Add scripts or executable automation.
- Add English translation beyond existing English stub files.
- Create the formal `zh/cases/` module.

The page may include example prompts, but it should stay platform-neutral and focus on delegation design rather than any single multi-agent tool.

## Scenario Definition

Create:

```text
zh/cookbook/multi-agent-delegation.md
```

Purpose:

Help a human or controller agent decide whether a task should be split across multiple agents, define non-overlapping ownership, dispatch bounded work, review each result, integrate safely, and run final verification.

The scenario should be useful when:

- several tasks can proceed independently,
- different files or modules have clear ownership boundaries,
- exploration questions can be answered in parallel,
- implementation and review should be separated,
- a larger task can be decomposed into small safe slices,
- a controller needs to coordinate several agent outputs before final integration.

## Boundary With Existing Cookbook Pages

`multi-agent-delegation.md` must make these boundaries explicit:

- Compared with `task-handoff.md`: delegation is a coordination layer. Each worker still needs a bounded handoff.
- Compared with `review-and-fix.md`: review or CI findings discovered by any agent should become bounded feedback fixes.
- Compared with `context-refresh.md`: if the controller loses track of branch, ownership, commits, or agent results, it should refresh context before continuing.
- Compared with `pr-ci-workflow.md`: PR / CI workflow may use delegation for independent fixes or reviews, but delegation itself does not authorize push, PR update, or merge.
- Compared with `zh/loops/README.md`: multi-agent delegation is not a loop. Parallel work does not imply repeated automatic execution.
- Compared with `zh/skill-workflow/README.md`: repeated delegation patterns may become a project-level skill candidate, but the agent must not automatically promote them.

## Required Sections

The page should include:

- Purpose
- When to Use
- When Not to Use
- Inputs
- Multi-Agent Delegation Template
- Controller Responsibilities
- Worker Responsibilities
- Reviewer Responsibilities
- Claude Code Prompt
- Codex CLI Prompt
- Expected Output
- Verification
- Stop and Escalate
- Conflict Handling
- Loop Boundary
- Skill Capture Signals
- Related Docs

## Inputs

The scenario should ask the human or controller to provide:

- project level,
- overall goal,
- decomposition rationale,
- agent roles,
- owned files or directories for each agent,
- forbidden files or actions for each agent,
- shared context files,
- expected output format,
- verification command or signal,
- integration owner,
- review owner,
- state record location,
- human confirmation gates,
- known risks.

The page should make clear that multi-agent delegation without ownership boundaries is unsafe.

## Multi-Agent Delegation Template

Include a copyable template with fields:

- Overall goal
- Project level
- Why multi-agent
- Shared context
- Agent assignments
- Owned files or areas
- Forbidden files or actions
- Allowed actions
- Verification command or signal
- Expected output format
- Integration owner
- Review owner
- State record location
- Human confirmation gates
- Known risks
- Open questions

The `Agent assignments` field should encourage a repeated sub-template for each worker:

- Agent name or role
- Task goal
- Owned files or areas
- Context files
- Forbidden files or actions
- Verification
- Completion criteria

## Controller Responsibilities

The page should state that the controller agent or human is responsible for:

- deciding whether multi-agent work is justified,
- decomposing work into independent slices,
- assigning non-overlapping ownership,
- giving each worker only the context needed for its task,
- preventing workers from modifying the same files unless explicitly coordinated,
- collecting worker results,
- running spec compliance and quality review,
- resolving conflicts,
- integrating results in a controlled order,
- running final verification,
- updating state records,
- deciding whether to stop or escalate.

The controller remains accountable for final state. Worker reports are evidence, not proof.

## Worker Responsibilities

The page should state that each worker must:

- work only inside owned scope,
- avoid reverting or overwriting other agents' work,
- stop if the task needs files or actions outside its ownership,
- avoid broad refactors unless explicitly assigned,
- run the assigned verification or report why it cannot run,
- summarize changed files, decisions, verification, residual risk, and questions,
- avoid claiming success without evidence.

Workers should not merge, push, update PR metadata, deploy, or change shared state unless explicitly authorized.

## Reviewer Responsibilities

The page should define reviewer roles:

- spec reviewer checks whether a worker output satisfies its assignment and the broader plan,
- quality reviewer checks clarity, maintainability, safety, and scope control,
- final reviewer checks the integrated result across all workers.

The page should make clear that self-review by a worker does not replace independent review.

## Prompt Behavior

Claude Code and Codex CLI prompts should ask the controller to:

- inspect the task goal and repo state,
- decide whether multi-agent delegation is appropriate,
- propose assignments with non-overlapping ownership,
- include allowed actions, forbidden actions, verification, output format, and stop conditions per worker,
- avoid dispatching agents on vague or overlapping work,
- require worker outputs to cite files, commands, and verification results,
- require review before integration,
- stop when ownership, risk, verification, or project level is unclear.

Worker prompts should ask the worker to:

- restate assignment scope,
- modify only owned files,
- avoid reverting other work,
- stop when scope expands,
- run verification,
- report files changed, verification, risks, and questions.

The prompts should not ask agents to run automatic loops or perform remote/destructive actions.

## Expected Output

The controller output should include:

- whether multi-agent delegation is appropriate,
- decomposition rationale,
- agent assignments,
- owned and forbidden files or areas,
- verification plan,
- integration order,
- review plan,
- stop/escalation conditions,
- state update plan.

Worker outputs should include:

- task understanding,
- changed files or read-only findings,
- verification commands and results,
- residual risks,
- questions or blockers,
- recommended state update.

## Verification

Verification should cover both individual worker outputs and the integrated result.

Suggested checks:

- each worker stayed within owned files or areas,
- no worker touched forbidden files or actions,
- worker verification was run or failure to run was explained,
- integration did not drop another worker's changes,
- final diff matches the overall goal,
- final verification command or checklist was run,
- `git status --short --branch`,
- `git diff --check`,
- project-specific tests or docs checks when available.

The page should require the controller to verify rather than trust worker reports.

## Stop And Escalate

The page should tell the controller to stop and ask the human when:

- the task cannot be decomposed into independent slices,
- ownership overlaps and cannot be resolved,
- two agents need to modify the same file without a clear integration plan,
- project level is unclear or may be Level 4,
- verification signal is missing,
- any worker needs to expand scope,
- any worker reports conflicting assumptions,
- integration creates conflicts or unexpected changes,
- work touches production, authentication, authorization, payment, sensitive data, security, compliance, migrations, infrastructure, deployment, permissions, or external services,
- push, PR update, merge, remote rerun, deploy, migration, permission change, or external access is needed without explicit authorization.

## Conflict Handling

The page should include practical conflict handling:

- prefer avoiding conflicts through ownership assignment,
- if conflicts happen, stop and inspect both sides before editing,
- do not let one worker blindly overwrite another worker's changes,
- ask the integration owner to resolve conflicts with evidence,
- rerun relevant verification after conflict resolution,
- record what was kept, changed, or discarded.

## Loop Boundary

The page should state:

- Multi-agent delegation is not an automatic loop.
- Running several agents in parallel does not authorize repeated fix attempts.
- If delegation happens inside a loop, the loop must define maximum rounds, per-agent ownership, verification, stop conditions, escalation conditions, and integration rules.
- If those boundaries are missing, run one delegation pass and return to a human or controller decision.

## Skill Capture Signals

The page should explain that this scenario may become a skill candidate when:

- a project repeatedly uses the same agent roles,
- ownership boundaries are stable,
- verification and review gates are stable,
- the same controller/reviewer workflow repeats across tasks,
- repeated delegation reduces human coordination cost,
- the workflow works across several tasks or PRs.

Project-level skill capture should come first. System-level skill promotion is only a candidate when the pattern works across multiple projects and has been reviewed by a human.

## Cookbook Entrypoint Update

Update `zh/cookbook/README.md`:

- Add `Multi-Agent Delegation` to current scenarios.
- Remove or adjust the planned scenario bullet for multi-agent delegation.
- Remove the planned scenarios section if no planned cookbook scenarios remain, or replace it with a short note that future scenarios will be added after real usage.

The entrypoint should not imply multi-agent delegation is always recommended.

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
- `pr-ci-workflow.md`

## Acceptance Criteria

- `zh/cookbook/multi-agent-delegation.md` exists.
- The page follows the cookbook scenario structure.
- The page includes a copyable Multi-Agent Delegation Template.
- The page defines controller, worker, and reviewer responsibilities.
- The page includes Claude Code and Codex CLI prompts.
- Prompts require non-overlapping ownership and explicit forbidden actions.
- Prompts tell workers not to revert or overwrite other agents' work.
- Prompts do not ask agents to run automatic loops.
- Prompts do not authorize push, PR update, merge, deployment, migration, permission changes, or external service access without explicit authorization.
- The page includes verification, stop/escalate, conflict handling, loop boundary, and skill capture sections.
- The page distinguishes itself from task handoff, review-and-fix, context refresh, PR CI workflow, loops, and skill workflow.
- `zh/cookbook/README.md` links the new scenario as current.
- The planned scenarios section no longer lists multi-agent delegation as future work.
- English stub files are unchanged.
- Related links point to existing files.
- Markdown whitespace checks pass.
