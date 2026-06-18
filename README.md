# AI Workflow Handbook

This handbook helps developers build AI-assisted development workflows that are readable, executable, verifiable, and reusable. It treats an agent as part of a project operating system: humans remain responsible for goals, judgment, risk, and final decisions, while infra, loops, skills, handoff, and verification signals make agent work easier to direct and inspect.

## 3-Minute Reading Path

1. Start the English v1 draft core path with [Project Levels](en/guides/project-levels.md) to decide whether your project is Level 1-4.
2. Read [Infra By Level](en/guides/infra-by-level.md) to choose the smallest useful AI workflow infra.
3. Read [Infra Checklist](en/guides/infra-checklists.md) to add context, rules, verification signal, collaboration boundaries, risk controls, and loop constraints.
4. Use the v1 draft [Start A New Project Playbook](en/playbooks/start-a-new-project.md) when setting up a new repo or retrofitting an existing one.
5. Use the v1 draft [Cookbook](en/cookbook/README.md) for concrete Claude Code and Codex CLI scenarios such as repo orientation, task handoff, review handling, PR / CI work, and multi-agent delegation.
6. Read the v1 draft [Loops](en/loops/README.md) guidance before turning repeated agent work into a loop, especially when defining stop / escalate conditions and human gate checkpoints.
7. Read the v1 draft [Skill Workflow](en/skill-workflow/README.md) guidance when a repeated handoff should become a reusable skill.
8. Read the v1 draft [Reference](en/reference/README.md) when you want the appendix-style working philosophy behind the handbook.

## Repository Map

- `en/guides/`: English v1 draft core concepts for project levels, infra recommendations, and checklists.
- `en/playbooks/`: English v1 draft end-to-end workflows from goal to completion.
- `en/cookbook/`: English v1 draft scenario guidance for Claude Code and Codex CLI usage.
- `en/loops/`: English v1 draft guidance for loop readiness, loop design, stop / escalate rules, and human gates.
- `en/skill-workflow/`: English v1 draft guidance for when and how to capture reusable skills.
- `en/cases/`: future English pass for practical case studies and case templates.
- `en/templates/`: English v1 draft document templates for project workbenches.
- `en/reference/`: English v1 draft appendix material for AI development workflow philosophy and best practices.
- `zh/`: Chinese v1 source content.
- `docs/repo/`: repository maintenance notes and translation tracking.

## Current Language Status

Chinese v1 is the source of truth. English v1 draft content now covers the public entrypoint, core concepts, the new-project playbook, cookbook scenarios, loops, skill workflow, project workspace templates, and reference material, while cases remain a later English pass.

For the source entrypoint, see [Chinese source entrypoint](README.zh.md).
