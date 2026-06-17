# Review And Fix

Use this scenario to hand Claude Code or Codex CLI specific test, lint, typecheck, review, documentation review, or CI feedback.

The goal is not to let the agent see a failure and automatically loop forever. The goal is to understand the feedback, diagnose the likely cause, make or propose a bounded fix, and verify the result with a real signal.

## Purpose

Review And Fix turns concrete feedback into a bounded repair task.

After the work, the agent should explain the feedback, the likely cause, the files changed, the verification result, whether the original feedback is resolved, and any residual risk.

## When To Use

Use this scenario for:

- Test failures.
- Lint or typecheck errors.
- Review comments that point to a specific issue.
- CI failures with readable logs.
- Documentation review notes that clearly identify what needs correction.

Do not use it when:

- The feedback is only "this feels wrong" or the preference is unclear.
- The feedback requires a product or architecture direction decision.
- The work needs an automated loop and the loop boundary has not been designed.
- The fix requires expanded scope, data model changes, permission changes, deployment, or production risk.
- The work has Level 4 risk without a human gate.

## What The Agent Should Inspect First

Before changing files, the agent should inspect:

- The original failure log, CI summary, review comment, or documentation note.
- The command that produced the failure, if available.
- The files named by the feedback.
- Adjacent files needed to understand the behavior.
- Relevant tests, lint config, type config, or CI workflow files.
- Project state and ownership notes that define who owns CI, review decisions, deployment, or integration.

The agent should not start from a guessed fix. It should first classify the feedback as test, lint, typecheck, review, CI, or docs feedback and identify the smallest plausible repair.

## Inputs

Provide at least:

- Feedback source.
- Full failure log, review comment, or CI summary.
- Project level.
- Likely files or directories.
- Allowed actions.
- Forbidden actions.
- Verification command or verification signal.
- Maximum fix scope.
- Human confirmation gates.

"Fix the feedback" is not enough. The agent needs evidence, boundaries, and a verification signal.

## Feedback Fix Template

Copy this template into an issue, review reply, `STATE.md`, or the agent conversation.

```markdown
## Feedback Fix

- Feedback source:
- Feedback text or log:
- Project level:
- Likely files or areas:
- Allowed actions:
- Forbidden actions:
- Verification command or signal:
- Maximum fix scope:
- Human confirmation gates:
- Notes or hypotheses:
```

Fill it carefully:

- `Feedback source` should say whether the feedback came from test, lint, typecheck, review, CI, or docs review.
- `Feedback text or log` should include the original failure text when possible, not only a summary.
- `Allowed actions` must be narrow enough that the agent cannot expand scope on its own.
- `Forbidden actions` must name the boundaries around production, permissions, auth, payment, sensitive data, security, deployment, migrations, and external services.
- `Verification command or signal` must be a real project signal. If none exists, write "no real verification signal currently exists."
- `Maximum fix scope` should name the largest allowed file, directory, or behavior change.

## Diagnosis, Fix, Verification

Keep the work separated:

1. Diagnosis: restate the original feedback, classify it, inspect the relevant context, and identify the likely cause.
2. Fix: make the smallest allowed change that addresses that cause.
3. Verification: rerun the original failing command or the handoff's verification command, then report whether the same failure repeated, a new failure appeared, verification passed, or verification could not be run.

This separation prevents the agent from hiding uncertainty inside a broad edit. It also makes review decisions and CI ownership clearer: humans still own unclear review calls, risky fixes, integration decisions, push, merge, deployment, migrations, permission changes, and sensitive data handling.

## Claude Code Prompt

```text
Please handle the concrete feedback described in this Feedback Fix.

Requirements:
- First read the feedback and relevant project context.
- Restate the failure or review request before changing files.
- Classify the feedback type: test / lint / typecheck / review / CI / docs.
- Before editing, explain the likely cause and the smallest fix plan.
- Only perform actions allowed by the Feedback Fix.
- Do not perform broad refactors that were not authorized.
- Do not invent commands, tests, deployment flows, team rules, or risk assumptions.
- If the fix requires dependency installation, deployment, migration, permission changes, external service access, or touching auth, payment, production, sensitive data, security, or compliance, stop and ask for human confirmation.
- After the fix, run or report the real verification signal.
- If verification cannot be run, explain why.
- Finish with changed files, fix explanation, verification result, whether the original feedback is resolved, residual risk, and questions needing human confirmation.
- Do not automatically enter a loop. If another repair round seems needed, first explain whether loop conditions are met.

Feedback Fix:
<paste the Feedback Fix template content>
```

## Codex CLI Prompt

```text
Please handle the following Feedback Fix in the current repo.

Requirements:
- First read the feedback, related files, and necessary project context.
- Before editing, restate the original failure, review request, or CI issue.
- Classify the feedback type and explain the likely cause.
- Implement only the smallest necessary fix.
- Do not expand scope.
- Do not invent commands, tests, deployment flows, team rules, or risk assumptions.
- Do not run deploy, migration, permission, auth, payment, production, or sensitive data commands unless the Feedback Fix explicitly approves them.
- After the fix, run the same failing command or the verification command named in the handoff.
- If verification fails, distinguish between the same failure repeating, a new error, and verification being unable to run.
- Output changed files, verification result, whether the original feedback is resolved, residual risk, and next step.
- Do not automatically loop. If you think a loop is needed, output only the loop judgment and the boundaries that need human confirmation.

Feedback Fix:
<paste the Feedback Fix template content>
```

## Verification Signals

Prefer verifying the original feedback:

- Rerun the same failing command, or run the handoff's specified verification command.
- Check that the original review comment has been answered.
- Run `git diff --check`.
- Check that changed files stay inside the allowed scope.
- If verification cannot run, record the reason.

Report the result as one of:

- `verification passed`
- `verification failed with a new error`
- `verification could not be run`
- `same failure repeated`

Common checks:

```bash
git status --short
git diff --check
```

Expected:

- `git status --short` shows only expected files.
- `git diff --check` has no output.

## Stop / Escalate Conditions

The agent must stop and return to the human when:

- The feedback is vague or only a subjective preference.
- The fix requires changing product direction or architecture direction.
- The same failure repeats after one bounded fix.
- There is no real verification signal.
- The fix needs a wider scope than allowed.
- The fix requires dependency installation, deployment, migration, permission changes, or external service access.
- The fix touches production, permissions, auth, payment, sensitive data, security, or compliance.
- Documentation, tests, review, or CI feedback contradict each other.
- The agent cannot explain the cause or next step.

Unclear review decisions and risky fixes are human gates. The agent may summarize options and recommend a path, but the human owns the decision before implementation continues.

## When This Becomes A Loop Or Skill

Do not start with a loop by default.

A one-step fix is enough when:

- The feedback is specific.
- Allowed actions are clear.
- A real verification command can confirm the result.
- The fix scope is small.

Consider a development feedback loop only when all of these are explicit:

- Goal.
- Allowed actions.
- Forbidden actions.
- Verification signal.
- Stop conditions.
- Escalation conditions.
- Maximum rounds.

Read [Loops](../loops/README.md) before designing a loop. If any loop boundary is unclear, continue as a human-confirmed single-step fix.

Consider a project-level skill when:

- The same kind of feedback keeps recurring.
- The fix flow has stable inputs and verification.
- The project has fixed review, CI, or lint conventions.
- The agent repeatedly needs the same stop / escalate rules.
- A feedback-handling pattern works across several tasks.

Prefer project-level skill capture first. Only record a system-level skill candidate when the pattern is stable across multiple projects, and only after human review.

## Related Docs

- [Project Levels](../guides/project-levels.md)
- [Infra By Level](../guides/infra-by-level.md)
- [Infra Checklists](../guides/infra-checklists.md)
- [Start A New Project Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
- [Task Handoff](task-handoff.md)
