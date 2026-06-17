# Cookbook

The cookbook turns AI development workflow ideas into concrete scenarios for Claude Code and Codex CLI.

It is not a command reference. Each page describes when to use a scenario, what context to give the agent, how to write a useful prompt, what verification signal to expect, when to stop / escalate, and when a repeated pattern may be worth capturing as a skill.

## Positioning

- `en/playbooks/`: end-to-end workflows, such as starting a new project from an empty repo.
- `en/cookbook/`: task-level scenarios, such as orienting an agent in a repo or handing off a bounded task.
- `en/loops/`: guidance for deciding whether a task is safe and useful to run as an automated loop.
- `en/skill-workflow/`: guidance for deciding whether repeated work should become a skill.

Cookbook prompts are not magic commands. They still depend on real project context, clear boundaries, human ownership, and verifiable results.

## Current Scenarios

- [Repo Orientation](repo-orientation.md): have an agent understand an existing repo before editing files.
- [Task Handoff](task-handoff.md): hand Claude Code or Codex CLI a bounded task with explicit scope, forbidden actions, and verification.
- [Review And Fix](review-and-fix.md): handle specific test, lint, review, or CI feedback without turning it into an uncontrolled loop.
- [Context Refresh](context-refresh.md): recover project state after a long session, interruption, agent switch, or context compaction.
- [PR CI Workflow](pr-ci-workflow.md): organize agent collaboration around PR preparation, CI reading, review handling, and pre-merge checks.
- [Multi-Agent Delegation](multi-agent-delegation.md): split independent work across multiple agents while preserving ownership, review, integration, and final verification.

## Extension Principles

Add a cookbook scenario because a real task keeps recurring, not because you want a larger command catalog. A scenario is ready to stand alone only when its inputs, boundaries, verification signals, human gates, and stop / escalate conditions can be written clearly.

## How To Use A Scenario

1. Decide the project level.
2. Read the current project state.
3. Choose the matching cookbook scenario.
4. Fill in the inputs, constraints, and forbidden actions.
5. Run the matching Claude Code or Codex CLI prompt.
6. Verify the result with real signals.
7. Record status, stop or escalate to a human when needed, and consider skill capture only when the pattern repeats.

## Related Docs

- [Project Levels](../guides/project-levels.md)
- [Infra By Level](../guides/infra-by-level.md)
- [Infra Checklists](../guides/infra-checklists.md)
- [Start A New Project Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
