# English Reference Pass Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Translate the stabilized Chinese reference article into an English v1 draft and update handbook status so reference material is no longer a placeholder.

**Architecture:** Keep `en/reference/README.md` as the reference section entrypoint and add one translated article mirroring `zh/reference/ai-development-workflow-best-practices.md`. Keep status updates limited to `README.md` and `docs/repo/translation-status.md`; cases remain the only English placeholder area.

**Tech Stack:** Markdown documentation only; verification uses shell commands, `rg`, `find`, `git diff --check`, and a local Markdown relative-link checker.

---

## File Structure

- Modify `en/reference/README.md`: replace placeholder with English reference section entrypoint.
- Create `en/reference/ai-development-workflow-best-practices.md`: English translation of the Chinese reference article.
- Modify `README.md`: update reading path, repository map, and language status for reference availability.
- Modify `docs/repo/translation-status.md`: mark reference as English v1 draft and keep cases as the only remaining placeholder.

## Task 1: Reference Entrypoint

**Files:**
- Modify: `en/reference/README.md`

- [ ] **Step 1: Read current placeholder and Chinese source file list**

Run:

```bash
sed -n '1,120p' en/reference/README.md
find zh/reference en/reference -maxdepth 1 -type f | sort
```

Expected: `en/reference/README.md` is a placeholder and `zh/reference/ai-development-workflow-best-practices.md` is the source article.

- [ ] **Step 2: Replace `en/reference/README.md`**

Write this exact content:

```markdown
# Reference

This section contains appendix-style reference material for the handbook's AI development workflow philosophy.

Chinese v1 remains the source of truth. The English reference currently includes a v1 draft translation of the stabilized Chinese reference article.

## Available Reference

- [AI Development Workflow Best Practices](ai-development-workflow-best-practices.md): the working philosophy behind this handbook, including local agentic tools, AI-readable workspaces, feedback loops, skills, loops, multi-agent boundaries, human judgment, and risk controls.
```

- [ ] **Step 3: Verify entrypoint has no placeholder wording**

Run:

```bash
rg -n 'Translation is planned|planned in a later English pass|placeholder|matching section under `zh/`' en/reference/README.md
```

Expected: no output.

- [ ] **Step 4: Commit Task 1**

Run:

```bash
git add en/reference/README.md
git commit -m "docs: translate english reference entrypoint"
```

Expected: commit succeeds.

## Task 2: Reference Article Translation

**Files:**
- Create: `en/reference/ai-development-workflow-best-practices.md`

- [ ] **Step 1: Read the Chinese source article**

Run:

```bash
sed -n '1,220p' zh/reference/ai-development-workflow-best-practices.md
```

Expected: the source article has frontmatter, a main title, a current conclusion blockquote, sections 1-8, and a recommended v1 workflow.

- [ ] **Step 2: Create `en/reference/ai-development-workflow-best-practices.md`**

Write this exact content:

````markdown
---
title: AI Development Workflow Best Practices
created: 2026-06-12
updated: 2026-06-12
tags: [ai, workflow, agents, productivity]
---

# AI Development Workflow Best Practices

> Current conclusion: do not treat AI as a chat box, and do not start by chasing full automation. First make the project AI-readable, executable, and verifiable; then turn repeated and verifiable parts into Skills / Loops.

This document is the working philosophy entrypoint for this repository. It does not try to record every source. Instead, it distills a set of AI workflow principles that can guide real development work.

## 1. Main Entrypoint: Move From Chat Boxes To Local Agentic Tools

The main development entrypoint should move from web chat boxes to local agentic tools that can read repositories, edit files, run commands, and inspect errors, such as Claude Code, Codex CLI, and Cursor.

The problem with chat boxes is not that the model is not powerful enough. The problem is that humans must carry information back and forth between AI and the execution environment. A more efficient approach is to let the agent enter the engineering environment directly and complete the loop of "read code -> modify -> run -> read errors -> modify again -> verify."

Practical principles:

- Start the agent inside the repo by default instead of copying code snippets into a chat box.
- Let the agent access code, tests, logs, scripts, documentation, and error output.
- Humans define goals, constraints, acceptance criteria, and key judgments; they should not act as middleware for copying and pasting errors.

## 2. Project Shape: Build A Context Workbench For AI

Put project materials, code, designs, logs, tasks, and rules into an AI-readable workspace that can be diffed and versioned.

Recommended structure:

```text
project/
├── AGENTS.md           # agent operating contract and project rules
├── SPEC.md             # high-level goals, boundaries, product judgment
├── STATE.md            # current state, next steps, blockers, lessons
├── skills/             # reusable working methods
├── docs/               # designs, research, decision records
├── src/
├── tests/
├── scripts/            # deterministic tools agents can call
└── logs/               # feedback material agents can read
```

The core point is not the directory shape itself. The core point is to keep context stable in the filesystem. Files are more controllable than chat memory, and they are better suited for review, reuse, and version management.

## 3. Build The Feedback Loop Before Automation

Before automation, make sure the agent can verify its own output. Tests, lint, builds, logs, traces, CLIs, and browser debugging are more fundamental than complex prompts.

Practical principles:

- Prepare one-command verification for each project: `test`, `lint`, `typecheck`, and `build`.
- Task descriptions must include success criteria.
- Capabilities that can be turned into a CLI should become a CLI first; the GUI should be a human interface, not the agent's only entrypoint.
- Have the agent actively run verification commands before ending a task and report the results.

## 4. Skills Are The Real Compounding Asset

Do not re-prompt repeated tasks from scratch every time. Turn success criteria, real failure modes, and tool usage into Skills.

A good Skill should include at least:

- Applicable scenarios: when it should be used.
- Success criteria: what counts as completion.
- Working steps: the recommended execution order.
- Common pitfalls: failure modes from real tasks.
- Callable tools: commands, scripts, checks, and reference files.
- Stop conditions: when to ask a human for judgment.

Practical principles:

- After completing a high-value task, add a summary prompt: "Turn this workflow into a Skill so it can be reused next time."
- Do not write Skills as long SOPs. Make boundaries, acceptance criteria, and key tools clear.
- Maintain `skills/index.md` so agents can discover which Skill to use for a task.

## 5. Loops Are Not The Default Option

Do not turn everything into a background loop. A task is worth turning into a loop only when it is repeated, automatically verifiable, within budget, and supported by agent tools.

Good candidates for early loops:

- CI failure triage;
- dependency upgrade PRs;
- lint-and-fix;
- flaky test reproduction;
- issue-to-PR work under strong test coverage.

Poor candidates for early loops:

- architecture rewrites;
- authentication / payments;
- production deployments;
- vague product exploration;
- tasks where completion depends mainly on subjective judgment.

A loop must have hard stop conditions, such as maximum iterations, maximum time, maximum tokens, consecutive no-progress attempts, and risk points that require human confirmation.

## 6. Design Boundaries Before Using Multiple Agents

The key to multi-agent work is not the number of agents. It is context isolation, permission boundaries, ownership, and merge / reduce. Without a convergence mechanism, multiple agents only produce multiple opinions.

Practical principles:

- If one agent can do the work, do not split it first.
- When splitting work, write a delegation contract: Role / Goal / Context / Allowed actions / Ownership / Forbidden / Output format / Stop condition.
- Use worktrees or explicit file ownership when writing code in parallel.
- A reducer must be responsible for final integration, conflict judgment, and closing accountability.

## 7. Move The Human Role Upward

AI development workflows do not remove humans. They move humans from typists into system designers, reviewers, direction setters, and risk judges.

Good tasks to give agents:

- small-scope implementation;
- test additions;
- error diagnosis;
- documentation cleanup;
- repetitive code migration;
- toolable tasks with clear acceptance criteria.

Humans must retain judgment over:

- architecture direction;
- data models;
- permissions and security;
- dependency introduction;
- product experience;
- broad refactors;
- whether to continue, roll back, or stop.

## 8. Manage Understanding Debt And Safety Debt

The more capable AI becomes, the easier it is to accumulate understanding debt, safety risk, and the habit of "just one more prompt."

Practical principles:

- Read key diffs, not only agent summaries.
- Isolate large changes in branches or worktrees.
- Do not blindly install third-party Skills, MCPs, or plugins; review them first or rewrite clean versions.
- Give loops hard stops.
- Avoid giving an agent all three at once: private data, an untrusted input environment, and external communication.

## Recommended v1 Workflow

1. Build an AI-readable repo for every project: `AGENTS.md`, `SPEC.md`, `STATE.md`, `docs/`, `skills/`, `tests/`, and `scripts/`.
2. Use local agents as the main tools: Codex CLI, Claude Code, and Cursor, instead of web chat boxes.
3. Write success criteria before every task: what must be completed, how it will be verified, and when to stop.
4. Standardize verification commands: one-command test / lint / typecheck / build.
5. Turn repeated tasks into Skills: run them manually first, then write the Skill, then consider a loop.
6. Turn only tasks that pass the four-condition test into loops: repeated, verifiable, budgeted, and tooled.
7. Use multiple agents only for isolatable tasks: each subagent has a delegation contract, ownership, and stop condition.
8. Keep final judgment with humans: architecture, permissions, dependencies, product direction, and broad refactors should not be delegated automatically.
````

- [ ] **Step 3: Verify structure mirrors Chinese source**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
for path in [Path('zh/reference/ai-development-workflow-best-practices.md'), Path('en/reference/ai-development-workflow-best-practices.md')]:
    headings = [line.rstrip() for line in path.read_text().splitlines() if line.startswith('#')]
    print(path)
    for heading in headings:
        print(heading)
PY
```

Expected: both files have one H1, sections 1-8, and a final recommended v1 workflow section in the same order.

- [ ] **Step 4: Check for accidental Chinese prose**

Run:

```bash
rg -n "[一-龥]" en/reference
```

Expected: no output.

- [ ] **Step 5: Run formatting check**

Run:

```bash
git diff --check
```

Expected: no output.

- [ ] **Step 6: Commit Task 2**

Run:

```bash
git add en/reference/ai-development-workflow-best-practices.md
git commit -m "docs: translate english workflow reference"
```

Expected: commit succeeds.

## Task 3: README And Translation Status

**Files:**
- Modify: `README.md`
- Modify: `docs/repo/translation-status.md`

- [ ] **Step 1: Update README reading path**

Add this as item 8 in the `3-Minute Reading Path` list:

```markdown
8. Read the v1 draft [Reference](en/reference/README.md) when you want the appendix-style working philosophy behind the handbook.
```

- [ ] **Step 2: Update README repository map**

Change the `en/reference/` bullet to:

```markdown
- `en/reference/`: English v1 draft appendix material for AI development workflow philosophy and best practices.
```

- [ ] **Step 3: Update README language status**

Change the current language status paragraph to:

```markdown
Chinese v1 is the source of truth. English v1 draft content now covers the public entrypoint, core concepts, the new-project playbook, cookbook scenarios, loops, skill workflow, project workspace templates, and reference material, while cases remain a later English pass.
```

- [ ] **Step 4: Update translation status overview**

Change this paragraph:

```markdown
English content under `en/` mirrors the Chinese structure. Core concepts, scenario guidance, and project workspace templates are drafted first; reference material and cases remain placeholders.
```

To:

```markdown
English content under `en/` mirrors the Chinese structure. Core concepts, scenario guidance, project workspace templates, and reference material are drafted first; cases remain a placeholder.
```

- [ ] **Step 5: Update translation policy**

Change the policy bullet about pass order to:

```markdown
- Translate English content in passes, starting with core concepts, scenario guidance, project workspace templates, and reference material before cases.
```

- [ ] **Step 6: Update translation status table**

Change the reference row to:

```markdown
| reference | v1 stabilized | v1 draft |
```

Keep the cases row as:

```markdown
| cases | v1 placeholder/template only | Placeholder |
```

- [ ] **Step 7: Add reference pass note**

Add this note after the English templates note:

```markdown
- English reference draft completed on 2026-06-18 for `en/reference/`.
```

- [ ] **Step 8: Update remaining placeholders note**

Change the remaining placeholders note to:

```markdown
- Cases remain the only English placeholder section; `zh/cases/` intentionally contains an entrypoint and template only, and real cases will be added manually later.
```

Remove the duplicate old `zh/cases/` note if this creates repetition.

- [ ] **Step 9: Run Markdown relative-link verification**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
import re

roots = [Path('README.md'), Path('en/reference'), Path('docs/repo/translation-status.md')]
files = []
for root in roots:
    if root.is_file():
        files.append(root)
    else:
        files.extend(sorted(root.rglob('*.md')))

broken = []
pattern = re.compile(r'(?<!!)\[[^\]]+\]\(([^)]+)\)')
for path in files:
    text = path.read_text()
    for target in pattern.findall(text):
        if '://' in target or target.startswith('#') or target.startswith('mailto:'):
            continue
        clean = target.split('#', 1)[0]
        if not clean:
            continue
        resolved = (path.parent / clean).resolve()
        if not resolved.exists():
            broken.append((str(path), target))

if broken:
    print('BROKEN LINKS:')
    for path, target in broken:
        print(f'{path}: {target}')
    raise SystemExit(1)
print('BROKEN LINKS: none')
PY
```

Expected output:

```text
BROKEN LINKS: none
```

- [ ] **Step 10: Run stale wording scan**

Run:

```bash
rg -n 'reference material and cases remain placeholders|reference material and cases remain later|en/reference/`: future English pass|Translation is planned|planned in a later English pass|matching section under `zh/`' README.md en/reference docs/repo/translation-status.md
```

Expected: no output.

- [ ] **Step 11: Run formatting check**

Run:

```bash
git diff --check
```

Expected: no output.

- [ ] **Step 12: Review final diff**

Run:

```bash
git diff -- README.md docs/repo/translation-status.md en/reference
```

Expected: diff only includes English reference translation, reference entrypoint, and status updates; no Chinese source files are changed.

- [ ] **Step 13: Commit Task 3**

Run:

```bash
git add README.md docs/repo/translation-status.md
git commit -m "docs: update english reference translation status"
```

Expected: commit succeeds.

## Final Verification

- [ ] **Step 1: Confirm committed history for this pass**

Run:

```bash
git log --oneline -6
```

Expected: recent commits include:

```text
docs: update english reference translation status
docs: translate english workflow reference
docs: translate english reference entrypoint
docs: add english reference pass plan
docs: add english reference pass design
```

- [ ] **Step 2: Run final relative-link verification**

Run the relative-link checker from Task 3 Step 9.

Expected output:

```text
BROKEN LINKS: none
```

- [ ] **Step 3: Run final content scans**

Run:

```bash
rg -n "[一-龥]" en/reference
rg -n 'reference material and cases remain placeholders|reference material and cases remain later|en/reference/`: future English pass|Translation is planned|planned in a later English pass|matching section under `zh/`' README.md en/reference docs/repo/translation-status.md
git diff --check
```

Expected: both `rg` commands and `git diff --check` produce no output.

- [ ] **Step 4: Confirm repository status**

Run:

```bash
git status --short --branch
```

Expected: clean working tree on the implementation branch, except allowed local-only `.superpowers/` content if present.
