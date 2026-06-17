# Multi-Agent Delegation

Use this scenario when a task may be split across multiple agents while keeping scope, ownership, review, integration, and final verification under control.

The goal is not to open more agents by default or let multiple agents repeatedly fix problems on their own. The goal is to use multi-agent delegation only when the work can be decomposed into bounded, independently verifiable slices and a controller can integrate the results.

## Purpose

Multi-agent delegation helps a human or controller agent decide whether parallel work is appropriate and, if it is, assign work with clear ownership boundaries.

After planning, there should be clear agent assignments, owned files or areas, forbidden actions, review plan, integration order, and final verification signal.

## When To Use

Use this scenario when:

- Several subtasks can move independently.
- Files, directories, modules, or research questions have clear ownership.
- Multiple exploration questions can be answered in parallel.
- Implementation and review should be separated.
- A larger task can be sliced into small, safe pieces.
- A controller needs to gather worker outputs before integrating the result.

Do not use it when:

- The task goal is unclear.
- Multiple agents would need to edit the same file frequently.
- Ownership cannot be separated.
- There is no real verification signal.
- The project level is unclear, especially if it may be Level 4.
- The work involves production, permissions, auth, payment, sensitive data, security, compliance, migration, deployment, infra, or external service risk without a human gate.
- You only want agents to automatically loop on failures; read [Loops](../loops/README.md) first.

Multi-agent delegation is not the default strategy. Parallel work is not automatically safer or faster.

## Required Inputs

Provide at least:

- Project level.
- Overall goal.
- Why multi-agent delegation is appropriate.
- Agent roles.
- Each agent's owned files, directories, modules, or question scope.
- Each agent's forbidden files or actions.
- Shared context files.
- Expected output format.
- Verification command or verification signal.
- Integration owner.
- Review owner.
- State record location.
- Human confirmation gates.
- Known risks.

Do not start multi-agent delegation without ownership boundaries.

## Multi-Agent Delegation Template

Copy this template into `STATE.md`, an issue, a PR comment, a task description, or the agent conversation.

```markdown
## Multi-Agent Delegation

- Overall goal:
- Project level:
- Why multi-agent:
- Shared context:
- Agent assignments:
  - Agent name or role:
    - Task goal:
    - Owned files or areas:
    - Context files:
    - Forbidden files or actions:
    - Verification:
    - Completion criteria:
- Owned files or areas:
- Forbidden files or actions:
- Allowed actions:
- Verification command or signal:
- Expected output format:
- Integration owner:
- Review owner:
- State record location:
- Human confirmation gates:
- Known risks:
- Open questions:
```

Fill it carefully:

- `Why multi-agent` must explain why parallel work is better than a single agent for this task.
- `Owned files or areas` must be specific: files, directories, modules, or read-only questions.
- `Forbidden files or actions` must name the areas and actions each agent must avoid.
- Each assignment needs verification, or an explicit note that no real verification signal exists.
- `Integration owner` owns final merge of the outputs and conflict handling.
- `Review owner` owns independent review; worker self-review is not enough.

## Controller Responsibilities

The controller may be a human or a primary agent. The controller owns:

- Deciding whether multi-agent delegation is actually useful.
- Splitting work into independent slices.
- Assigning non-overlapping ownership wherever possible.
- Giving each worker only the context needed for its task.
- Preventing workers from editing the same file unless there is an explicit integration plan.
- Collecting worker outputs.
- Arranging spec compliance review and quality review.
- Handling conflicts.
- Integrating results in a controlled order.
- Running final verification.
- Updating the state record.
- Deciding when to stop / escalate to the human.

The controller is responsible for the final state. Worker reports are evidence, not proof.

## Worker Responsibilities

Each worker must:

- Work only inside its owned scope.
- Avoid reverting or overwriting other agents' changes.
- Stop if the task requires files or actions outside ownership.
- Avoid unauthorized broad refactors.
- Run the assigned verification, or explain why it cannot run.
- Report changed files, key decisions, verification results, residual risks, and questions.
- Avoid completion claims without evidence.

Workers should not push, merge, update PR metadata, deploy, change shared state, migrate, change permissions, or access external services unless the assignment explicitly authorizes it and the human gate has been cleared.

## Subagent Boundaries

Subagents are useful for bounded investigation, implementation slices, focused review, and independent verification.

Subagents should not:

- Decide product direction, architecture direction, review tradeoffs, or unclear ownership questions on their own.
- Expand scope into shared files or adjacent cleanup.
- Touch sensitive data, auth, payment, production, permissions, deployment, migration, infra, or external services without explicit authorization.
- Resolve conflicts by overwriting another worker's output.
- Treat their own report as final integration approval.

When a subagent finds a boundary problem, it should stop and report the exact decision needed.

## Reviewer Responsibilities

Multi-agent delegation needs at least three review perspectives:

- Spec reviewer: checks whether worker output satisfies the assignment and overall plan.
- Quality reviewer: checks clarity, maintainability, risk boundaries, and scope control.
- Final reviewer: checks the integrated result after all worker outputs are combined.

Worker self-review is useful, but it does not replace independent review.

## Claude Code Prompt

```text
Please evaluate whether the Multi-Agent Delegation below should be split across multiple agents, then produce an executable delegation plan.

Requirements:
- First read the overall goal, project level, shared context, and current repo state.
- Decide whether the task is actually suitable for multi-agent delegation. If not, explain why and recommend a single-agent or human-led path.
- If suitable, split the work into independent assignments.
- Each assignment must include task goal, owned files or areas, context files, forbidden files or actions, allowed actions, verification, completion criteria, and expected output.
- Ownership should not overlap. If overlap cannot be avoided, name the integration owner and conflict handling plan.
- State controller, worker, and reviewer responsibilities.
- Require workers not to revert or overwrite other agents' changes.
- Require workers to stay inside owned scope and stop if they need more scope.
- Require each worker to output changed files, verification results, residual risks, and questions.
- Require spec review and quality review before integration; require final verification after integration.
- Do not invent commands, tests, team rules, ownership, or risk assumptions.
- Do not automatically enter a loop.
- Without explicit authorization, do not push, update PR metadata, merge, deploy, migrate, change permissions, or access external services.

Output:
- Whether multi-agent delegation is appropriate
- Decomposition rationale
- Agent assignments
- Owned / forbidden files or areas
- Verification plan
- Review plan
- Integration order
- Conflict handling plan
- Stop / escalate conditions
- State update plan

Multi-Agent Delegation:
<paste the Multi-Agent Delegation template content>
```

## Codex CLI Prompt

```text
Please design delegation from the Multi-Agent Delegation below in the current repo. Do not start multi-agent execution yet.

Requirements:
- First read the shared context, project level, overall goal, and current repo state.
- Decide whether the task is suitable for multi-agent delegation. If ownership cannot be separated or the verification signal is missing, stop and explain.
- If suitable, output a bounded handoff for each worker.
- Each worker handoff must include task goal, owned files or areas, context files, forbidden files or actions, allowed actions, verification, completion criteria, and expected output.
- State that each worker must not revert or overwrite other agents' changes.
- State how the controller will collect outputs, review them, integrate them, and run final verification.
- If the same file must be edited by multiple workers, write the conflict handling plan first. Do not let multiple workers edit it directly at the same time.
- Do not invent commands, tests, team rules, ownership, or risk assumptions.
- Do not automatically loop.
- Without explicit authorization, do not push, update PR metadata, merge, deploy, migrate, change permissions, or access external services.

Output:
- Whether multi-agent delegation is appropriate
- Decomposition rationale
- Worker handoffs
- Review plan
- Integration plan
- Verification plan
- Conflict handling plan
- Stop / escalate conditions
- Questions needing human confirmation

Multi-Agent Delegation:
<paste the Multi-Agent Delegation template content>
```

## Verification Signals

Check each worker output:

- Work stayed inside owned files or areas.
- Forbidden files or actions were not touched.
- Assigned verification ran, or the worker explained why it could not run.
- Changed files, risks, and questions were reported.

Check the integrated result:

- Other workers' changes were not lost.
- The final diff matches the overall goal.
- Conflict resolution has evidence.
- Final verification command or checklist ran.

Common checks:

```bash
git status --short --branch
git diff --check
```

If the project has tests, lint, typecheck, docs checks, or CI, this scenario should use the real project verification signal.

## Stop / Escalate Conditions

The controller must stop and return control to the human when:

- The task cannot be split into independent slices.
- Ownership overlaps and cannot be resolved.
- Two agents need to modify the same file without an integration plan.
- The project level is unclear, especially if it may be Level 4.
- There is no real verification signal.
- Any worker needs expanded scope.
- Any worker reports contradictory assumptions.
- Integration reveals conflicts or unexpected changes.
- The work involves production, permissions, auth, payment, sensitive data, security, compliance, migration, infra, deployment, or external services.
- The next step requires push, PR metadata updates, merge, rerunning remote checks, deployment, migration, permission changes, or external service access without explicit authorization.

## Conflict Handling

Avoid conflicts through ownership assignment first.

If a conflict occurs:

- Stop and read both sides of the change.
- Do not let one worker overwrite another worker's output.
- Have the integration owner resolve the conflict based on evidence.
- Record what was kept, changed, or discarded.
- Rerun relevant verification after conflict resolution.
- Return to the human if the conflict source or resolution cannot be explained.

## Final Human Judgment

Multi-agent output still requires human judgment when the result affects review decisions, product direction, architecture direction, risky commands, remote actions, push, merge, deploy, migration, permission changes, sensitive data, CI ownership, or integration ownership.

The agent may summarize evidence and recommend a path. The human owns unclear tradeoffs and final approval at each human gate.

## Loop Or Skill Capture Signals

Multi-agent delegation is not an automatic loop.

Parallel workers do not imply repeated automatic repair. If delegation appears inside a designed loop, the loop must define:

- Maximum rounds.
- Each agent's ownership.
- Allowed actions.
- Forbidden actions.
- Verification signal.
- Stop conditions.
- Escalation conditions.
- Integration rules.

If those boundaries are unclear, do one delegation round and return control to the controller or human.

Consider a project-level skill when:

- The project repeatedly uses the same agent roles.
- Ownership boundaries are stable.
- Verification and review gates are stable.
- The controller / reviewer workflow repeats across tasks.
- Delegation clearly reduces human coordination cost.
- The same delegation pattern works across multiple PRs or tasks.

Prefer project-level skill capture first. Only record a system-level skill candidate when the delegation pattern works across projects and has human review.

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
- [PR CI Workflow](pr-ci-workflow.md)
