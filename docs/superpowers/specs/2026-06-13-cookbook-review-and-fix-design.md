# Cookbook Review And Fix Scenario Design

Date: 2026-06-13

## Goal

Add a third Chinese cookbook scenario for handling test, lint, review, and CI feedback with Claude Code or Codex CLI.

The scenario should help readers hand concrete feedback to an agent, ask it to diagnose before editing, make a bounded fix, verify with real signals, and know when to stop or escalate. It should also clarify when a feedback-fix task is a one-shot fix versus when it may need a deliberately designed loop.

## Scope

Create one scenario page and link it from the cookbook entrypoint:

```text
zh/cookbook/review-and-fix.md
zh/cookbook/README.md
```

The new page should follow the existing cookbook style used by:

- `zh/cookbook/repo-orientation.md`
- `zh/cookbook/task-handoff.md`

## Non-Goals

This first version will not:

- Create an automatic test-fix or CI-fix loop.
- Add scripts or executable automation.
- Cover PR-specific or GitHub-specific workflows in depth.
- Cover every possible review platform or CI provider.
- Add English translation beyond existing English stub files.
- Create a formal `zh/cases/` module.

PR and CI workflows can later become dedicated cookbook pages if real usage shows enough repeated structure.

## Scenario Definition

Create:

```text
zh/cookbook/review-and-fix.md
```

Purpose:

Help a human ask an agent to process concrete feedback such as:

- failing tests,
- lint or typecheck errors,
- review comments,
- CI failures,
- documentation review notes.

The scenario should require concrete feedback input. It should not be used for vague dissatisfaction, broad architecture debates, or unclear product direction.

## Required Sections

The page should include:

- Purpose
- When to Use
- When Not to Use
- Inputs
- Feedback Fix Template
- Claude Code Prompt
- Codex CLI Prompt
- Expected Output
- Verification
- Stop and Escalate
- When This Becomes A Loop
- Skill Capture Signals
- Related Docs

## Inputs

The scenario should ask the human to provide:

- feedback source,
- exact failure log or review comment,
- project level,
- files or areas likely involved,
- allowed actions,
- forbidden actions,
- verification command or signal,
- maximum fix scope,
- human confirmation gates.

The page should make clear that "fix the feedback" is not enough. The agent needs evidence, scope, and verification.

## Feedback Fix Template

Include a copyable template with fields:

- Feedback source
- Feedback text or log
- Project level
- Likely files or areas
- Allowed actions
- Forbidden actions
- Verification command or signal
- Maximum fix scope
- Human confirmation gates
- Notes or hypotheses

The template should be suitable for issue comments, task descriptions, `STATE.md`, or direct prompts.

## Prompt Behavior

Claude Code and Codex CLI prompts should ask the agent to:

- read the feedback and relevant project context,
- restate the failure or requested change,
- classify the feedback type,
- identify the likely cause before editing,
- propose a bounded fix,
- avoid broad refactors unless explicitly authorized,
- avoid inventing commands, tests, deployment flows, team rules, or risk assumptions,
- avoid risky commands without human confirmation,
- run or report the real verification signal,
- summarize changed files, verification result, and residual risk.

Prompts should not tell the agent to loop automatically.

## Verification

Verification should include:

- re-run the same failing command or relevant verification command,
- confirm the original failure or review comment is addressed,
- run `git diff --check`,
- inspect changed files for scope creep,
- document if verification cannot be run.

The page should distinguish:

- "verification passed",
- "verification failed with a new error",
- "verification could not be run",
- "same failure repeated".

## Stop And Escalate

The page should tell the agent to stop and escalate when:

- feedback is ambiguous or subjective,
- the fix requires changing product direction or architecture,
- the same failure repeats after a bounded attempt,
- no real verification signal exists,
- required changes exceed the allowed scope,
- changes touch production, permissions, authentication, payment, sensitive data, security, compliance, deployment, migrations, or external services,
- docs or feedback contradict each other,
- the agent cannot explain the cause or next step.

## Loop Boundary

The page should include a section named `什么时候转成 loop / When This Becomes A Loop`.

It should say:

- Do not start with a loop by default.
- A one-shot fix is enough when feedback is concrete and a single verification command can confirm it.
- Consider a development feedback loop only when the goal, allowed actions, verification signal, stop conditions, escalation conditions, and maximum iterations are explicit.
- Use `../loops/README.md` before designing the loop.
- If any loop boundary is missing, continue as human-confirmed single steps.

## Skill Capture Signals

The page should explain that this scenario may become a skill candidate when:

- the same class of feedback appears repeatedly,
- the fix process has stable inputs and verification,
- the project has recurring review or CI conventions,
- agents repeatedly need the same stop/escalate rules,
- the feedback handling pattern works across several tasks or projects.

It should say project-level skill comes first, and system skill promotion remains a human-reviewed candidate.

## Cookbook Entrypoint Update

Update `zh/cookbook/README.md`:

- Add `Review And Fix` to current scenarios.
- Remove or adjust the planned scenario bullet for review/test/lint feedback so it no longer describes a future missing page.

The entrypoint should not imply every CI or PR workflow is now covered in depth.

## Related Docs

The new page should link to:

- `../guides/project-levels.md`
- `../guides/infra-by-level.md`
- `../guides/infra-checklists.md`
- `../playbooks/start-a-new-project.md`
- `../loops/README.md`
- `../skill-workflow/README.md`
- `task-handoff.md`

## Acceptance Criteria

- `zh/cookbook/review-and-fix.md` exists.
- The page follows the cookbook scenario structure.
- The page includes a copyable Feedback Fix Template.
- The page includes Claude Code and Codex CLI prompts.
- Prompts ask the agent to diagnose before editing.
- Prompts forbid invented commands, tests, deployment flows, team rules, and risk assumptions.
- Prompts do not ask agents to run automatic loops.
- The page includes verification, stop/escalate, loop boundary, and skill capture sections.
- The loop boundary links conceptually to `zh/loops/README.md`.
- `zh/cookbook/README.md` links the new scenario as current.
- English stub files are unchanged.
- Related links point to existing files.
- Markdown whitespace checks pass.
