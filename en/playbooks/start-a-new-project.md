# Start A New Project Playbook

This playbook turns a project into a workspace an agent can read, act on, verify, and hand off.

It covers two paths:

- Path A: start from an empty repo.
- Path B: retrofit an existing project.

## Purpose

Use this playbook to move from a project idea or existing repo to the first stable setup where an agent can participate in development without needing the same context explained every time.

It connects these materials:

- `en/guides/project-levels.md`
- `en/guides/infra-by-level.md`
- `en/guides/infra-checklists.md`
- `en/templates/project/README.md` when available; otherwise use the matching source template under `zh/templates/project/` with human review.

## When to Use

Use it when:

- You are starting a personal or team project.
- You are adding AI workflow infra to an existing project.
- You want Claude Code, Codex CLI, or another agentic tool to work inside the repo.
- You want to reduce repeated context-setting before each task.

Do not use it directly when:

- The work is already in a PR or CI fix stage.
- The main problem is designing complex multi-agent ownership.
- The main problem is designing an automatic loop.

Those scenarios should use a later playbook or cookbook workflow.

## Success Criteria

When this playbook is complete:

- `README.md` has been created or updated.
- `STATE.md` has been created or updated.
- Level 2+ projects have `SPEC.md`.
- Level 2+ projects have `AGENTS.md`.
- At least one real verification command can run.
- `STATE.md` explains the current phase, next step, and blockers.
- Level 1 projects can record verification requirements, forbidden actions, and done criteria in `README.md` or `STATE.md`.
- Level 2+ projects should record verification requirements, forbidden actions, and done criteria in `AGENTS.md`.
- A human has reviewed AI-generated commands, tests, deployment steps, and team rules.
- An agent can use these files to understand the project and take over a low-risk small task.

## Stop and Escalate

Stop and ask for human judgment when:

- The project level is unclear, especially if it may be Level 4.
- The project touches production, permissions, authentication, payments, sensitive data, security, or compliance.
- There is no real runnable verification command.
- The AI produced commands, tests, deployment flow, or team rules that do not exist in the repo.
- `README.md`, `SPEC.md`, `STATE.md`, and `AGENTS.md` contradict each other.
- The repo already has many uncommitted changes and this setup would touch the same files.
- The next step requires new dependencies, data model changes, permission boundary changes, or broad refactoring.
- The agent cannot explain the current project state or next step.

## Path A: Start From An Empty Repo

### A1. Write One Project Goal

- Input: A project idea.
- Action: Write one sentence that states the problem this project solves.
- Output: One project goal.
- Verification: The sentence answers "why does this project exist?"
- AI Prompt:

```text
I am creating a new project. Based on the idea below, compress it into one project goal.
Do not expand the feature scope. Do not add requirements I did not mention.

Project idea:
<write your idea>
```

### A2. Judge the Project Level

- Input: The one-sentence project goal; whether it will be maintained long-term, involve multiple people, or touch production or sensitive risk.
- Action: Read `en/guides/project-levels.md`; judge Level 1-4.
- Output: Project level, reasoning, and questions that need human confirmation.
- Verification: If the project may be Level 4, stop and require human confirmation.
- AI Prompt:

```text
Read `en/guides/project-levels.md` and judge whether the project is Level 1-4.

Input:
- Project goal: <one-sentence project goal>
- Long-term maintenance: <yes/no/uncertain>
- Multi-person collaboration: <yes/no/uncertain>
- Production or sensitive risk: <yes/no/uncertain>

Output:
- Recommended project level: Level 1/2/3/4
- Reasoning
- Questions that need human confirmation

If the project may involve production, permissions, authentication, payments, sensitive data, security, or compliance, stop and ask for human confirmation.
Do not assume risk is absent. When information is missing, write "needs confirmation."
```

### A3. Choose the Minimal Useful Infra

- Input: Project level.
- Action: Read `en/guides/infra-by-level.md`; list the minimum files or directories needed for the current level.
- Output: The workspace file and directory list for this project.
- Verification: The list covers the minimum for the selected level. Level 1 includes at least `README.md`, `STATE.md`, and one verification command. Level 2 adds at least `SPEC.md`, `AGENTS.md`, `docs/decisions/`, and tests or an explicit verification script. Level 3 includes collaboration, CI, tasks, handoff, and a multi-agent delegation contract. Level 4 includes risk, security, privacy, rollback, human approval, deployment, audit, loop stop, and agent permission boundary materials.
- AI Prompt:

```text
Read `en/guides/infra-by-level.md` and list the minimum workspace files or directories for the current project level.

Project level: <Level 1/2/3/4>

Judge by level:
- Level 1: `README.md`, `STATE.md`, at least one verification command.
- Level 2: all Level 1 items, plus `SPEC.md`, `AGENTS.md`, `docs/decisions/`, tests or an explicit verification script.
- Level 3: all Level 2 items, plus `CONTRIBUTING.md`, `CODEOWNERS` or ownership docs, PR checklist, CI checks, issue / task template, handoff notes, and a multi-agent delegation contract.
- Level 4: all Level 3 items, plus a risk register, security / privacy checklist, rollback plan, human approval gates, deployment checklist, incident / audit notes, loop stop conditions, and permission boundaries for agents.

List only the minimal infra for the selected level. Keep project-specific optional additions separate. Do not add unnecessary optional files.
Output the file list, each file's purpose, and which items need human confirmation.
```

### A4. Apply the Project Template

- Input: The workspace file list.
- Action: The project template covers four core workspace files: `README.md`, `STATE.md`, Level 2+ `SPEC.md`, and Level 2+ `AGENTS.md`. Other minimal infra selected in A3 should be created as a minimal stub, connected to existing project tooling, or marked as `needs confirmation` when context is missing.
- Output: Minimal infra files and directories created or marked as needing confirmation.
- Verification: The full A3 minimal infra list is preserved; every created file contains real project context; no generic fill-in-later text remains.
- AI Prompt:

```text
Use the corresponding template under `en/templates/project/` if available. If the English template is missing, use `zh/templates/project/` as the source template and adapt it into English.
Create draft workspace files for this project and preserve the complete minimal infra list from A3.

Files to create:
<file list>

Project context:
<project goal, project level, known scope, known constraints>

Requirements:
- Use the project templates for the four core workspace files: `README.md`, `STATE.md`, Level 2+ `SPEC.md`, and Level 2+ `AGENTS.md`.
- For other minimal infra items from A3 that do not have a project template: create a minimal stub or connect to existing project tooling only when there is enough context; otherwise mark the item as "needs confirmation."
- Preserve the full minimal infra selected in A3. Do not output only the core workspace files.
- Start from the template content and trim optional sections by project level.
- Every file must contain real project context.
- Write "needs confirmation" for missing information.
- State your assumptions explicitly.
- Do not invent commands, tests, deployment flows, or team rules.
```

### A5. Human Review of Key Fields

- Input: AI-generated workspace file drafts.
- Action: A human reviews the project goal, scope, verification command, forbidden actions, and done criteria.
- Output: Human-confirmed workspace files.
- Verification: Commands and rules come from the real project or explicit human judgment; the AI has not invented tests, deployment flow, or team rules.
- AI Prompt:

```text
Generate only a review checklist. Do not modify files.

Focus on:
- Whether the project goal is accurate
- Whether the scope is too broad or too narrow
- Whether commands, tests, and deployment flow come from the real project
- Whether team rules come from explicit human judgment
- Whether forbidden actions are clear
- Whether done criteria are verifiable

If you find AI-invented commands, tests, deployment flow, or team rules, mark them as needing human confirmation.
```

### A6. Establish At Least One Verification Command

- Input: Current project files; available scripts or toolchain.
- Action: Inspect real project files, scripts, and tooling. If a real command exists, automatically run only read-only or low-risk local minimal verification. If the command may deploy, migrate data, write files, install dependencies, contact external services, change authentication or permissions, or touch production or sensitive data, stop and request human confirmation first. After human confirmation, record the command and result in `README.md` or `STATE.md` for Level 1, or in the appropriate `README.md` / `STATE.md` / `AGENTS.md` section for Level 2+. If `AGENTS.md` does not exist, do not assume it exists.
- Output: One real runnable verification command and its result, or a clear statement that no real runnable verification command was found.
- Verification: If a real command exists, only read-only or low-risk local commands were run automatically and recorded. High-risk commands waited for human confirmation before running. Failed commands were not treated as success. If no command exists, that fact is clearly recorded.
- AI Prompt:

```text
Inspect the current project files, available scripts, and toolchain to find a real runnable verification command.

If one exists, output:
- Command
- Why it is the minimal necessary verification command
- Actual run result
- Recommended write location and content: Level 1 writes to `README.md` or `STATE.md`; Level 2+ writes to the appropriate place in `README.md` / `STATE.md` / `AGENTS.md`

After finding a real command, automatically run only read-only or low-risk local minimal verification and report the result.
If the command may deploy, migrate data, write files, install dependencies, contact external services, change authentication or permissions, or touch production or sensitive data, stop and request human confirmation before running it.
If the command fails, do not claim verification succeeded. Report the failure and the next step that may need human judgment.
Only after human confirmation should you write the command and result into files. Level 1 writes to `README.md` or `STATE.md`; Level 2+ writes to the appropriate place in `README.md` / `STATE.md` / `AGENTS.md`. If `AGENTS.md` does not exist, do not assume it exists or create it unconditionally. Before confirmation, explicitly say "waiting for human confirmation before editing files."

If no real runnable verification command exists, clearly state "no real runnable verification command found."
Do not invent commands, scripts, tests, or tooling.
```

### A7. Update State and Run a Smoke Test

- Input: Confirmed workspace files; verification command result.
- Action: First read the created workspace files, restate the project state, propose a read-only or low-risk smoke test, and wait for human confirmation. After confirmation, run and report only the smoke test result; do not update `STATE.md`. After a second confirmation, update `STATE.md` with the current phase, next step, blockers, latest verification result, and smoke test result.
- Output: First stage: current state, next step, blockers, and smoke test proposal. Second stage: actual smoke test result and a wait for another confirmation. Third stage: handoff-ready `STATE.md`.
- Verification: The AI assistant can restate project state and next step without asking for basic context again; the smoke test was reported separately before `STATE.md` was updated; `STATE.md` was updated only after the second human confirmation.
- AI Prompt:

```text
Read the created workspace files and restate the current project state from those files.
Level 1 usually includes README/STATE. Level 2+ usually also includes SPEC/AGENTS.

Output:
- Current phase
- Next step
- Blockers
- Latest verification result
- One read-only or low-risk smoke test task proposal

Do not modify files in this first stage. Explicitly say "waiting for human confirmation before running the smoke test."
Unless the human explicitly confirms otherwise, the smoke test must be read-only or low-risk.

After the first human confirmation, run the read-only or low-risk smoke test and report only the result. Do not update STATE.md yet.
After reporting, explicitly say "waiting for another confirmation before updating STATE.md."

After the second human confirmation, update STATE.md with:
- Current phase
- Next step
- Blockers
- Latest verification result
- Smoke test result
```

## Path B: Retrofit An Existing Project

### B1. Scan the Existing Repo

- Input: An existing repo.
- Action: Check README, scripts, tests, build configuration, docs, and current git status.
- Output: Current project summary; existing infra list; risks or uncertainties.
- Verification: The summary cites real files or commands and does not fill in project structure from guesses.
- AI Prompt:

```text
Scan the current repo and list the existing README, scripts, tests, build configuration, docs, and git status.

Requirements:
- Do not modify files.
- Do not invent commands, directories, or project structure.
- Summarize only from real files and actual command output.
- If a category is missing or cannot be confirmed, write "missing" or "needs confirmation."

Output:
- Current project summary
- Existing infra list
- Risks or uncertainties
```

### B2. Judge the Project Level

- Input: Repo scan result; known collaboration, production, permission, and data risks.
- Action: Read `en/guides/project-levels.md`; judge Level 1-4.
- Output: Project level, reasoning, and questions that need human confirmation.
- Verification: If production, permissions, authentication, payments, sensitive data, security, or compliance risk exists, stop and require human confirmation.
- AI Prompt:

```text
Read `en/guides/project-levels.md` and judge whether this project is Level 1-4 based on the repo scan.

Input:
- Repo scan result: <paste B1 output>
- Known collaboration, production, permission, and data risks: <paste known information>

Requirements:
- Explain the reasoning.
- If the project may involve production, permissions, authentication, payments, sensitive data, security, or compliance, stop and ask for human confirmation.
- If the project may be Level 4, stop and ask for human confirmation.
- Do not lower the risk level on your own.
- Do not assume missing information means low risk.

Output:
- Project level
- Reasoning
- Questions that need human confirmation
```

### B3. Inventory Existing Infra and Gaps

- Input: Project level; repo scan result.
- Action: Compare against `en/guides/infra-by-level.md` and `en/guides/infra-checklists.md`; list existing and missing items.
- Output: Existing infra, missing infra, and minimal completion plan.
- Verification: Level 1 includes at least README, STATE, and a verification command. Level 2+ includes at least SPEC, AGENTS, `docs/decisions/`, and tests or an explicit verification script. If Level 3 or 4 is selected, the minimum infra in the guide is covered.
- AI Prompt:

```text
Read `en/guides/infra-by-level.md` and `en/guides/infra-checklists.md`, then inventory the AI workflow infra for the current project level and repo scan result.

Input:
- Project level: <Level 1/2/3/4>
- Repo scan result: <paste B1 output>

Requirements:
- List existing infra.
- List missing infra.
- Propose only the minimal completion plan for the current level.
- Do not suggest unrelated files, unrelated process, or optional items beyond the current level.
- Level 1 needs at least README, STATE, and one verification command.
- Level 2+ needs at least SPEC, AGENTS, `docs/decisions/`, and tests or an explicit verification script.
- If Level 3/4 is selected, cover the guide's minimum infra for that level.
- If an item cannot be confirmed from repo evidence, mark it as "needs confirmation."

Output:
- Existing infra
- Missing infra
- Minimal completion plan
```

### B4. Complete Workspace Files From Existing Evidence

- Input: Existing repo docs and code; missing infra list.
- Action: Based on existing evidence, generate or update the workspace files in the B3 minimal completion plan. Level 1 usually only needs `README.md`, `STATE.md`, and verification command records; do not create `SPEC.md` or `AGENTS.md` unconditionally. Write uncertain information as `needs confirmation`.
- Output: Completed workspace files and/or items marked as `needs confirmation`.
- Verification: New content can be traced to existing files, real commands, or human confirmation. Before modifying any existing file, the agent listed exact files and evidence sources and received explicit human confirmation. The agent did not invent commands, tests, deployment flow, or team rules.
- AI Prompt:

```text
Complete the missing workspace files using only real repo files, real commands, and existing human-confirmed information.

Input:
- Existing repo docs and code summary: <paste evidence>
- Missing infra list: <paste B3 output>

Requirements:
- Generate or update workspace files according to the project level and B3 minimal completion plan. Do not create or update `SPEC.md` or `AGENTS.md` unconditionally.
- For Level 1, do not assume `SPEC.md` or `AGENTS.md` exists. Unless the human explicitly requests it or B3 evidence supports it, complete only the Level 1 requirements: `README.md`, `STATE.md`, and verification command records.
- All new content must be traceable to existing files, actual commands, or human confirmation.
- Write all uncertain information as "needs confirmation."
- Do not invent commands, tests, deployment flow, team rules, permission models, or done criteria.
- Before editing, list the exact files you will create or update and the evidence source for each file.
- If you will modify any existing file, especially `README.md`, `STATE.md`, `SPEC.md`, `AGENTS.md`, or other existing infra files, wait for explicit human confirmation before editing. Do not modify files while waiting.

Output:
- Exact file list to create or update
- Evidence source for each file
- Explicit statement: "waiting for human confirmation before modifying existing files"
- Completed workspace files
- Items still needing human confirmation
- Evidence source for each key piece of information
```

### B5. Verify the Verification Command

- Input: Verification commands recorded in README, STATE, and AGENTS if it exists.
- Action: Automatically run only read-only or low-risk local verification commands. If a recorded command may deploy, migrate data, write files, install dependencies, contact external services, change authentication or permissions, or touch production or sensitive data, stop and request human confirmation first. Record the command and result.
- Output: Verification command result; if it failed, key error and judgment.
- Verification: Automatically run commands are read-only or low-risk local commands. High-risk commands waited for human confirmation before running. A failed command was not treated as success.
- AI Prompt:

```text
Check the verification commands recorded in README, STATE, and AGENTS if AGENTS exists, then run the minimal necessary verification.

Requirements:
- Level 1 may not have AGENTS. Do not assume the file exists.
- Automatically run only read-only or low-risk local verification commands.
- If a recorded command may deploy, migrate data, write files, install dependencies, contact external services, change authentication or permissions, or touch production / sensitive data, stop and ask for human confirmation. Do not run it before confirmation.
- If multiple commands exist, prefer the smallest read-only or low-risk local verification command.
- Actually run at least one available verification command. If the only available command requires human confirmation, state the risk and wait for confirmation.
- Report the command, result, and key errors.
- If no verification command is recorded, clearly state "no verification command found."
- If the command fails, say it failed. Do not claim success.
- Do not invent scripts, tests, or tooling.

Output:
- Files checked
- Command actually run
- Command result
- Key error and judgment if it failed
```

### B6. Human Review and State Update

- Input: Completed workspace files; verification command result.
- Action: A human reviews the goal, boundaries, forbidden actions, done criteria, and verification result. After confirmation, update `STATE.md` with current status, next step, and blockers.
- Output: Confirmed workspace files; handoff-ready `STATE.md`.
- Verification: `STATE.md` lets the next agent take over. Level 1 forbidden actions and done criteria can live in `README.md` or `STATE.md`; Level 2+ should include forbidden actions and done criteria in `AGENTS.md`.
- AI Prompt:

```text
Based on README, SPEC, STATE, AGENTS, and the verification result, list the key fields that need human confirmation.

Requirements:
- Focus on goal, boundaries, forbidden actions, done criteria, and verification result.
- Level 1 may not have SPEC or AGENTS. Do not assume those files exist.
- Level 1 forbidden actions and done criteria can live in README or STATE. If AGENTS does not exist, check whether README/STATE covers them.
- Level 2+ should include forbidden actions and done criteria in AGENTS. If AGENTS does not exist or lacks these items, mark them as missing.
- If AGENTS exists, confirm that it includes forbidden actions and done criteria.
- Do not modify files before confirmation.
- Do not invent team rules, acceptance criteria, or verification conclusions.

Output:
- Fields needing human confirmation
- Current content or missing status for each field
- Suggested confirmation questions
- Explicit statement: "waiting for human confirmation before updating STATE.md"
```

### B7. Let the Agent Run a Low-Risk Handoff Test

- Input: Confirmed workspace files.
- Action: The agent reads the files, restates the project state, and proposes a low-risk handoff task. Wait for confirmation before execution. If the task is executed, report the result before any documentation update.
- Output: Agent understanding summary; low-risk task proposal or execution result.
- Verification: The agent can state the project level, current state, next step, and verification method without asking for basic context again.
- AI Prompt:

```text
Read the created or updated workspace files and restate your understanding of the project.

Requirements:
- Level 1 may not have SPEC or AGENTS. Do not assume those files exist.
- State the project level, current state, next step, blockers, and verification method.
- Propose one read-only or low-risk handoff task.
- Do not perform high-risk operations.
- Do not edit files before confirmation.
- If the human confirms execution of the low-risk task, execute it and report the result first. Wait for further confirmation before updating any docs.
- Do not ask again for basic context that is already written in the workspace files.

Output:
- Project understanding summary
- Project level
- Current state
- Next step
- Blockers
- Verification method
- Low-risk handoff task proposal
- Explicit statement: "waiting for human confirmation before execution"
```

## Key Claude Code / Codex CLI Prompts

These prompts are intended for isolated use at key checkpoints. By default, the tool should read files first, report a plan, and wait for confirmation. Do not let the tool assume Level 1 always has `SPEC.md` or `AGENTS.md`.

### Repo Scan

Claude Code prompt:

```text
First read the current repo's README, scripts, tests, build configuration, and docs.
Do not modify files.
Do not invent commands, directories, or project structure.
Output the current project state, available verification commands, and missing AI workflow infra.
If a category is missing or cannot be confirmed, clearly write "missing" or "needs confirmation."
```

Codex CLI prompt:

```text
In the current repo, inspect README.md, STATE.md, optional SPEC.md / AGENTS.md, and available scripts.
Judge the current project level, list missing infra, and propose the minimal completion plan.
For Level 1, do not assume SPEC.md or AGENTS.md exists.
Do not modify files.
Do not invent commands. If no verification command can be found, say so clearly.
```

### Template Generation

Claude Code prompt:

```text
Using the templates under en/templates/project/ if available, draft the files required for this project level. If English templates are missing, adapt from zh/templates/project/ into English.
Level 1 usually needs only README.md, STATE.md, and verification command records. Level 2+ needs SPEC.md and AGENTS.md.
First list the files you will create or update, evidence sources, assumptions, and questions that need human confirmation.
Before modifying any existing file, wait for human confirmation.
Do not invent commands, tests, deployment flow, or team rules.
```

Codex CLI prompt:

```text
Read the existing files and scripts in the current repo.
Using the templates under en/templates/project/ if available, complete the minimal workspace files. If English templates are missing, adapt from zh/templates/project/ into English.
Use only information that really exists in the repo.
Do not create SPEC.md or AGENTS.md unconditionally for Level 1. Complete those files only for Level 2+.
Before modifying any existing file, state the plan, affected files, and evidence sources, then wait for confirmation.
Write "needs confirmation" for uncertain content.
```

### Final Smoke Test

Claude Code prompt:

```text
Read README.md, STATE.md, and SPEC.md / AGENTS.md if they exist.
Restate the project level, current state, next step, blockers, and verification method.
Then propose one read-only or low-risk handoff task.
Do not modify files. Wait for confirmation first.
```

Codex CLI prompt:

```text
Based on the current workspace files, restate the project state and propose one small task that can be executed safely.
Level 1 may have only README.md and STATE.md. Do not assume SPEC.md or AGENTS.md exists.
The handoff task must be read-only or low-risk.
Do not modify files. Do not execute the task before confirmation.
Explicitly say "waiting for confirmation before execution."
Do not perform high-risk operations.
```

## Final Checklist

- [ ] Project level has been judged.
- [ ] `README.md` has been created or updated.
- [ ] `STATE.md` has been created or updated.
- [ ] Level 2+ projects have `SPEC.md`.
- [ ] Level 2+ projects have `AGENTS.md`.
- [ ] Level 3/4 projects cover the minimum collaboration, risk, and handoff infra from the matching guide.
- [ ] The verification command is real and runnable, or the absence of a real verification command has been explicitly recorded.
- [ ] `STATE.md` records the next step and blockers.
- [ ] Level 1 forbidden actions and done criteria are recorded in `README.md` or `STATE.md`.
- [ ] Level 2+ `AGENTS.md` records forbidden actions and done criteria.
- [ ] A human has reviewed AI-generated commands and rules.
- [ ] An agent can restate the project state and take over one read-only or low-risk small task.

## Related Docs

- [Project-Level Judgment](../guides/project-levels.md)
- [Infra Recommendations By Project Level](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
