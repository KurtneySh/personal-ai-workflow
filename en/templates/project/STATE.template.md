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
