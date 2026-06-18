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
