# Infra By Level

AI workflow infra is useful when it gives an agent stable context, executable feedback, and clear boundaries. More files are not automatically better.

Start from the project level, then add the smallest set of docs, checks, and collaboration rules that make the work easier to repeat and safer to review.

## Why Overbuilding Infra Costs

Infra has maintenance cost. Every extra file can become stale, contradict another file, or make the agent follow ceremony instead of the real project state.

Overbuilding usually shows up as:

- Docs that describe an ideal workflow nobody follows.
- Checklists that are copied across projects but do not match the risk.
- Specs, plans, or issue templates that hide the actual decision owner.
- Verification steps that are too slow or vague to run consistently.

The right infra earns its place by reducing repeated explanation, producing a clearer verification signal, improving handoff, or controlling a real risk.

## Level 1: Personal Experiment Or Small Script

Minimum useful infra:

- `README.md`: what the project is and how to run it.
- A current status note, either in `README.md` or a small `STATE.md`.
- At least one verification command, such as test, run, lint, or an equivalent script.

How common infra appears:

- README: short and practical.
- Status docs: optional, but useful when the work spans more than one session.
- Specs and plans: usually unnecessary unless the experiment has a sharp goal.
- Tests: one smoke test or runnable example may be enough.
- CI: usually not needed.
- Review: self-review plus the verification command.
- Issue tracking: usually a task list or chat brief.

Agent handoff is minimal at this level. The main expectation is that the agent can understand the project and produce a basic verification signal before handing work back.

## Level 2: Personal Project Expected To Continue

Minimum useful infra:

- Everything from Level 1.
- `SPEC.md`: goal, scope, non-goals, and key decisions.
- `AGENTS.md`: project-specific agent rules.
- A place for important decisions, such as `docs/decisions/`.
- Tests or explicit verification scripts.

How common infra appears:

- README: stable entrypoint and setup path.
- Status docs: current state, next steps, known issues.
- Specs: define boundaries so the agent does not expand the project by accident.
- Plans: useful for larger changes, migrations, or multi-step refactors.
- Tests: expected for behavior that should survive future edits.
- CI: optional, but valuable once local verification becomes easy to forget.
- Review: human review of important diffs and decisions.
- Issue tracking: lightweight task briefs or reusable prompts.

Agent handoff becomes more important because the project has memory. The agent should know where to read project state, where to write follow-up notes, and which verification commands matter.

## Level 3: Collaborative Project

Minimum useful infra:

- Everything from Level 2.
- `CONTRIBUTING.md`.
- `CODEOWNERS` or another ownership document.
- PR checklist.
- CI checks.
- Issue or task template.
- Handoff notes.
- A delegation contract for multi-agent work.

How common infra appears:

- README: entrypoint for contributors, not the only source of truth.
- Status docs: shared project state and active work.
- Specs: reviewed before major implementation.
- Plans: used to split work, define acceptance criteria, and support handoff.
- Tests: local and CI verification are expected for agent changes.
- CI: required as a shared verification signal.
- Review: PR review, ownership checks, and human judgment over key decisions.
- Issue tracking: tasks carry context, acceptance criteria, and stop / escalate conditions.

At this level, handoff is a first-class requirement. Agents should leave enough context for a reviewer or another agent to continue, and verification expectations increase from "I ran a command" to "the work can be reviewed against shared checks."

## Level 4: Production, Business-Critical, Or High-Risk Project

Minimum useful infra:

- Everything from Level 3.
- Risk register.
- Security and privacy checklist.
- Rollback plan.
- Human approval gates.
- Deployment checklist.
- Incident or audit notes.
- Loop stop conditions.
- Permission boundaries for agents.

How common infra appears:

- README: points to operating docs, ownership, and safety boundaries.
- Status docs: include release state, known risks, and active mitigations.
- Specs: include risk, permissions, rollout, rollback, and human gates.
- Plans: identify irreversible steps, stop / escalate points, and required approvals.
- Tests: include automated checks plus manual or staging verification where needed.
- CI: required, with checks that match release risk.
- Review: human approval is required for architecture, permissions, dependencies, deploys, and rollback.
- Issue tracking: tickets preserve audit context, decisions, and verification evidence.

Agent handoff and verification expectations are highest here. An agent can gather context, propose changes, implement bounded work, and collect verification signals, but humans keep responsibility for approval, release, rollback, and high-risk judgment.

## Using This Guide

Do not create files just to satisfy a level. Choose the level as a default, then use [Infra Checklist](infra-checklists.md) to add capability where the project actually needs it.
