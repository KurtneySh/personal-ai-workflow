# Skill Workflow Module Design

Date: 2026-06-13

## Goal

Add the first Chinese `zh/skill-workflow/` entrypoint for deciding when an AI development workflow should be captured as a reusable skill, how to choose between project-level and system-level skills, and how humans and agents should share that judgment.

The module should help readers avoid turning every repeated action into a formal skill too early. The default path is to start with a lightweight project skill, validate it through real use, and only promote it to a system skill when it is stable, bounded, verifiable, and useful beyond one local context.

## Scope

Create one focused document:

```text
zh/skill-workflow/README.md
```

The document will be the first version of the skill workflow module. It should stay compact, but its section boundaries should make later splitting easy.

## Non-Goals

This first version will not:

- Create real Codex or Claude Code skill packages.
- Install, publish, or synchronize skills.
- Add scripts or automation.
- Add English translation beyond existing English stub files.
- Define a full skill registry for this repository.
- Move current repository maintenance records under `docs/repo/`.

Concrete examples and deeper recipes can later move into `zh/cookbook/`, `zh/cases/`, or dedicated files under `zh/skill-workflow/`.

## Default Stance

The module's default stance is:

- Do not rush to create a skill.
- Start with a lightweight project skill when the pattern is still local or emerging.
- Promote to a system skill only after repeated validation and clear cross-task or cross-project value.
- Agent-generated skill suggestions are candidates, not automatic upgrades.
- A system skill should require explicit human review before creation, installation, or promotion.

## Skill Definition

The document should define skill broadly as a reusable AI workflow capability: a captured way of recognizing a situation, applying a bounded process, using tools or context correctly, and verifying the result.

It should distinguish two forms:

1. Project Skill
   - A project-bound reusable workflow or rule.
   - May live in `AGENTS.md`, `STATE.md`, a project runbook, a checklist, a task template, or a future project-local skill file.
   - Useful when the pattern depends on the repository's conventions, commands, architecture, risk boundaries, or team rules.

2. System Skill
   - A cross-project reusable capability that can exist outside one repository.
   - Similar to a formal `SKILL.md` package or agent-readable capability.
   - Useful only when the pattern has stable triggers, clear scope, reliable verification, examples, non-examples, and maintenance ownership.

The document should make clear that project skills are not inferior to system skills. They are often the correct first form.

## Candidate Sources

The document should explain where skill candidates come from:

- Human-led workflow: a human notices repeated coordination, repeated prompts, repeated review feedback, or repeated agent mistakes.
- Loop-led workflow: an agent running inside a confirmed loop observes recurring steps, recurring verification rules, or reusable recovery patterns.
- Project infra: stable rules from `README.md`, `STATE.md`, `SPEC.md`, `AGENTS.md`, CI, review conventions, or runbooks become candidates for project skills.
- Case studies: repeated lessons from `zh/cases/` can become candidates after several examples share the same shape.

## When To Capture A Skill

The document should include a concise checklist. A workflow is a good skill candidate when it is:

- repeated,
- stable enough to describe,
- bounded by clear inputs and outputs,
- teachable to an agent,
- verifiable with real signals,
- useful beyond one immediate task,
- more efficient as a reusable process than as repeated ad hoc prompting,
- clear about what it does not cover.

The document does not need a separate "do not create a skill" core chapter. Negative cases should appear inside the checklist and templates through fields such as boundaries, non-goals, failure modes, and not-applicable scenarios.

## Project Skill vs System Skill

The document should include a comparison table or compact list covering:

- scope,
- evidence required,
- storage location,
- owner or reviewer,
- verification expectation,
- promotion criteria,
- retirement or update conditions.

Suggested distinction:

- Choose a project skill when the pattern is tied to one repo, one team, one product, one command set, or one risk boundary.
- Choose a system skill candidate when the pattern has been validated across multiple tasks or projects and can be described without relying on hidden project context.
- Do not promote a project skill to a system skill only because it is useful once.

## Human, Agent, And Loop Boundaries

The module should make the judgment boundary explicit:

1. Human-led workflow
   - The human coordinates the work.
   - The human is responsible for noticing repeated patterns.
   - The agent may produce a skill candidate report to help the human decide.

2. Loop-led workflow
   - The loop already has clear goal, action boundaries, verification, stop conditions, and escalation conditions.
   - At the end of the loop, the agent may propose and draft a project skill candidate if it sees a repeated, bounded, verifiable pattern.
   - The agent must wait for human review before treating the draft as an adopted project skill.

3. System skill promotion
   - The agent may only produce a system skill candidate.
   - The agent must not automatically create, install, or promote a system skill.
   - Human review is required because system skills can affect future work outside the current project.

This section should link conceptually to `zh/loops/README.md` without duplicating loop design rules.

## Templates

Include two pure documentation templates.

### Project Skill Template

Fields:

- Skill name
- Trigger
- Project context
- Allowed actions
- Forbidden actions
- Inputs
- Steps
- Verification
- State updates
- Owner or reviewer
- Examples
- Not applicable when
- Last reviewed

The template should be usable in `AGENTS.md`, `STATE.md`, a project runbook, or a task brief.

### System Skill Candidate Template

Fields:

- Candidate name
- Problem it solves
- Evidence from repeated use
- Scope
- Non-goals
- Required inputs
- Workflow steps
- Tool or context requirements
- Verification
- Failure modes
- Examples
- Non-examples
- Maintenance owner
- Promotion decision

The template should frame the output as a candidate, not as an installed system skill.

## Prompt Guidance

Include short prompts for Claude Code and Codex CLI covering three scenarios:

- human-led skill candidate report,
- loop-led project skill draft,
- system skill candidate assessment.

Prompts should ask the tool to:

- inspect current project context,
- identify repeated patterns and evidence,
- distinguish project skill from system skill candidate,
- state boundaries and non-applicable scenarios,
- include verification requirements,
- avoid creating or installing system skills without human approval.

Prompts should not ask the tool to execute a loop or automatically modify system-level skill locations.

## Maintenance

The document should define lightweight maintenance rules:

- Review a project skill after real use.
- Update a skill when commands, project rules, verification signals, or risk boundaries change.
- Retire a skill when its trigger no longer appears or it causes repeated wrong behavior.
- Promote only when evidence shows it works across tasks or projects.
- Record enough context for the next human or agent to understand why the skill exists.

## Related Docs

The skill workflow README should link to:

- `../loops/README.md`
- `../playbooks/start-a-new-project.md`
- `../guides/project-levels.md`
- `../guides/infra-by-level.md`
- `../guides/infra-checklists.md`

## Acceptance Criteria

- `zh/skill-workflow/README.md` exists.
- The document defines project skills and system skills.
- The default stance says not to rush skill creation and to start with project skills when appropriate.
- The document explains human-led and loop-led skill candidate workflows.
- The document says system skills are always candidates until human-reviewed.
- The document includes project skill and system skill candidate templates.
- Negative cases are represented through checklist and template fields, not a separate core chapter.
- Prompt guidance includes Claude Code and Codex CLI examples.
- Prompts do not ask tools to auto-install or auto-promote system skills.
- Related links point to existing files.
- Markdown whitespace checks pass.
