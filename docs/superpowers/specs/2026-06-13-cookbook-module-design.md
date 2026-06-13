# Cookbook Module Design

Date: 2026-06-13

## Goal

Add the first Chinese `zh/cookbook/` module for scenario-driven Claude Code and Codex CLI usage.

The cookbook should help readers move from general workflow concepts to concrete task prompts. It should not become a detached command reference. Commands, prompts, and checks should live inside real development scenarios so readers can see when to use them, what context to provide, how to verify results, when to stop, and when a pattern may become a skill.

## Scope

Create a compact first version of the Chinese cookbook:

```text
zh/cookbook/README.md
zh/cookbook/repo-orientation.md
zh/cookbook/task-handoff.md
```

This gives the module:

1. a clear entrypoint and usage model,
2. one scenario for helping an agent understand an existing repository,
3. one scenario for handing a bounded task to Claude Code or Codex CLI.

These two scenarios are the foundation for later cookbook pages such as review/fix loops, context refresh, PR work, and multi-agent delegation.

## Non-Goals

This first version will not:

- Create a full Claude Code or Codex CLI command encyclopedia.
- Cover every common development scenario.
- Add scripts or executable automation.
- Add English translation beyond existing English stub files.
- Create the formal `zh/cases/` module.
- Duplicate the full start-project playbook, loop module, or skill workflow module.

The `zh/cases/` module may remain empty or user-maintained for now.

## Design Direction

Use a scenario-driven structure.

Each cookbook page should answer:

- when to use this scenario,
- when not to use it,
- what input material the human should provide,
- what Claude Code prompt to use,
- what Codex CLI prompt to use,
- how to verify the result,
- when to stop or escalate,
- what signals suggest the scenario should later become a project skill or system skill candidate.

The cookbook should embed prompts directly in the scenario pages. It should avoid splitting "command quick reference" away from the situation where the command or prompt is useful.

## Cookbook Entrypoint

`zh/cookbook/README.md` should explain:

- cookbook is for concrete task scenarios,
- playbooks are end-to-end workflows,
- loops are for deciding whether repeated automatic execution is safe,
- skill workflow is for deciding whether repeated scenarios should be captured,
- cookbook pages are not magic prompts and still require project context and verification.

It should list current scenarios:

- repository orientation,
- task handoff.

It should also list planned later scenarios without creating the files:

- review and fix feedback,
- context refresh,
- PR or CI work,
- multi-agent delegation.

The entrypoint should include a short "how to use a scenario" checklist:

1. Determine project level.
2. Read current project state.
3. Choose the matching scenario.
4. Fill in inputs and constraints.
5. Run the prompt.
6. Verify with real signals.
7. Record state, stop/escalate, or consider skill capture.

## Scenario 1: Repository Orientation

Create:

```text
zh/cookbook/repo-orientation.md
```

Purpose:

Help a human ask Claude Code or Codex CLI to read an existing repository and produce an orientation summary before doing work.

The scenario should be useful when:

- entering an unfamiliar repo,
- resuming a repo after time away,
- asking a new agent to understand project structure,
- preparing for task handoff.

It should not be used as permission for the agent to modify files.

Required sections:

- Purpose
- When to Use
- When Not to Use
- Inputs
- Claude Code Prompt
- Codex CLI Prompt
- Expected Output
- Verification
- Stop and Escalate
- Skill Capture Signals
- Related Docs

Prompt behavior:

- read README, STATE, SPEC, AGENTS if present,
- inspect scripts and package/config files,
- identify project level if possible,
- summarize structure, current state, verification commands, risks, and open questions,
- explicitly say when information is missing,
- avoid modifying files,
- avoid inventing commands, tests, deployment flows, team rules, or risk assumptions.

Verification:

- outputs cite real files or commands,
- verification commands exist before being recommended,
- unknowns are marked as unknown or needing human confirmation.

## Scenario 2: Task Handoff

Create:

```text
zh/cookbook/task-handoff.md
```

Purpose:

Help a human hand a bounded task to Claude Code or Codex CLI with enough context, constraints, verification requirements, and stop/escalation rules.

The scenario should be useful when:

- the project already has at least minimal AI workflow infra,
- the task has a clear outcome,
- the human wants an agent to make or propose a bounded change,
- there is a known verification signal or a need to explicitly report that no signal exists.

It should not be used for vague product exploration, high-risk Level 4 changes without human gates, or automatic loops.

Required sections:

- Purpose
- When to Use
- When Not to Use
- Inputs
- Handoff Template
- Claude Code Prompt
- Codex CLI Prompt
- Expected Output
- Verification
- Stop and Escalate
- Skill Capture Signals
- Related Docs

The handoff template should include:

- task goal,
- project level,
- files or areas likely involved,
- allowed actions,
- forbidden actions,
- verification command or signal,
- completion criteria,
- state update location,
- human confirmation gates.

Prompt behavior:

- read relevant project context first,
- restate understanding before editing,
- ask or flag missing information when needed,
- avoid broad refactors unless explicitly requested,
- avoid risky commands without confirmation,
- run or report verification,
- summarize changes and residual risk.

## Prompt Style Rules

Cookbook prompts should:

- be copyable,
- be scenario-specific,
- include both Claude Code and Codex CLI variants,
- ask the agent to use existing repo evidence,
- forbid invented commands, tests, deployment flows, team rules, and risk assumptions,
- include explicit verification expectations,
- include stop/escalation boundaries,
- avoid telling the agent to run loops automatically.

## Related Docs

Cookbook pages should link to relevant existing docs:

- `../guides/project-levels.md`
- `../guides/infra-by-level.md`
- `../guides/infra-checklists.md`
- `../playbooks/start-a-new-project.md`
- `../loops/README.md`
- `../skill-workflow/README.md`

`README.zh.md` should link `zh/cookbook/` to the cookbook entrypoint once the module exists.

## Acceptance Criteria

- `zh/cookbook/README.md` exists.
- `zh/cookbook/repo-orientation.md` exists.
- `zh/cookbook/task-handoff.md` exists.
- The cookbook entrypoint explains the difference between cookbook, playbooks, loops, and skill workflow.
- The cookbook uses scenario-driven structure, not a detached command reference.
- Both scenario pages include Claude Code and Codex CLI prompts.
- Prompts ask tools to inspect real project context and avoid inventing commands, tests, deployment flows, team rules, or risk assumptions.
- Scenario pages include verification, stop/escalate, and skill capture signals.
- Task handoff includes a copyable handoff template.
- Prompts do not ask agents to run automatic loops.
- `README.zh.md` links to the cookbook entrypoint.
- English stub files are unchanged.
- Related links point to existing files.
- Markdown whitespace checks pass.
