# Project Levels

Project levels are defaults for choosing AI workflow infra. They are not permanent labels, maturity badges, or a reason to add process for its own sake.

Use the lowest level that gives the agent enough context, instructions, and verification signal to work safely. Move up when the project needs more continuity, collaboration, review, release control, or risk management.

Ask these questions in order:

1. Will this project be maintained over time?
2. Are multiple people involved, including review or handoff?
3. Does it touch production systems, permissions, sensitive data, payments, authentication, security, or compliance?
4. Does it need CI/CD, release control, rollback, or auditability?

## Level 1: Personal Experiment Or Small Script

Use Level 1 for one-off scripts, quick prototypes, learning projects, and small experiments where manual repair or rebuilding is cheap.

The goal is simple:

- Help the agent understand what the project is.
- Give the agent at least one way to run or verify the work.
- Avoid adding long-lived process to a short-lived project.

Move up from Level 1 when:

- The project starts getting repeated changes.
- You keep re-explaining the same context to the agent.
- Mistakes become hard to track from memory.

## Level 2: Personal Project Expected To Continue

Use Level 2 for personal tools, side projects, and solo codebases that you expect to maintain over time.

The goal is to write down enough project memory that the agent can participate repeatedly without starting from zero:

- Goals, scope, current status, and important decisions live in files.
- The agent can read, change, and verify locally in a closed loop.
- Recurring instructions become project rules instead of chat-only context.

Move up from Level 2 when:

- Collaborators join the project.
- Work needs review, ownership, or handoff.
- CI, release, or rollback starts to matter.

## Level 3: Collaborative Project

Use Level 3 when multiple people maintain the project, PR review is expected, ownership matters, or agent changes need stable review and handoff.

The goal is to make collaboration explicit:

- Ownership, review, CI, and handoff are documented.
- Parallel agent work has boundaries that reduce context conflicts.
- Humans keep responsibility for judgment; agents handle execution that can be inspected and verified.

Move up from Level 3 when:

- Changes affect production deployment.
- The project touches permissions, sensitive data, authentication, payments, security, or compliance.
- A failed change would create high-cost damage.

## Level 4: Production, Business-Critical, Or High-Risk Project

Use Level 4 for production systems, high-permission systems, business-critical workflows, regulated environments, and projects that affect many users or important operations.

The goal is to make risk, authority, and recovery explicit:

- Risks, permissions, approvals, rollback, and audit needs are documented.
- Loops have hard stop / escalate conditions.
- Human gates remain in front of architecture decisions, permissions, dependencies, releases, and rollback.

An agent can still help at Level 4, but it should not become the final authority for irreversible or high-risk decisions.

## What Changes When A Project Moves Up

Moving up a level usually changes the infra around the work, not the identity of the project.

- Context becomes less conversational and more file-backed.
- Instructions become more project-specific and less prompt-specific.
- Verification moves from one local command toward tests, CI, review, and release checks.
- Handoff becomes explicit because other people or agents need to continue the work.
- Human gates appear earlier around risk, authority, deployment, and rollback.
- Stop / escalate rules become clearer because the cost of a wrong loop is higher.

Start small. Add the next layer only when it reduces repeated explanation, prevents confusion, improves verification, or controls a real risk.
