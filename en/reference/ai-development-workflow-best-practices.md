---
title: AI Development Workflow Best Practices
created: 2026-06-12
updated: 2026-06-12
tags: [ai, workflow, agents, productivity]
---

# AI Development Workflow Best Practices

> Current conclusion: do not treat AI as a chat box, and do not start by chasing full automation. First make the project AI-readable, executable, and verifiable; then turn repeated and verifiable parts into Skills / Loops.

This document is the working philosophy entrypoint for this repository. It does not try to record every source. Instead, it distills a set of AI workflow principles that can guide real development work.

## 1. Main Entrypoint: Move From Chat Boxes To Local Agentic Tools

The main development entrypoint should move from web chat boxes to local agentic tools that can read repositories, edit files, run commands, and inspect errors, such as Claude Code, Codex CLI, and Cursor.

The problem with chat boxes is not that the model is not powerful enough. The problem is that humans must carry information back and forth between AI and the execution environment. A more efficient approach is to let the agent enter the engineering environment directly and complete the loop of "read code -> modify -> run -> read errors -> modify again -> verify."

Practical principles:

- Start the agent inside the repo by default instead of copying code snippets into a chat box.
- Let the agent access code, tests, logs, scripts, documentation, and error output.
- Humans define goals, constraints, acceptance criteria, and key judgments; they should not act as middleware for copying and pasting errors.

## 2. Project Shape: Build A Context Workbench For AI

Put project materials, code, designs, logs, tasks, and rules into an AI-readable workspace that can be diffed and versioned.

Recommended structure:

```text
project/
├── AGENTS.md           # agent operating contract and project rules
├── SPEC.md             # high-level goals, boundaries, product judgment
├── STATE.md            # current state, next steps, blockers, lessons
├── skills/             # reusable working methods
├── docs/               # designs, research, decision records
├── src/
├── tests/
├── scripts/            # deterministic tools agents can call
└── logs/               # feedback material agents can read
```

The core point is not the directory shape itself. The core point is to keep context stable in the filesystem. Files are more controllable than chat memory, and they are better suited for review, reuse, and version management.

## 3. Build The Feedback Loop Before Automation

Before automation, make sure the agent can verify its own output. Tests, lint, builds, logs, traces, CLIs, and browser debugging are more fundamental than complex prompts.

Practical principles:

- Prepare one-command verification for each project: `test`, `lint`, `typecheck`, and `build`.
- Task descriptions must include success criteria.
- Capabilities that can be turned into a CLI should become a CLI first; the GUI should be a human interface, not the agent's only entrypoint.
- Have the agent actively run verification commands before ending a task and report the results.

## 4. Skills Are The Real Compounding Asset

Do not re-prompt repeated tasks from scratch every time. Turn success criteria, real failure modes, and tool usage into Skills.

A good Skill should include at least:

- Applicable scenarios: when it should be used.
- Success criteria: what counts as completion.
- Working steps: the recommended execution order.
- Common pitfalls: failure modes from real tasks.
- Callable tools: commands, scripts, checks, and reference files.
- Stop conditions: when to ask a human for judgment.

Practical principles:

- After completing a high-value task, add a summary prompt: "Turn this workflow into a Skill so it can be reused next time."
- Do not write Skills as long SOPs. Make boundaries, acceptance criteria, and key tools clear.
- Maintain `skills/index.md` so agents can discover which Skill to use for a task.

## 5. Loops Are Not The Default Option

Do not turn everything into a background loop. A task is worth turning into a loop only when it is repeated, automatically verifiable, within budget, and supported by agent tools.

Good candidates for early loops:

- CI failure triage;
- dependency upgrade PRs;
- lint-and-fix;
- flaky test reproduction;
- issue-to-PR work under strong test coverage.

Poor candidates for early loops:

- architecture rewrites;
- authentication / payments;
- production deployments;
- vague product exploration;
- tasks where completion depends mainly on subjective judgment.

A loop must have hard stop conditions, such as maximum iterations, maximum time, maximum tokens, consecutive no-progress attempts, and risk points that require human confirmation.

## 6. Design Boundaries Before Using Multiple Agents

The key to multi-agent work is not the number of agents. It is context isolation, permission boundaries, ownership, and merge / reduce. Without a convergence mechanism, multiple agents only produce multiple opinions.

Practical principles:

- If one agent can do the work, do not split it first.
- When splitting work, write a delegation contract: Role / Goal / Context / Allowed actions / Ownership / Forbidden / Output format / Stop condition.
- Use worktrees or explicit file ownership when writing code in parallel.
- A reducer must be responsible for final integration, conflict judgment, and closing accountability.

## 7. Move The Human Role Upward

AI development workflows do not remove humans. They move humans from typists into system designers, reviewers, direction setters, and risk judges.

Good tasks to give agents:

- small-scope implementation;
- test additions;
- error diagnosis;
- documentation cleanup;
- repetitive code migration;
- toolable tasks with clear acceptance criteria.

Humans must retain judgment over:

- architecture direction;
- data models;
- permissions and security;
- dependency introduction;
- product experience;
- broad refactors;
- whether to continue, roll back, or stop.

## 8. Manage Understanding Debt And Safety Debt

The more capable AI becomes, the easier it is to accumulate understanding debt, safety risk, and the habit of "just one more prompt."

Practical principles:

- Read key diffs, not only agent summaries.
- Isolate large changes in branches or worktrees.
- Do not blindly install third-party Skills, MCPs, or plugins; review them first or rewrite clean versions.
- Give loops hard stops.
- Avoid giving an agent all three at once: private data, an untrusted input environment, and external communication.

## Recommended v1 Workflow

1. Build an AI-readable repo for every project: `AGENTS.md`, `SPEC.md`, `STATE.md`, `docs/`, `skills/`, `tests/`, and `scripts/`.
2. Use local agents as the main tools: Codex CLI, Claude Code, and Cursor, instead of web chat boxes.
3. Write success criteria before every task: what must be completed, how it will be verified, and when to stop.
4. Standardize verification commands: one-command test / lint / typecheck / build.
5. Turn repeated tasks into Skills: run them manually first, then write the Skill, then consider a loop.
6. Turn only tasks that pass the four-condition test into loops: repeated, verifiable, budgeted, and tooled.
7. Use multiple agents only for isolatable tasks: each subagent has a delegation contract, ownership, and stop condition.
8. Keep final judgment with humans: architecture, permissions, dependencies, product direction, and broad refactors should not be delegated automatically.
