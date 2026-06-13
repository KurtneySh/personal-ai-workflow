# Loops Module Design

## Goal

Add the first Chinese `zh/loops/` entrypoint for deciding whether an AI workflow should use a loop and, if so, how to design the loop safely.

The module should help readers avoid treating loops as a default automation pattern. A loop is only appropriate when the task has a clear goal, bounded actions, reliable verification, explicit stop conditions, and a known escalation path back to a human.

## Scope

Create one focused document:

- `zh/loops/README.md`

The document will be the first version of the loops module. It should stay compact, but its section boundaries should make later splitting easy.

## Non-Goals

This first version will not:

- Provide scenario-specific execution prompts for test-fix, review-fix, documentation cleanup, or CI loops.
- Design a long-running autonomous agent system.
- Add scripts or automation.
- Add English translation beyond existing placeholders.
- Modify the existing project templates.

Scenario execution prompts can later move into `zh/cookbook/` or `zh/playbooks/`.

## Default Stance

The module's default stance is:

- Do not loop by default.
- Consider a loop only when the goal, action boundary, verification signal, stop condition, and escalation path are clear.
- For Level 3 and Level 4 projects, require stronger human confirmation before using loops.
- For Level 4 projects, default to no automatic loop unless a human explicitly approves the risk boundary, rollback or audit expectations, and permission limits.

## Loop Types

The document should define three loop types:

1. Task execution loop
   - Pattern: plan -> execute -> verify -> adjust.
   - Use for a clearly bounded task with a reliable verification signal.

2. Development feedback loop
   - Pattern: test/lint/review/CI feedback -> fix -> re-run verification.
   - Use when feedback is concrete and the agent can safely iterate.

3. Long-running automatic loop
   - Pattern: monitor -> generate/fix/maintain -> verify -> repeat.
   - Use only with explicit human approval and strong stop/escalation controls.

The document should make clear that these are different risk classes. A one-task verification loop is not the same as a long-running autonomous process.

## Should This Be a Loop

The document should include a checklist that asks, in order:

- What is the target outcome?
- What actions is the agent allowed to take?
- What actions are forbidden?
- What real verification signal decides success or failure?
- What should stop the loop?
- What should escalate to a human?
- How many iterations are allowed?
- What state should be recorded between iterations?
- Does the project level require additional approval?

If any answer is missing, the recommendation should be to avoid the loop and run the task as a human-confirmed sequence instead.

## Stop / Verify / Escalate

Every loop design must include a Stop / Verify / Escalate block.

Stop defines when the loop must end, including:

- Success condition reached.
- Maximum iteration count reached.
- Verification keeps failing with the same class of error.
- The agent needs to change scope, permissions, dependencies, data model, deployment, or risk boundary.
- The agent cannot explain current state or next action.

Verify defines the real signals used to judge progress, such as:

- Test, lint, typecheck, build, or review result.
- A human-approved checklist.
- A recorded status update.
- Explicit absence of a reliable verification command.

Escalate defines when the loop returns to a human, such as:

- Level 4 uncertainty.
- Production, permissions, authentication, payment, sensitive data, security, or compliance risk.
- Risky commands.
- Contradictory docs.
- Missing verification signal.
- Repeated failure.

## Project-Level Gates

The document should map loop requirements to project levels:

- Level 1: lightweight loops are allowed only with clear stop conditions and low-risk actions.
- Level 2: require a real verification command and state recording.
- Level 3: require collaboration rules, review or CI signals, ownership boundaries, and handoff records.
- Level 4: default to no automatic loop; require human approval, risk boundary, rollback or audit expectations, and agent permission limits.

The wording should avoid making Level 3 or Level 4 loop requirements optional once those levels are selected.

## Loop Design Template

Include a pure documentation template with these fields:

- Loop goal
- Loop type
- Project level
- Allowed actions
- Forbidden actions
- Verification signal
- Stop conditions
- Escalation conditions
- Maximum iterations
- Human confirmation gates
- State record location
- Owner or reviewer

The template should be usable as a copied section in a project README, STATE, AGENTS, or task brief.

## Judgment Prompts

Include one Claude Code prompt and one Codex CLI prompt for loop judgment and design only.

The prompts should ask the tool to:

- Read the current repo context.
- Judge whether a loop is appropriate.
- Identify the loop type.
- Check project level and risk.
- Draft Stop / Verify / Escalate.
- Refuse to execute the loop until a human confirms.
- Avoid inventing commands, tests, deployment flows, team rules, or risk assumptions.

The prompts should not ask the tool to run the loop.

## Related Docs

The loops README should link to:

- `../guides/project-levels.md`
- `../guides/infra-by-level.md`
- `../guides/infra-checklists.md`
- `../playbooks/start-a-new-project.md`

## Acceptance Criteria

- `zh/loops/README.md` exists.
- The document defines the default stance, loop types, loop decision checklist, Stop / Verify / Escalate, project-level gates, loop design template, judgment prompts, and related docs.
- The document clearly says loop is not the default.
- Level 1 does not assume `SPEC.md` or `AGENTS.md`.
- Level 3/4 requirements are not framed as optional once those project levels are selected.
- Judgment prompts do not execute loops.
- Related links point to existing files.
- Markdown whitespace checks pass.
