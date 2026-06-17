# Loops

In this handbook, a loop is a controlled way for an agent to work toward a goal by repeating a cycle of action, verification, and correction.

The default stance is conservative: do not make a task a loop by default. A loop is appropriate only when the goal is clear, the action boundary is narrow, verification signals are real, stop / escalate conditions are explicit, and the agent knows when to hand the work back to a human.

## Default Stance

- Do not loop by default.
- Judge the project level first, then judge loop risk.
- Do not design a loop without a real verification signal.
- Do not design a loop without stop conditions.
- Do not design a loop without stop / escalate conditions that say when a human must take over.
- Level 3 and Level 4 projects require human confirmation of the goal, action boundary, verification signal, stop / escalate conditions, and permission boundary.
- Level 4 projects should not run automatic loops unless a human has explicitly approved the risk boundary, rollback requirements, audit requirements, and limits on agent permissions.

## What Makes a Task Loop-Ready

A task is loop-ready when all of these are true:

- The desired outcome is concrete and verifiable.
- The agent is allowed to perform a small, specific set of actions.
- The agent is forbidden from expanding scope, changing permissions, touching sensitive data, or changing production behavior unless a human gate allows it.
- Each iteration has a real verification signal, such as a test, lint, typecheck, build, CI result, review result, or human-confirmed checklist.
- The maximum number of iterations is fixed before the loop starts.
- The loop has clear stop / escalate conditions.
- The loop has a state record, handoff note, or other place to record what happened in each iteration.
- The project level does not require additional approval, or that approval has already been granted by a human.

If any of these are missing, treat the work as a sequence of human-reviewed steps instead of a loop.

## When a Task Is Not Loop-Ready

Do not design a loop when:

- The goal is still exploratory or likely to change.
- Success depends on taste, product direction, or a human judgment that has not been captured as a checklist.
- The agent would need to change scope, add dependencies, alter infra, change the data model, change deployment behavior, or cross a permission boundary.
- The task involves production, authentication, payments, security, compliance, sensitive data, or system-level skill behavior without explicit human approval.
- The next step could push, merge, deploy, run a migration, change permissions, expose sensitive data, or modify a system-level skill.
- Failures cannot be reproduced or verified with a real signal.
- The agent cannot explain the current state, the last iteration, and the proposed next iteration.

In these cases, stop and use a handoff instead: summarize the current state, identify the decision needed, and ask a human to confirm the next step.

## Loop Types

### Task Execution Loop

Pattern:

```text
plan -> execute -> verify -> correct
```

Suitable when:

- The goal is clear.
- The action boundary is small.
- Every iteration has a real verification signal.

Not suitable when:

- The goal is still being explored.
- The scope needs to change often.
- The loop would need new permissions, data model changes, deployment changes, or production risk.

### Development Feedback Loop

Pattern:

```text
test / lint / review / CI feedback -> fix -> verify again
```

Suitable when:

- The feedback is specific.
- The failure is reproducible.
- The verification command can actually run.

Not suitable when:

- The feedback is a vague preference.
- The cause of failure is unclear.
- Each iteration requires a human to re-decide product direction.

### Long-Running Automatic Loop

Pattern:

```text
monitor -> generate / fix / maintain -> verify -> repeat
```

Suitable when:

- There is a clear owner.
- There is an audit trail.
- There are strong stop / escalate conditions.
- The permission boundary has been approved by a human.

Not suitable when:

- There is no owner.
- There is no audit or rollback requirement.
- The loop touches production, permissions, authentication, payments, sensitive data, security, or compliance without human approval.

These loop types do not carry the same risk. A verification loop inside a task is not the same thing as a long-running automatic agent.

## Required Loop Design Fields

Copy this template into the project `README.md`, `STATE.md`, `AGENTS.md`, or task description before running a loop.

```markdown
## Loop Design

- Loop goal:
- Loop type:
- Project level:
- Allowed actions:
- Forbidden actions:
- Verification signal:
- Stop conditions:
- Escalation conditions:
- Maximum iterations:
- Human confirmation gates:
- State record location:
- Owner or reviewer:
```

Field requirements:

- `Loop goal` must be verifiable.
- `Loop type` should distinguish task execution, development feedback, and long-running automatic loops.
- `Project level` must be judged before loop risk.
- `Allowed actions` must be small enough that the agent cannot expand scope on its own.
- `Forbidden actions` must explicitly cover production, infra, permissions, authentication, payments, sensitive data, security, deployment, push, merge, migration, and system-level skill boundaries.
- `Verification signal` must come from a real command, real review / CI result, or human-confirmed checklist.
- `Stop conditions` must say when the loop ends without further action.
- `Escalation conditions` must say when the agent must stop / escalate to a human.
- `Maximum iterations` must be a specific number.
- `Human confirmation gates` must list every action that needs human approval before it runs.
- `State record location` must say where iteration status and handoff notes are recorded.
- `Owner or reviewer` must identify who can approve risky decisions.

## Verification Signals

A verification signal is evidence that an iteration succeeded or failed. It must be real, relevant, and available before the loop starts.

Good verification signals include:

- `test`, `lint`, `typecheck`, or `build` commands that actually exist.
- CI results.
- Code review or document review results.
- A checklist that a human has confirmed in advance.
- A domain-specific health check that is already trusted by the project.

Weak or invalid verification signals include:

- The agent's confidence.
- A command that does not exist in the repo.
- A vague statement such as "looks good."
- A future review that has not been requested.
- A metric that does not correspond to the loop goal.

Verification failure is not success. If the agent cannot find a real verification signal, it must record that gap and stop designing the loop.

## Stop / Escalate Conditions

Every loop must define stop / escalate before it starts.

### Stop

The agent must stop when:

- The success condition has been met.
- The maximum number of iterations has been reached.
- The same class of verification failure repeats and the agent has no new basis for judgment.
- The next step would change scope, permissions, dependencies, data model, deployment method, infra, or risk boundary.
- The next step could push, merge, deploy, run a migration, alter permissions, expose sensitive data, or change a system-level skill.
- The agent cannot explain the current state, what happened in the last iteration, and what it would do next.

### Escalate

The agent must stop / escalate to a human when:

- It is unclear whether the project is Level 4.
- The work involves production, permissions, authentication, payments, sensitive data, security, compliance, or system-level skill behavior.
- The next command may deploy, run a migration, write to a risky location, install dependencies, contact an external service, change authentication, change permissions, push, merge, or alter infra.
- Source documents contradict each other.
- There is no real verification signal.
- The same failure repeats.
- The loop output reveals a new capability that may require a future skill.

Escalation is not failure. It is the point where responsibility moves back to the human because the decision is outside the loop's approved boundary.

## Human Gates

A human gate is an explicit approval point before an agent takes a risky action. It protects the human responsibility boundary: the agent may recommend or prepare work, but a human remains responsible for approving actions that change risk.

Require a human gate before any action that may:

- Push to a shared branch.
- Merge code or documentation into a protected branch.
- Deploy or change deployment settings.
- Run a migration or alter data.
- Change permissions, authentication, secrets, or access control.
- Touch production, payments, security, compliance, or sensitive data.
- Change infra or external service configuration.
- Create, modify, or activate a system-level skill.
- Expand the loop's allowed actions or maximum iterations.

A human gate should state exactly what is being approved, what risk is involved, what rollback or recovery path exists, and what verification signal will be used after the action. If that cannot be stated clearly, do not proceed.

## Loop Output and Future Skill Opportunities

Loop output is useful beyond the immediate task. It can reveal repeated patterns that should become future skill opportunities.

Look for skill opportunities when a loop repeatedly produces:

- The same diagnosis steps.
- The same verification commands.
- The same handoff format.
- The same stop / escalate decision.
- The same checklist for human review.
- The same safe wrapper around infra, permission, migration, deploy, push, or merge workflows.

Do not turn every repeated action into a skill. A future skill is justified when the repeated work has a stable goal, stable inputs, stable verification signals, clear human gates, and safe stop / escalate behavior.

System-level skill changes are higher risk than ordinary documentation or project-local helpers. Treat them as Level 4 unless the project has defined a lower-risk policy, and require a human gate before creating, changing, installing, or activating them.

## Judgment Prompts

These prompts are for judging and designing a loop. They are not instructions to run the loop.

### Claude Code

```text
Read the current repo's README, STATE, optional SPEC/AGENTS files, and the relevant scripts and verification commands.

Decide whether the current task is suitable for a loop.

Requirements:
- Judge the project level first, from Level 1 to Level 4.
- Distinguish task execution loops, development feedback loops, and long-running automatic loops.
- If the goal, allowed actions, verification signal, stop conditions, or escalation conditions are unclear, recommend against a loop.
- If the task involves production, permissions, authentication, payments, sensitive data, security, compliance, or system-level skill behavior, stop and require human confirmation.
- Do not invent commands, tests, deploy processes, team rules, or risk assumptions that do not exist.
- Do not run the loop. Only output the judgment and a design draft.

Output:
- Whether you recommend a loop
- Loop type
- Reasoning
- Stop / Verify / Escalate
- Suggested maximum iterations
- Questions that need human confirmation
```

### Codex CLI

```text
Based on the current repo files and real commands, decide whether this task is suitable for a loop.

Read README.md first. If STATE.md, SPEC.md, or AGENTS.md exists, read those too.

Requirements:
- Judge the project level first, from Level 1 to Level 4.
- Distinguish task execution loops, development feedback loops, and long-running automatic loops.
- Use only information that really exists in the repo.
- If you cannot find a real verification signal, say so clearly and do not invent one.
- If the loop would need to run commands that may deploy, migrate data, write files, install dependencies, contact external services, change authentication or permissions, touch production, or touch sensitive data, stop and require human confirmation.
- Do not run the loop.

Output:
- Whether the task is suitable for a loop
- If not suitable, why
- If suitable, the loop type
- Stop / Verify / Escalate
- Maximum iterations
- Human gates
```

## Related Docs

- [Project level judgment](../guides/project-levels.md)
- [Infra by project level](../guides/infra-by-level.md)
