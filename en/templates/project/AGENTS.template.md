# AGENTS Template

## Purpose

Define the operating contract for agents in the current project, including working principles, verification requirements, forbidden operations, completion criteria, and human approval points.

## Recommended Levels

- Level 1: optional; this can start inside the README.
- Level 2: required.
- Level 3: required, with collaboration, review, ownership, and handoff rules clearly documented.
- Level 4: required, with permission boundaries, human approval points, risky operations, and rollback requirements clearly documented.

## Required Sections

- Project context
- Working principles
- Common commands
- Verification requirements
- Forbidden operations
- Operations that require human confirmation
- Completion criteria

## Optional Sections

- Code and documentation conventions
- Claude Code notes
- Codex CLI notes
- Multi-agent collaboration rules
- Release / rollback rules

## AI Generation Prompt

```text
Please read the current repository, README.md, SPEC.md, STATE.md, and available scripts, then generate a first draft of AGENTS.md for the project.
Common commands and verification requirements must be based on commands that really exist.
Mark the assumptions you made.
Do not invent tests, lint commands, deployment processes, or team rules that do not exist.
Clearly document forbidden operations, operations that require human confirmation, and completion criteria.
```

## Common Mistakes

- Writing a generic prompt instead of project rules.
- Missing real verification commands.
- Missing forbidden operations.
- Not explaining when human confirmation is required.
- Letting Claude Code / Codex CLI differences replace the main project rules.

## Copyable Template

````markdown
# AGENTS

## Project Context

- Project type:
- Project level:
- Main language / framework:
- Key directories:

## Working Principles

- Read existing documents first: `README.md`, `SPEC.md`, and `STATE.md`.
- Understand the current task's success criteria before making changes.
- Prefer small, verifiable changes.
- Do not make unrelated refactors.
- Before finishing, update necessary documentation or state.

## Common Commands

```bash
# Install dependencies

# Run

# Test

# Lint

# Build
```

## Verification Requirements

Before completing a task, run:

```bash
# Add real verification commands that exist in this project
```

If verification fails:

- Record the failed command and key error.
- Decide whether the failure was introduced by this change.
- Stop and ask for human judgment if you cannot fix it.

## Code And Documentation Conventions

- Naming, formatting, or documentation conventions:

## Forbidden Operations

- Do not delete or overwrite uncommitted user changes.
- Do not invent commands, APIs, configuration, or documentation that does not exist.
- Do not introduce new dependencies without confirmation.
- Do not run destructive git operations without confirmation.

## Operations That Require Human Confirmation

- Architecture direction changes.
- Data model changes.
- Permission, security, privacy, or compliance-related changes.
- New dependencies.
- Large refactors.
- Release, rollback, or production operations.

## Completion Criteria

- Target behavior is implemented or documentation is updated.
- Required verification has been run and results are recorded.
- Key diff has been self-reviewed.
- Remaining risks, if any, are explicitly documented.

## Claude Code Notes

- Project-local `CLAUDE.md` files or related skills can be used as supplementary rules.
- For long-running tasks, ask Claude Code to make the plan, verification commands, and stop conditions explicit.

## Codex CLI Notes

- When using Codex CLI, prefer having the agent read files, edit files, and run verification commands inside the repository.
- For multi-step tasks, generate a spec / plan before execution.
- Before finishing, report changed files, verification results, and remaining risks.
````
