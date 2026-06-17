# AI Workflow Handbook

This handbook helps developers build AI-assisted development workflows that are readable, executable, verifiable, and reusable. It treats an agent as part of a project operating system: humans remain responsible for goals, judgment, risk, and final decisions, while infra, loops, skills, handoff, and verification signals make agent work easier to direct and inspect.

## 3-Minute Reading Path

1. Start the English v1 draft core path with [Project Levels](en/guides/project-levels.md) to decide whether your project is Level 1-4.
2. Read [Infra By Level](en/guides/infra-by-level.md) to choose the smallest useful AI workflow infra.
3. Read [Infra Checklist](en/guides/infra-checklists.md) to add context, rules, verification signal, collaboration boundaries, risk controls, and loop constraints.
4. Use [Loops](en/loops/README.md) as an upcoming core translation in this pass before turning repeated agent work into a loop, especially when defining stop / escalate conditions and human gate checkpoints.
5. Use [Skill Workflow](en/skill-workflow/README.md) as an upcoming core translation in this pass when a repeated handoff should become a reusable skill.

## Repository Map

- `en/guides/`: English v1 draft core concepts for project levels, infra recommendations, and checklists.
- `en/playbooks/`: planned English section for end-to-end workflows from goal to completion.
- `en/cookbook/`: planned English section for scenario-driven Claude Code and Codex CLI usage.
- `en/loops/`: core translation pass section for loop readiness, loop design, stop / escalate rules, and human gates.
- `en/skill-workflow/`: core translation pass section for when and how to capture reusable skills.
- `en/cases/`: planned English section for practical case studies and case templates.
- `en/templates/`: planned English section for document templates for project workbenches.
- `en/reference/`: planned English section for concepts, philosophy, glossary, and appendix material.
- `zh/`: Chinese v1 source content.
- `docs/repo/`: repository maintenance notes and translation tracking.

## Current Language Status

Chinese v1 is the source of truth. This pass is drafting the English v1 core concepts so the public entrypoint can be English-first while the Chinese source remains available for authoritative reference.

For the source entrypoint, see [Chinese source entrypoint](README.zh.md).
