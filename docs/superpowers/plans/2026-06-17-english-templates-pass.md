# English Templates Pass Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Translate the stabilized Chinese project workspace templates into an English v1 draft that is directly usable from the English handbook.

**Architecture:** Keep the English templates as a strict structural mirror of `zh/templates/project/`. Each template file owns one document type, while `en/templates/README.md` and `en/templates/project/README.md` provide navigation and level-based selection guidance.

**Tech Stack:** Markdown documentation only; verification uses shell commands, `rg`, `find`, `git diff --check`, and a local Markdown relative-link checker.

---

## File Structure

- Create `en/templates/project/README.md`: English project workspace template overview and Level 1-4 recommendation table.
- Create `en/templates/project/README.template.md`: copyable English project README template.
- Create `en/templates/project/SPEC.template.md`: copyable English project SPEC template.
- Create `en/templates/project/STATE.template.md`: copyable English project STATE template.
- Create `en/templates/project/AGENTS.template.md`: copyable English AGENTS template.
- Modify `en/templates/README.md`: replace placeholder with English template section entrypoint.
- Modify `README.md`: update repository map and language status so templates are no longer described as a later pass.
- Modify `docs/repo/translation-status.md`: mark templates as English v1 draft and add this pass to notes.

## Task 1: Template Entrypoints

**Files:**
- Modify: `en/templates/README.md`
- Create: `en/templates/project/README.md`

- [ ] **Step 1: Read the Chinese source and current English placeholder**

Run:

```bash
sed -n '1,220p' zh/templates/project/README.md
sed -n '1,160p' en/templates/README.md
```

Expected: the Chinese source includes purpose, usage, Level 1-4 recommendation table, AI generation prompt, and common mistakes; the English file is still a placeholder.

- [ ] **Step 2: Replace `en/templates/README.md` with an English section entrypoint**

Write this exact content:

```markdown
# Templates

This section provides document templates for building a minimal AI-readable project workspace.

Templates are examples, not mandatory standards. Use the project level guidance to decide which files are required now and which files can be added as the project grows.

## Available Templates

- [Project Workspace Templates](project/README.md): templates for `README.md`, `SPEC.md`, `STATE.md`, and `AGENTS.md`.

## How To Use This Section

1. Read [Project Levels](../guides/project-levels.md) to decide whether your project is Level 1-4.
2. Read [Infra By Level](../guides/infra-by-level.md) to choose the smallest useful AI workflow infra.
3. Open the [Project Workspace Templates](project/README.md) and copy the templates that match your project level.
4. Ask an AI agent to draft the files from the real repository, then review assumptions, verification commands, forbidden operations, and human approval points.
```

- [ ] **Step 3: Create `en/templates/project/README.md`**

Write this exact content:

```markdown
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
```

- [ ] **Step 4: Verify Task 1 links**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
for p in [Path('en/templates/README.md'), Path('en/templates/project/README.md')]:
    text = p.read_text()
    print(p, 'placeholder' in text.lower(), 'planned later' in text.lower())
PY
```

Expected output:

```text
en/templates/README.md False False
en/templates/project/README.md False False
```

- [ ] **Step 5: Commit Task 1**

Run:

```bash
git add en/templates/README.md en/templates/project/README.md
git commit -m "docs: translate english template entrypoints"
```

Expected: commit succeeds.

## Task 2: Project Workspace Templates

**Files:**
- Create: `en/templates/project/README.template.md`
- Create: `en/templates/project/SPEC.template.md`
- Create: `en/templates/project/STATE.template.md`
- Create: `en/templates/project/AGENTS.template.md`

- [ ] **Step 1: Read the Chinese source templates**

Run:

```bash
sed -n '1,240p' zh/templates/project/README.template.md
sed -n '1,260p' zh/templates/project/SPEC.template.md
sed -n '1,260p' zh/templates/project/STATE.template.md
sed -n '1,280p' zh/templates/project/AGENTS.template.md
```

Expected: each source template includes purpose, recommended levels, required sections, optional sections, AI generation prompt, common mistakes, and a copyable template.

- [ ] **Step 2: Create `en/templates/project/README.template.md`**

Write this exact content:

````markdown
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

```markdown
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
```
````

- [ ] **Step 3: Create `en/templates/project/SPEC.template.md`**

Write this exact content:

```markdown
# SPEC Template

## Purpose

Record project goals, boundaries, constraints, and key decisions so product direction and implementation judgment do not need to be re-explained repeatedly.

## Recommended Levels

- Level 1: optional.
- Level 2-3: required.
- Level 4: required, with explicit safety, privacy, compliance, production, or other high-risk constraints.

## Required Sections

- Background
- Goals
- Non-goals
- Core features
- Constraints
- Success criteria
- Key decisions

## Optional Sections

- Users / scenarios
- Risks and open questions
- Data model
- Permission boundaries
- Release strategy

## AI Generation Prompt

```text
Please read the current repository and README.md, then generate a first draft of SPEC.md for this project.
Clearly separate goals from non-goals.
Mark the assumptions you made.
If the repository does not contain evidence for a feature, constraint, or success criterion, write "needs confirmation" instead of inventing it.
Do not invent commands, verification methods, features, constraints, or success criteria that do not exist.
```

## Common Mistakes

- Writing only a vision without non-goals.
- Defining success criteria that cannot be verified.
- Treating implementation details as product goals.
- Omitting permission, safety, privacy, or rollback constraints for high-risk projects.

## Copyable Template

```markdown
# SPEC

## Background

Why does this project exist? What problem is it solving right now?

## Goals

- Goal 1:

## Non-Goals

- Non-goal 1:

## Users / Scenarios

- User:
- Scenario:

## Core Features

- Feature 1:

## Constraints

- Technical constraints:
- Time / resource constraints:
- Safety / privacy / compliance constraints:

## Success Criteria

- Behavioral result:
- Verification method:
- Completion judgment:
- Handling when criteria are not met:

## Key Decisions

| Date | Decision | Reason |
| --- | --- | --- |
|  |  |  |

## Risks And Open Questions

- Risk:
- Open question:
```
```

- [ ] **Step 4: Create `en/templates/project/STATE.template.md`**

Write this exact content:

```markdown
# STATE Template

## Purpose

Record short-term project state, next steps, blockers, and handoff context. This file should be updated frequently, with clarity for the next handoff as the priority.

## Recommended Levels

- Level 1-4: required.

## Required Sections

- Current phase
- Current goal
- Next steps
- Blockers
- Recently completed
- Context for the next agent

## Optional Sections

- Recent decisions
- Known issues
- Current branch / PR
- Recent verification results

## AI Generation Prompt

```text
Please read the current repository, README.md, SPEC.md, and the latest git diff, then generate a first draft of STATE.md for this project.
Record only the current state, next steps, blockers, and handoff context.
Mark the assumptions you made.
Do not put the long-term product vision into STATE.md; long-term goals belong in SPEC.md.
Do not invent commands, verification results, blockers, or decisions that do not exist.
```

## Common Mistakes

- Creating the file and then not updating it.
- Turning STATE into a second README.
- Not recording blockers and next steps.
- Leaving the context for the next agent too vague.

## Copyable Template

```markdown
# STATE

## Current Phase

- Phase name:

## Current Goal

- Current goal:

## Next Steps

1. Next action:
2. Owner or prerequisite:
3. Verification or handoff requirement:

## Blockers

- None

## Recently Completed

- Completed item:

## Recent Decisions

| Date | Decision | Reason |
| --- | --- | --- |
|  |  |  |

## Known Issues

- Known issue:

## Context For The Next Agent

- Current priority:
- Do not repeat:
- Confirm first:
- Recent verification result:
```
```

- [ ] **Step 5: Create `en/templates/project/AGENTS.template.md`**

Write this exact content:

````markdown
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
````

- [ ] **Step 6: Verify Task 2 file set**

Run:

```bash
find en/templates/project -maxdepth 1 -type f | sort
```

Expected output:

```text
en/templates/project/AGENTS.template.md
en/templates/project/README.md
en/templates/project/README.template.md
en/templates/project/SPEC.template.md
en/templates/project/STATE.template.md
```

- [ ] **Step 7: Check for accidental Chinese text in English templates**

Run:

```bash
rg -n "[一-龥]" en/templates
```

Expected: no output.

- [ ] **Step 8: Commit Task 2**

Run:

```bash
git add en/templates/project/README.template.md en/templates/project/SPEC.template.md en/templates/project/STATE.template.md en/templates/project/AGENTS.template.md
git commit -m "docs: translate english project templates"
```

Expected: commit succeeds.

## Task 3: Status Updates And Verification

**Files:**
- Modify: `README.md`
- Modify: `docs/repo/translation-status.md`

- [ ] **Step 1: Update `README.md` repository map and language status**

Change the `en/templates/` bullet from planned-later wording to:

```markdown
- `en/templates/`: English v1 draft document templates for project workbenches.
```

Change the current language status paragraph to:

```markdown
Chinese v1 is the source of truth. English v1 draft content now covers the public entrypoint, core concepts, the new-project playbook, cookbook scenarios, loops, skill workflow, and project workspace templates, while reference material and cases remain later English passes.
```

- [ ] **Step 2: Update `docs/repo/translation-status.md` policy summary**

Change this sentence:

```markdown
English content under `en/` mirrors the Chinese structure. Core concepts and scenario guidance are drafted first; templates, reference material, and cases remain placeholders.
```

To:

```markdown
English content under `en/` mirrors the Chinese structure. Core concepts, scenario guidance, and project workspace templates are drafted first; reference material and cases remain placeholders.
```

- [ ] **Step 3: Update templates status in `docs/repo/translation-status.md`**

Change the templates row to:

```markdown
| templates | v1 stabilized | v1 draft |
```

- [ ] **Step 4: Add the English templates pass note**

Add this note after the English scenario layer note:

```markdown
- English templates draft completed on 2026-06-17 for `en/templates/` and `en/templates/project/`.
```

- [ ] **Step 5: Run Markdown relative-link verification**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
import re

roots = [Path('README.md'), Path('en/templates'), Path('docs/repo/translation-status.md')]
files = []
for root in roots:
    if root.is_file():
        files.append(root)
    else:
        files.extend(sorted(root.rglob('*.md')))

broken = []
pattern = re.compile(r'(?<!!)\[[^\]]+\]\(([^)]+)\)')
for path in files:
    text = path.read_text()
    for target in pattern.findall(text):
        if '://' in target or target.startswith('#') or target.startswith('mailto:'):
            continue
        clean = target.split('#', 1)[0]
        if not clean:
            continue
        resolved = (path.parent / clean).resolve()
        if not resolved.exists():
            broken.append((str(path), target))

if broken:
    print('BROKEN LINKS:')
    for path, target in broken:
        print(f'{path}: {target}')
    raise SystemExit(1)
print('BROKEN LINKS: none')
PY
```

Expected output:

```text
BROKEN LINKS: none
```

- [ ] **Step 6: Run placeholder and stale-status scan**

Run:

```bash
rg -n "planned later English pass|Translation is planned|templates remain placeholders|templates, reference material, and cases remain placeholders" README.md en/templates docs/repo/translation-status.md
```

Expected: no output.

- [ ] **Step 7: Run formatting check**

Run:

```bash
git diff --check
```

Expected: no output.

- [ ] **Step 8: Review final diff**

Run:

```bash
git diff -- README.md docs/repo/translation-status.md en/templates
```

Expected: diff only includes English templates and related status updates; no Chinese source files are changed.

- [ ] **Step 9: Commit Task 3**

Run:

```bash
git add README.md docs/repo/translation-status.md
git commit -m "docs: update english template translation status"
```

Expected: commit succeeds.

## Final Verification

- [ ] **Step 1: Confirm committed history for this pass**

Run:

```bash
git log --oneline -5
```

Expected: recent commits include:

```text
docs: update english template translation status
docs: translate english project templates
docs: translate english template entrypoints
```

- [ ] **Step 2: Run repository status check**

Run:

```bash
git status --short
```

Expected: no uncommitted files, except allowed local-only `.superpowers/` content if present.

- [ ] **Step 3: Re-run full verification commands**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
import re

roots = [Path('README.md'), Path('en/templates'), Path('docs/repo/translation-status.md')]
files = []
for root in roots:
    if root.is_file():
        files.append(root)
    else:
        files.extend(sorted(root.rglob('*.md')))

broken = []
pattern = re.compile(r'(?<!!)\[[^\]]+\]\(([^)]+)\)')
for path in files:
    text = path.read_text()
    for target in pattern.findall(text):
        if '://' in target or target.startswith('#') or target.startswith('mailto:'):
            continue
        clean = target.split('#', 1)[0]
        if not clean:
            continue
        resolved = (path.parent / clean).resolve()
        if not resolved.exists():
            broken.append((str(path), target))

if broken:
    print('BROKEN LINKS:')
    for path, target in broken:
        print(f'{path}: {target}')
    raise SystemExit(1)
print('BROKEN LINKS: none')
PY
rg -n "[一-龥]" en/templates
rg -n "planned later English pass|Translation is planned|templates remain placeholders|templates, reference material, and cases remain placeholders" README.md en/templates docs/repo/translation-status.md
git diff --check
```

Expected:

```text
BROKEN LINKS: none
```

The two `rg` commands and `git diff --check` produce no output.
