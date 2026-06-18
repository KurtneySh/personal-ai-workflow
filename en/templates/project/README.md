# Project Workspace Templates

These templates help you set up the smallest useful AI-readable workspace for a project.

They are not mandatory standards. They are examples that help you decide which information must be written down and which parts can be added as project complexity grows.

## Purpose

Project workspace templates solve three problems:

- Help humans and agents quickly understand what the project is.
- Help agents know how to run, change, and verify the project.
- Keep project state, goals, rules, and context in the filesystem instead of scattering them across chat history.

## How To Use

1. Read `en/guides/project-levels.md` to decide the project level.
2. Read `en/guides/infra-by-level.md` to confirm the smallest set of files the project needs right now.
3. Copy the matching templates from this directory into your project root.
4. Ask an AI agent to read the current repository and generate a first draft, marking its assumptions.
5. Have a human review the critical fields, especially verification commands, forbidden operations, and human approval points.

## Level 1-4 Recommendation Table

| Template | Level 1 | Level 2 | Level 3 | Level 4 |
| --- | --- | --- | --- | --- |
| `README.template.md` | Required | Required | Required | Required |
| `STATE.template.md` | Required | Required | Required | Required |
| `SPEC.template.md` | Optional | Required | Required | Required, with high-risk constraints clearly documented |
| `AGENTS.template.md` | Optional | Required | Required, with collaboration rules clearly documented | Required, with permission boundaries and approval points clearly documented |

## Template Selection Guidance

- For Level 1 projects, start with `README.md` and `STATE.md`; keep `SPEC.md` and `AGENTS.md` optional unless the project will be maintained long term or agents will participate repeatedly.
- For Level 2 projects, it is usually worth adding `README.md`, `SPEC.md`, `STATE.md`, and `AGENTS.md` at the same time.
- For Level 3 projects, `AGENTS.md` must explain review, ownership, handoff, and collaboration boundaries.
- For Level 4 projects, `SPEC.md` and `AGENTS.md` must explain risks, permissions, human approval points, and rollback requirements.

## Ask AI To Generate A First Draft

You can use this prompt:

```text
Please read the current repository and decide whether this project is roughly Level 1-4.
Based on the templates under en/templates/project/, generate first drafts for the templates required at that level.
Level 1 usually starts with README.md and STATE.md; generate SPEC.md and AGENTS.md only if the project needs long-term maintenance or repeated agent participation.
Mark the assumptions you made.
Do not invent commands, tests, deployment processes, or team rules that do not exist.
If information is missing, mark it as "needs confirmation" instead of filling it in yourself.
```

## Common Mistakes

- Treating templates as mandatory standards instead of trimming them by project level.
- Letting AI invent verification commands instead of checking the actual project scripts first.
- Creating `STATE.md` and then letting it go stale.
- Writing `AGENTS.md` as a generic prompt instead of project-specific forbidden operations and completion criteria.
- Running a Level 4 project without human approval points, permission boundaries, or rollback requirements.
