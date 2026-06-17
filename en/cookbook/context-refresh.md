# Context Refresh

Use this scenario after a long session, interruption, agent switch, or context compaction.

The goal is not to keep implementing or quietly fix problems. The goal is to recover the current state: what the last goal was, where the repo stands now, what is already done, which assumptions may be stale, and whether the next step is safe.

## Purpose

Context refresh gives the human a read-only state recovery summary before deciding whether to resume, verify, hand off, handle feedback, stop, or escalate.

The expected output is a checkable status summary, not a file change.

## When To Use

Use this scenario when:

- A long AI coding session was interrupted.
- Conversation context was compacted or summarized.
- Work is moving from one agent to another.
- A human is returning to the repo after time away.
- There are local changes or recent commits, but the next step is unclear.
- You need to decide whether to resume, verify, create a new handoff, handle feedback, stop, or ask the human.

Do not use it when:

- This is the first time entering an unfamiliar repo; use [Repo Orientation](repo-orientation.md).
- There is already a clear new implementation task; use [Task Handoff](task-handoff.md).
- There is concrete test, lint, review, or CI feedback; use [Review And Fix](review-and-fix.md).
- You want the agent to continue implementation immediately.
- You are designing an automated loop, but the loop boundaries are not written down.
- The project may involve Level 4 risk and no human gate has confirmed the boundary.

This scenario does not grant permission to modify files.

## State To Recover

A useful refresh should recover:

- Last known goal and the evidence for it.
- Last completed step, including commit, verification, review, or state update.
- Expected branch or workspace.
- Current branch, dirty worktree state, changed files, and recent commits.
- Last known verification signal and whether it is current.
- State records such as `STATE.md`, an issue, a PR, a plan, or a conversation summary.
- Allowed actions, forbidden actions, ownership boundaries, and human gate decisions.
- Unknowns, contradictions, stale assumptions, and blockers.

If information is missing, the agent should mark it unknown. It should not invent commands, tests, deployment flows, team rules, ownership, or risk assumptions.

## Files And Signals To Read

Ask the agent to read only the real materials needed for recovery:

- The provided state record, plan, issue, PR, or conversation summary.
- `README.md`, `STATE.md`, `SPEC.md`, `AGENTS.md`, or similar files when they exist and are relevant.
- The current branch and worktree state with `git status --short --branch`.
- Recent commits with `git log --oneline -5`.
- Local diffs when the worktree is dirty.
- Verification command output or CI / review signals only when they are available from real logs or user-provided context.

The agent should treat stale summaries as clues, not proof. Recent commits, current diff, and explicit user instructions are stronger evidence than an old plan checkbox.

## Context Refresh Template

Copy this template into `STATE.md`, an issue, a PR comment, a task description, or the agent conversation.

```markdown
## Context Refresh

- Last known goal:
- Last completed step:
- Expected branch or workspace:
- Known changed files or areas:
- Last verification command or result:
- State record location:
- Allowed actions:
- Forbidden actions:
- Human confirmation gates:
- Known blockers or concerns:
- Question to answer before resuming:
```

Fill it carefully:

- `Last known goal` should describe a verifiable result, not just "continue the project."
- `Last completed step` should name the last confirmed action, commit, verification signal, or review result.
- `Expected branch or workspace` helps catch work happening in the wrong place.
- `Allowed actions` should usually be read-only checks.
- `Forbidden actions` should explicitly rule out file edits, dependency installation, deployment, migration, permission changes, production access, auth, payment, sensitive data, security, and compliance work.
- `Question to answer before resuming` should name the most important decision, such as whether the previous cookbook change was committed and pushed.

## Claude Code Prompt

```text
Please perform a read-only context refresh from the Context Refresh below. Do not modify files.

Requirements:
- First read the provided state record and any relevant README, STATE, SPEC, AGENTS, plan, issue, PR, or conversation summary.
- Check the current branch and worktree: git status --short --branch.
- Check recent commits: git log --oneline -5.
- If local changes exist, read the diff or relevant files only to understand state. Do not modify anything.
- Compare the repo state with Last known goal and identify what is complete, what is incomplete, and which assumptions may be stale.
- Flag contradictions between docs, commits, worktree state, PR / issue state, and user-provided context.
- Recommend the next step as one of: resume / verify / create new handoff / handle feedback / stop / ask human.
- Base claims only on real files, command output, commits, and user-provided context.
- Do not invent commands, tests, deployment flows, team rules, ownership, or risk assumptions.
- Do not modify files, install dependencies, deploy, migrate, change permissions, touch auth, payment, production, sensitive data, security, compliance, or external services.
- Do not continue implementation. Output only the context refresh result and next-step recommendation.

Output:
- Current repo state
- Branch and dirty worktree summary
- Your understanding of the last goal
- Completed items and evidence
- Incomplete items and evidence
- Changed files or recent commits
- Stale assumptions, contradictions, or unknowns
- Verification status
- Recommended next step
- Questions needing human confirmation

Context Refresh:
<paste the context refresh template content>
```

## Codex CLI Prompt

```text
Please perform a read-only context refresh in the current repo from the Context Refresh below.

Requirements:
- Do not modify files.
- Read the state record, README, STATE, SPEC, AGENTS, plan, issue, PR, or conversation summary that exists and is relevant.
- Run or inspect: git status --short --branch.
- Run or inspect: git log --oneline -5.
- If local changes exist, use git diff or file reads only to understand state. Do not edit.
- Compare the repo state with Last known goal and identify completed work, incomplete work, risks, unknowns, and stale assumptions.
- If branch, worktree, state record, recent commits, or user description contradict each other, stop at read-only analysis and list the human questions.
- Base claims only on real files, command output, commits, and user-provided context.
- Do not invent commands, tests, deployment flows, team rules, ownership, or risk assumptions.
- Do not install dependencies or run deploy, migration, permission, auth, payment, production, sensitive data, security, compliance, or external service commands.
- Do not continue implementation or automatically fix anything. Output only the recovery result and next-step recommendation.

Output:
- Current repo state
- Branch and dirty worktree summary
- Last goal
- Completed items and evidence
- Incomplete items and evidence
- Changed files or recent commits
- Stale assumptions, contradictions, or unknowns
- Verification status
- Recommended next step: resume / verify / create new handoff / handle feedback / stop / ask human
- Questions needing human confirmation

Context Refresh:
<paste the context refresh template content>
```

## Verification Signals

This scenario verifies the refresh itself, not product behavior.

Common checks:

```bash
git status --short --branch
git log --oneline -5
```

If local changes exist, also consider:

```bash
git diff --check
```

Check that:

- Branch state is correct.
- Changed files are not omitted.
- Recent commits are not misread.
- Unknown information is explicitly marked unknown.
- The recommended next step is supported by evidence.
- No files were modified.

## Stop / Escalate Conditions

The agent must stop and return control to the human when:

- The current branch or workspace does not match the expected one.
- Local changes exist, but ownership is unclear or permission to continue is unclear.
- Recent commits contradict the last known goal.
- State records, plans, README, issue, PR, or user description contradict each other.
- Verification status is missing or vague.
- The next step requires file modification.
- The next step requires dependency installation, deployment, migration, permission changes, or external service access.
- The repo touches production, permissions, auth, payment, sensitive data, security, or compliance.
- The project may be Level 4 and the risk boundary has not been confirmed.
- The agent cannot explain the current state from evidence.

## Avoiding Stale Assumptions

Do not let an old summary override current evidence. A good context refresh should:

- Compare the state record with `git status --short --branch` and recent commits.
- Treat unchecked assumptions as unknown.
- Distinguish completed work from intended work.
- Distinguish local verification from remote CI or review status.
- Name any human gate that must be cleared before push, merge, deploy, migration, permission change, or sensitive data work.
- Prefer a new handoff when the recovered state is clear but the next implementation scope is not.

## Loop Or Skill Capture Signals

Do not turn context refresh into a loop by default.

A single refresh is enough when the goal, branch, state record, and verification signal line up and the next step can be classified as resume, verify, handoff, handle feedback, stop, or ask human.

If context refresh keeps recurring, check whether:

- `STATE.md` records real progress.
- Plans include current checkbox state.
- Commits are small enough to recover from.
- Stop conditions are clear.
- Agent handoff notes identify the context entry points.

Consider a project-level skill when the same refresh checklist, state files, branch rules, verification signals, and output format are repeatedly useful. Only record a system-level skill candidate when the pattern works across multiple projects and has human review.

## Related Docs

- [Project Levels](../guides/project-levels.md)
- [Infra By Level](../guides/infra-by-level.md)
- [Infra Checklists](../guides/infra-checklists.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
- [Repo Orientation](repo-orientation.md)
- [Task Handoff](task-handoff.md)
- [Review And Fix](review-and-fix.md)
