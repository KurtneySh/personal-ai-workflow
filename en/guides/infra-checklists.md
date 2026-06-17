# Infra Checklist

This checklist is organized by capability, not by filename.

The same capability can live in different files depending on the project. The point is to help an agent understand, execute, verify, collaborate, reuse, and stop at the right time.

These items are optional additions. They are not a required maturity model, and a project does not need every item to be healthy.

## Checklist Item Schema

For any infra item you add, make sure you can answer:

- Item
- Purpose
- Recommended levels
- Required when
- Example file
- How AI uses it
- Common mistakes

## Context Infra

Purpose: help the agent understand what the project is, what state it is in, and where the boundaries are.

Basic items:

- `README.md`
- `STATE.md`

Extended items:

- `SPEC.md`
- `docs/decisions/`
- architecture notes
- glossary

Common mistakes:

- README only explains installation, not project purpose.
- STATE is not kept current.
- Important decisions exist only in chat history.

## Instruction Infra

Purpose: tell the agent how to work inside this project.

Basic items:

- Level 1 can keep simple rules in `README.md`.
- Level 2 and above usually benefit from `AGENTS.md`.

Extended items:

- coding conventions
- dependency policy
- branch and commit policy
- forbidden actions
- human gate rules

Common mistakes:

- Writing a generic prompt instead of project-specific rules.
- Omitting verification commands.
- Failing to say which actions require human confirmation.

## Verification Infra

Purpose: give the agent a clear verification signal so it can judge whether a change worked.

Basic items:

- At least one explicit verification command.

Extended items:

- tests
- lint
- typecheck
- build
- browser verification
- CI
- smoke test checklist

Common mistakes:

- Asking the agent to decide whether something "looks fine."
- Not recording the expected result of a verification command.
- Letting local verification and CI drift apart.

## Collaboration Infra

Purpose: reduce unclear ownership, review gaps, and merge risk when people or multiple agents work together.

Basic items:

- ownership
- PR checklist
- handoff notes

Extended items:

- issue template
- review checklist
- delegation contract
- worktree policy
- reducer role
- conflict resolution notes

Common mistakes:

- Multiple agents edit the same files without ownership.
- No reducer is responsible for final integration.
- Review relies on summaries instead of the key diff and verification evidence.

## Risk Infra

Purpose: control permissions, data exposure, deployment risk, and irreversible operations.

Basic items:

- risk register
- rollback plan
- security and privacy checklist
- human approval gates

Extended items:

- threat model
- deployment checklist
- incident notes
- audit trail
- permission boundaries for agents

Common mistakes:

- Letting an agent combine private data, untrusted input, and external communication without review.
- Having no rollback plan.
- Failing to define which operations must stop for human approval.

## Loop Infra

Purpose: decide whether automated iteration is worth using and how it should stop safely.

Extended items:

- loop readiness checklist
- iteration budget
- verification command
- stop conditions
- escalation rules
- human approval gates

Common mistakes:

- Starting a loop without a verification command.
- Running without a maximum iteration count.
- Allowing an agent to continue automatically through high-risk operations.

## Skill Infra

Purpose: turn repeated successful workflows into reusable assets.

Extended items:

- skill workflow notes
- skill index
- reusable prompts
- playbook links
- case notes

Common mistakes:

- Rewriting the same prompt every time.
- Saving only the result, not the workflow or judgment behind it.
- Not recording failure modes that future agents should avoid.

## Task Flow Infra

Purpose: align the human and the agent on goals, acceptance criteria, handoff, and stop / escalate rules.

Basic items:

- task brief
- acceptance criteria

Extended items:

- issue template
- PR checklist
- review checklist
- handoff notes
- changelog

Common mistakes:

- Tasks say only "improve this" or "clean this up."
- Completion criteria are missing.
- Stop / escalate conditions are not written down.
