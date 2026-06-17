# Task Handoff

Use this scenario to hand Claude Code or Codex CLI a bounded task.

A good handoff is not "please do this." It states the goal, project level, context, allowed actions, forbidden actions, verification signal, completion criteria, and the points where the agent must hand control back to the human.

## Purpose

Task handoff turns a concrete human request into work instructions an agent can execute safely.

After reading the handoff, the agent should know what to do, what not to do, how to verify the result, and when to stop / escalate.

## When To Hand Off A Bounded Task

Use this scenario when:

- The project already has minimum AI workflow infra.
- The task goal is clear.
- The change can be limited to a small set of files or directories.
- There is a real verification command or another explicit verification signal.
- The human wants the agent to implement the task, or first propose a specific change plan.

Do not use it when:

- The goal is still open-ended exploration.
- The project level or basic infra still needs to be established.
- The task has Level 4 risk without a human gate.
- The work needs an automated loop.
- The task requires broad refactoring, permission changes, data model changes, deployment, or production operations and the boundaries are not written down.

## Required Task Inputs

At minimum, provide:

- Task goal.
- Project level.
- Context files.
- Likely files or directories.
- Allowed actions.
- Forbidden actions.
- Verification command or verification signal.
- Completion criteria.
- State update location.
- Human confirmation gates.

## Handoff Template

Copy this template into an issue, task description, `STATE.md`, or the agent conversation.

```markdown
## Task Handoff

- Task goal:
- Project level:
- Context files:
- Likely files or areas:
- Allowed actions:
- Forbidden actions:
- Verification command or signal:
- Completion criteria:
- State update location:
- Human confirmation gates:
- Known risks:
- Questions before work:
```

Fill it carefully:

- `Task goal` must describe a verifiable result.
- `Allowed actions` must be narrow enough that the agent cannot expand scope on its own.
- `Forbidden actions` must name the boundaries around production, permissions, auth, payment, sensitive data, security, deployment, migrations, and broad refactors.
- `Verification command or signal` must come from the real project. If none exists, write "no real verification signal currently exists."
- `Human confirmation gates` must say which actions require the agent to stop and ask first.

## Scope And Forbidden Actions

The agent should only do what the handoff permits. It should not broaden ownership from one task into general cleanup, unrelated refactoring, dependency changes, remote actions, pushes, merges, deployments, migrations, permission changes, or sensitive data handling.

If the task appears to require any forbidden action, the right behavior is not to improvise. The agent should stop / escalate and return the decision to the human.

## Claude Code Prompt

```text
Please take over the task described in this Task Handoff.

Requirements:
- First read the listed context files and any adjacent files that are necessary to understand the task.
- Before editing, restate your understanding of the task goal, scope, allowed actions, forbidden actions, verification method, and completion criteria.
- If context is missing, the verification signal is missing, or the scope is unclear, ask a question or mark the task blocked before changing files.
- Only perform actions allowed by the handoff.
- Do not perform broad refactors that were not requested.
- Do not invent commands, tests, deployment flows, team rules, or risk assumptions.
- If the work requires dependency installation, deployment, migration, permission changes, or touching auth, payment, production, sensitive data, security, or compliance, stop and ask for human confirmation.
- After the work, run or report the verification result. If verification cannot be run, explain why.
- Finish with a change summary, verification result, residual risks, and suggested state update.

Task Handoff:
<paste the handoff template content>
```

## Codex CLI Prompt

```text
Please execute the following Task Handoff in the current repo.

Requirements:
- First read the context files listed in the handoff and any necessary adjacent files.
- Before editing, restate the task goal, boundaries, forbidden actions, verification method, and completion criteria.
- If context is insufficient, the verification command does not exist, or the project level or risk is unclear, stop and list what needs human confirmation.
- Modify only the files needed for the task.
- Do not perform broad refactors that were not requested.
- Do not invent commands, tests, deployment flows, team rules, or risk assumptions.
- Do not run deploy, migration, permission, auth, payment, production, or sensitive data commands unless the handoff explicitly approves them.
- After the work, run the real verification command. If you cannot run it, explain why and describe any substitute checks.
- Output changed files, verification result, residual risks, and next step.

Task Handoff:
<paste the handoff template content>
```

## Verification Signals

Check that:

- File changes stay inside the handoff scope.
- No forbidden action was taken.
- The verification command really ran, or the agent clearly explained why it could not run.
- The output includes residual risk.
- Any required state update is identified.

Common checks:

```bash
git status --short
git diff --check
```

Expected:

- `git status --short` shows only expected files.
- `git diff --check` has no output.

## Handoff Back To The Human

The agent should hand control back when:

- The project level is unclear, especially if it may be Level 4.
- There is no real verification signal.
- The task needs a wider scope or unauthorized file changes.
- The task needs dependency installation, deployment, migration, permission changes, or external service access.
- The task touches production, permissions, auth, payment, sensitive data, security, or compliance.
- Project documents contradict each other.
- The agent cannot explain the current state or next step.

The handoff back should include changed files, verification results, residual risks, and the exact decision needed from the human.

## Loop Or Skill Capture Signals

Do not turn a task handoff into a loop by default. Consider a loop only when the goal, allowed actions, forbidden actions, verification signal, stop conditions, escalation conditions, and maximum rounds are all explicit.

Consider a project-level skill when:

- Every handoff needs the same fields.
- Agents repeatedly fail on the same unclear boundary.
- A class of tasks has stable inputs, steps, and verification.
- The same stop / escalate rules keep appearing.

If the handoff pattern works across multiple projects, record it as a system-level skill candidate. Do not upgrade it automatically; human review still owns that decision.

## Related Docs

- [Project Levels](../guides/project-levels.md)
- [Infra By Level](../guides/infra-by-level.md)
- [Infra Checklists](../guides/infra-checklists.md)
- [Start A New Project Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
