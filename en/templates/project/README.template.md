# README Template

## Purpose

Use this as the project entrypoint so humans and agents can quickly understand the project's purpose, how to run it, how to verify it, and where to find critical context.

## Recommended Levels

- Level 1-4: required.

## Required Sections

- What the project is
- Quick start
- Common commands
- Verification
- Related documents

## Optional Sections

- Who this is for
- Directory structure
- Current status
- Deployment
- Contributing

## AI Generation Prompt

```text
Please read the current repository and generate a first draft of README.md for this project.
Base quick start, common commands, and verification on real files and scripts.
Mark the assumptions you made.
Do not invent commands, services, deployment processes, or document links that do not exist.
```

## Common Mistakes

- Only writing installation commands without explaining the project goal.
- Missing verification guidance, which leaves agents unable to judge whether a change is correct.
- Linking to documents that do not exist.
- Writing current status in too much detail, causing the README to go stale quickly.

## Copyable Template

````markdown
# Project Name

## What This Project Is

Use 2-4 sentences to explain what problem this project solves and what the main output is.

## Who This Is For

- Users:
- Maintainers:
- How agents participate:

## Quick Start

```bash
# Install dependencies

# Start the project
```

## Common Commands

```bash
# Run

# Test

# Lint

# Build
```

## Verification

Before completing a change, run at least:

```bash
# Add real verification commands that exist in this project
```

Expected result:

- All checks pass, or the failed command and key error are recorded.

## Directory Structure

```text
.
├── README.md
└── ...
```

## Current Status

Current status is recorded in `STATE.md`.

## Related Documents

- `SPEC.md`: project goals, scope, and key decisions.
- `STATE.md`: current status, next steps, and blockers.
- `AGENTS.md`: agent operating rules.
````
