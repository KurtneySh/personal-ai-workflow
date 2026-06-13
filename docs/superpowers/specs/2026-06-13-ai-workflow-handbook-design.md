# AI Workflow Handbook Design

Date: 2026-06-13

## Goal

Build this repository into a bilingual AI development workflow handbook.

The repository should work as both:

- a knowledge base for AI-assisted development principles, and
- a practical usage manual for choosing project infrastructure, running agentic workflows, using Claude Code and Codex CLI, designing loops, and capturing reusable skills.

The v1 content source of truth is Chinese. The public default entry point is English-first.

## Design Direction

Use a decision-first handbook structure:

1. Identify the project level.
2. Choose the minimum useful AI workflow infrastructure for that level.
3. Add infrastructure from capability-based checklists when needed.
4. Use playbooks and tool cookbooks for concrete tasks.
5. Use loop guidance, skill workflow guidance, and case studies to deepen and reuse successful workflows.

The existing `AI 开发工作流最佳实践.md` provides the philosophical foundation and should move into the Chinese reference section.

## Repository Structure

```text
README.md
README.zh.md

zh/
  guides/
  playbooks/
  cookbook/
  loops/
  skill-workflow/
  cases/
  templates/
  reference/

en/
  guides/
  playbooks/
  cookbook/
  loops/
  skill-workflow/
  cases/
  templates/
  reference/

docs/repo/
  translation-status.md
  decisions/
  maintenance-guide.md
```

### Root README Files

`README.md` is the English public entry point. It should state that the Chinese content is currently the source of truth and that English content will be translated progressively.

`README.zh.md` is the main usable entry point for v1. It should stay short and act as a navigation hub, not a long-form container.

### Language Strategy

Chinese is the v1 source of truth.

English directories mirror the Chinese structure but can start as placeholders. Translation happens after the Chinese content stabilizes.

### Repo Working Records

`docs/repo/` is for temporary repository buildout records, not reader-facing workflow content.

It may include:

- translation progress,
- repository design decisions,
- maintenance rules for adding new documents, cases, and templates.

After v1 stabilizes, these records can be deleted, archived, or folded into `CONTRIBUTING.md` and a small set of retained decision records.

## Content Areas

### `guides/`

Purpose: help readers decide what infrastructure their project needs.

Initial documents:

```text
project-levels.md
infra-by-level.md
infra-checklists.md
```

### `playbooks/`

Purpose: describe end-to-end workflows from goal to completion.

Initial documents:

```text
start-a-new-project.md
feature-to-pr.md
debug-and-fix.md
ci-failure.md
capture-skill.md
```

### `cookbook/`

Purpose: provide scenario-driven Claude Code and Codex CLI usage.

The cookbook should not be a separate command reference. Each document starts from a real task and explains how to use the tools inside that scenario.

Initial documents:

```text
understand-a-repo.md
implement-a-change.md
debug-a-failure.md
review-a-diff.md
handoff-and-resume.md
```

### `loops/`

Purpose: make loop design a first-class topic.

This section explains when a workflow should or should not become an automated loop, and how to design budget limits, verification, stop conditions, escalation rules, and human approval gates.

Initial documents:

```text
loop-readiness.md
loop-design.md
loop-stop-conditions.md
```

### `skill-workflow/`

Purpose: explain when and how successful workflows should be captured as reusable skills.

The directory is named `skill-workflow/` rather than `skills/` to avoid implying that this repository stores installable skills.

Initial documents:

```text
when-to-create-a-skill.md
skill-design-principles.md
skill-examples.md
```

`skill-examples.md` should include examples such as:

- repeated debugging workflows,
- PR review routines,
- CI failure triage,
- new project initialization checks,
- document generation and verification,
- multi-agent delegation contracts,
- repeated framework-specific code modification workflows.

### `cases/`

Purpose: show practical examples that connect project level, infrastructure choices, agent behavior, human judgment, validation, skill opportunities, and loop boundaries.

Initial documents:

```text
case-template.md
example-personal-project.md
example-collaborative-project.md
```

Each case should answer:

- project background and project level,
- selected infrastructure,
- intentionally omitted infrastructure,
- what the agent did,
- what the human decided,
- validation loop,
- what should be captured as a skill,
- what should not be looped,
- what to improve next time.

### `templates/`

Purpose: provide pure document templates as examples. v1 will not include generator scripts.

Recommended structure:

```text
project/
  README.template.md
  SPEC.template.md
  STATE.template.md
  AGENTS.template.md

task/
  task-brief.template.md
  handoff-note.template.md
  pr-description.template.md
  pr-review-checklist.template.md

collaboration/
  delegation-contract.template.md
  ownership.template.md

risk/
  risk-register.template.md
  rollback-plan.template.md
  human-approval-gate.template.md

skill/
  skill.template.md
  skill-index.template.md

case/
  case-study.template.md
```

Each template should include:

- purpose,
- recommended project levels,
- required sections,
- optional sections,
- example,
- AI generation prompt,
- common mistakes.

### `reference/`

Purpose: hold concepts, philosophy, glossary, and appendix material.

Initial documents:

```text
AI-开发工作流最佳实践.md
concepts.md
glossary.md
```

## Project Levels

Project level is a recommendation mechanism, not a fixed identity label. A project can borrow higher-level checklists when risk requires it.

### Level 1: Personal / Scratch

For personal small projects, experiments, and one-off scripts.

Minimum infrastructure:

- `README.md`: what the project is and how to run it,
- `STATE.md`: current state, next step, known issues,
- at least one verification command.

Optional additions:

- task brief,
- short prompt or command log.

### Level 2: Personal / Maintained

For personal long-lived projects.

Minimum infrastructure:

- Level 1 items,
- `SPEC.md`: goals, scope, non-goals, key decisions,
- `AGENTS.md`: agent working rules,
- `docs/decisions/`,
- tests or explicit verification scripts.

Optional additions:

- skill index,
- reusable task briefs,
- release notes.

### Level 3: Collaborative

For multi-person projects that need review, ownership, and stable handoff.

Minimum infrastructure:

- Level 2 items,
- `CONTRIBUTING.md`,
- `CODEOWNERS` or ownership documentation,
- PR checklist,
- CI checks,
- issue or task templates,
- handoff notes,
- delegation contract for multi-agent work.

Optional additions:

- team skill registry,
- architecture decision records,
- review playbooks.

### Level 4: Production / High-risk

For production systems, high-permission systems, sensitive data, payment, authentication, security, compliance, or large user impact.

Minimum infrastructure:

- Level 3 items,
- risk register,
- security and privacy checklist,
- rollback plan,
- human approval gates,
- deployment checklist,
- incident or audit notes,
- loop stop conditions,
- permission boundaries for agents.

Optional additions:

- threat model,
- staging verification matrix,
- change freeze rules,
- postmortem template.

## Checklist Model

Infrastructure checklists should be capability-based rather than file-based.

Capabilities:

1. Context: stable project understanding.
2. Instructions: agent behavior rules.
3. Feedback: verification loop.
4. Task Flow: task goals, acceptance criteria, and stop conditions.
5. Reuse: prompts, playbooks, and skill workflow assets.
6. Automation / Loop: loop readiness, budget, verification, stop conditions, escalation.
7. Collaboration: ownership, PR workflow, handoff, delegation contracts.
8. Risk: security, privacy, rollback, approval gates, audit trail.

Each checklist item should use this schema:

```text
Item:
Purpose:
Recommended levels:
Required when:
Example file:
How AI uses it:
Common mistakes:
```

## Cross-Section Relationships

Example task: fix a CI failure.

- `guides/` explains why a collaborative project needs CI, PR checklist, and ownership.
- `playbooks/ci-failure.md` describes the end-to-end CI repair workflow.
- `cookbook/debug-a-failure.md` explains how to use Claude Code and Codex CLI for the concrete debugging steps.
- `loops/loop-readiness.md` explains whether CI repair is safe to loop and where to stop.
- `skill-workflow/when-to-create-a-skill.md` explains when repeated CI triage should become a reusable skill.
- `cases/` records a real CI repair and the judgments made by the agent and human.

## Scope

In scope for v1:

- define the bilingual structure,
- write Chinese-first handbook content,
- create English placeholders,
- provide pure document templates,
- cover Claude Code and Codex CLI as first-class tools,
- cover loop design as a first-class section,
- add practical case study structure.

Out of scope for v1:

- script-based template generation,
- fully translated English content,
- automated publishing,
- installable skill packages,
- a full command reference detached from scenarios.

## Implementation Constraints

- The current `AI 开发工作流最佳实践.md` should be moved into `zh/reference/` and renamed to a path-safe filename while preserving its Chinese title inside the document.
- `.superpowers/` is used only for brainstorming visual companion artifacts and should not be part of the reader-facing content.
- The implementation plan should create the structure incrementally, starting with navigation, project levels, infra recommendations, and checklist documents.
