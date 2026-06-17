# Repo Orientation

Use this scenario when Claude Code or Codex CLI needs to understand an existing repo before it edits files.

The goal is not immediate implementation. The goal is a checkable repo orientation: what the project does, what state it is in, which files matter, which verification commands are real, and which risks or unknowns need human confirmation.

## Purpose

Repo orientation gives the human a read-only project summary before deciding whether to hand off a concrete task.

The expected output is an orientation summary the human can inspect, not a file change.

## When To Use

Use this scenario when:

- You are entering an unfamiliar repo for the first time.
- You are returning to a project after time away.
- A new agent needs project context.
- You want orientation before a task handoff.

Do not use it when:

- You already know exactly what files need to change and have enough context.
- You want the agent to execute an edit immediately.
- You are designing an automated loop.
- The project may involve Level 4 risk and the human has not confirmed the risk boundary.

This scenario does not grant permission to modify files.

## Context The Agent Must Read

Ask the agent to read real project materials before making claims:

- The main README and any state document such as `STATE.md`.
- Existing specs, agent instructions, or contributor guidance such as `SPEC.md`, `AGENTS.md`, or similar files.
- Package, build, test, and config files.
- Scripts that reveal verification commands or operational workflows.
- The main directory structure and the files most relevant to the question.

If the agent cannot find a piece of information, it should mark it as unknown instead of guessing. It should not invent commands, tests, deployment flows, team rules, or risk assumptions.

## Inputs

Provide as much of this as possible:

- One-sentence project background or goal.
- The questions you want answered.
- Known project level, or a request for the agent to judge the level and explain the evidence.
- Files or directories to prioritize.
- Forbidden actions, especially no file edits, no dependency installation, and no deploy, migration, permission, auth, payment, production, or sensitive data commands.

## Claude Code Prompt

```text
Please do repo orientation only. Do not modify files.

Read the README, state documents, specs, agent instructions, package/config files, scripts, and main directory structure that exist in this repo.

Requirements:
- Base the summary only on real files and commands.
- Judge the project level, Level 1-4. If there is not enough evidence, say "uncertain" and list what a human must confirm.
- Summarize the project goal, directory structure, current state, key conventions, available verification commands, risks, and unknowns.
- If README, STATE, SPEC, AGENTS, or similar documents conflict, list the conflicts.
- Do not invent commands, tests, deployment flows, team rules, or risk assumptions.
- Do not modify files, install dependencies, or run deploy, migration, permission, auth, payment, production, or sensitive data commands.

Output:
- One-sentence project summary
- Project level judgment and evidence
- Key files and directories
- Current state
- Available verification commands and where you found them
- Risks and unknowns
- Recommended next step
```

## Codex CLI Prompt

```text
Please perform a read-only orientation in the current repo.

Read README.md, STATE.md, and any existing SPEC.md, AGENTS.md, package/config files, scripts, and main directory structure.

Requirements:
- Use only information that really exists in the repo.
- Do not modify files.
- Do not invent commands, tests, deployment flows, team rules, or risk assumptions.
- If you cannot find verification commands, say that clearly.
- If the project level is uncertain, list what a human needs to confirm.
- Do not run deploy, migration, permission, auth, payment, production, or sensitive data commands.

Output:
- Repo overview
- Project level judgment
- Key files and directories
- Current state
- Real available verification commands
- Documentation conflicts or gaps
- Risks and questions for human confirmation
- Recommended next step
```

## Expected Output

A useful orientation includes:

- A one-sentence project summary.
- Project level judgment and evidence.
- Key files, directories, and scripts.
- Current state and obvious gaps.
- Real verification commands.
- Risks, unknowns, and questions for human confirmation.
- A recommended next step.

## Verification And Stop Conditions

Check the agent output against the repo:

- It references real files.
- Recommended commands exist.
- Missing information is marked unknown.
- Guesses are not presented as facts.
- No files were modified.

You can run:

```bash
git status --short
```

Expected: a read-only orientation leaves no file changes.

Stop / escalate to a human when:

- The project may be Level 4.
- The repo touches production, permissions, auth, payment, sensitive data, security, or compliance.
- Project documents contradict each other.
- No real verification command can be found.
- The agent wants to install dependencies, modify files, deploy, migrate, call external services, or change permissions.
- The agent cannot cite where its claims came from.

## When This May Become A Skill

Consider a project-level skill when the same orientation flow repeats:

- Every repo entry requires the same orientation steps.
- Agents repeatedly miss the same critical files.
- The orientation output needs a fixed shape for later tasks.
- The project has stable verification commands, risk boundaries, and state-recording rules.

If the same orientation pattern works across multiple projects, record it as a system-level skill candidate. Do not upgrade it automatically; human review still owns that decision.

## Related Docs

- [Project Levels](../guides/project-levels.md)
- [Infra By Level](../guides/infra-by-level.md)
- [Infra Checklists](../guides/infra-checklists.md)
- [Start A New Project Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
