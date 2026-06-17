# English Scenario Layer Pass Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Translate the English playbook and cookbook scenario layer so English readers can apply the AI workflow handbook in real development work.

**Architecture:** Keep Chinese v1 as source of truth and translate scenario-heavy English docs in focused batches. The pass updates the public entrypoint and translation records only after the scenario files exist, so intermediate commits do not overstate completion.

**Tech Stack:** Markdown documentation, local shell checks, `rg`, `find`, `git diff --check`, and a local Markdown link checker.

---

## File Structure

- Modify `en/playbooks/README.md`: English playbook entrypoint.
- Create `en/playbooks/start-a-new-project.md`: English adaptation of `zh/playbooks/start-a-new-project.md`.
- Modify `en/cookbook/README.md`: English cookbook entrypoint.
- Create `en/cookbook/repo-orientation.md`: English adaptation of `zh/cookbook/repo-orientation.md`.
- Create `en/cookbook/task-handoff.md`: English adaptation of `zh/cookbook/task-handoff.md`.
- Create `en/cookbook/review-and-fix.md`: English adaptation of `zh/cookbook/review-and-fix.md`.
- Create `en/cookbook/context-refresh.md`: English adaptation of `zh/cookbook/context-refresh.md`.
- Create `en/cookbook/pr-ci-workflow.md`: English adaptation of `zh/cookbook/pr-ci-workflow.md`.
- Create `en/cookbook/multi-agent-delegation.md`: English adaptation of `zh/cookbook/multi-agent-delegation.md`.
- Modify `README.md`: mark playbooks and cookbook as English v1 draft scenario guidance.
- Modify `docs/repo/translation-status.md`: mark playbooks and cookbook as English v1 draft.

## Shared Translation Rules

- Preserve these terms: `agent`, `infra`, `loop`, `skill`, `handoff`, `verification signal`, `human gate`, `stop / escalate`, `ownership`.
- Translate naturally for English readers. Do not produce a literal sentence-by-sentence copy if it makes the result hard to use.
- Preserve Chinese v1 safety boundaries around risky commands, remote actions, push, merge, deployment, migrations, permission changes, sensitive data, CI ownership, review decisions, and multi-agent integration.
- Prompt examples should be directly usable, but they must not authorize an agent to bypass human confirmation.
- Do not edit `zh/` unless an objective link or status inconsistency is discovered.

### Task 1: English New Project Playbook

**Files:**
- Modify: `en/playbooks/README.md`
- Create: `en/playbooks/start-a-new-project.md`

- [ ] **Step 1: Read source files**

Run:

```bash
sed -n '1,620p' zh/playbooks/start-a-new-project.md
sed -n '1,80p' en/playbooks/README.md
```

Expected: Chinese source covers new project startup, level judgment, infra, staged collaboration, human decisions, and verification; English README is still a placeholder.

- [ ] **Step 2: Write `en/playbooks/README.md`**

Replace placeholder content with an entrypoint that:

- describes playbooks as end-to-end workflows,
- links to `[Start A New Project](start-a-new-project.md)`,
- points readers back to guides, loops, and skill workflow where useful.

- [ ] **Step 3: Write `en/playbooks/start-a-new-project.md`**

Create an idiomatic English version of `zh/playbooks/start-a-new-project.md` that preserves:

- project-level judgment before infra choices,
- minimal useful infra by level,
- staged collaboration with Claude Code or Codex CLI,
- human decisions around scope, risk, dependencies, release, and maintenance,
- explicit verification before treating the project as ready,
- prompt examples and checklists from the Chinese source.

- [ ] **Step 4: Verify playbook links and stale wording**

Run:

```bash
rg -n "Translation is planned|later English pass|after the Chinese version stabilizes|after Chinese stabilizes" en/playbooks || true
rg -n "\[[^]]+\]\([^)]+\)" en/playbooks
git diff --check
```

Expected: first command prints nothing; second command prints the internal playbook links; `git diff --check` prints nothing.

- [ ] **Step 5: Commit Task 1**

Run:

```bash
git add en/playbooks/README.md en/playbooks/start-a-new-project.md
git commit -m "docs: translate english new project playbook"
```

Expected: commit succeeds.

### Task 2: English Cookbook Entry And Basic Scenarios

**Files:**
- Modify: `en/cookbook/README.md`
- Create: `en/cookbook/repo-orientation.md`
- Create: `en/cookbook/task-handoff.md`
- Create: `en/cookbook/review-and-fix.md`

- [ ] **Step 1: Read source files**

Run:

```bash
sed -n '1,120p' zh/cookbook/README.md
sed -n '1,220p' zh/cookbook/repo-orientation.md
sed -n '1,260p' zh/cookbook/task-handoff.md
sed -n '1,280p' zh/cookbook/review-and-fix.md
```

Expected: sources cover cookbook positioning, repo orientation, task handoff, and review/fix scenarios.

- [ ] **Step 2: Write `en/cookbook/README.md`**

Replace placeholder content with an entrypoint that:

- positions cookbook as scenario-driven Claude Code and Codex CLI usage,
- links to all six scenario pages:
  - `[Repo Orientation](repo-orientation.md)`
  - `[Task Handoff](task-handoff.md)`
  - `[Review And Fix](review-and-fix.md)`
  - `[Context Refresh](context-refresh.md)`
  - `[PR CI Workflow](pr-ci-workflow.md)`
  - `[Multi-Agent Delegation](multi-agent-delegation.md)`
- links to related docs under `../guides/`, `../loops/`, and `../skill-workflow/`.

- [ ] **Step 3: Write `en/cookbook/repo-orientation.md`**

Create an English scenario page covering:

- when to use repo orientation,
- what context the agent must read before editing,
- prompt examples,
- what the human must still decide,
- verification or stop conditions,
- when this scenario may become a skill.

- [ ] **Step 4: Write `en/cookbook/task-handoff.md`**

Create an English scenario page covering:

- when to hand off a bounded task,
- required task inputs,
- scope and forbidden actions,
- prompt examples,
- verification signals,
- handoff back to the human,
- loop or skill capture signals.

- [ ] **Step 5: Write `en/cookbook/review-and-fix.md`**

Create an English scenario page covering:

- test, lint, review, CI, or failure feedback,
- what the agent should inspect before changing files,
- how to separate diagnosis, fix, and verification,
- prompt examples,
- stop / escalate conditions,
- human gates for unclear review decisions or risky fixes.

- [ ] **Step 6: Verify basic cookbook links and stale wording**

Run:

```bash
rg -n "Translation is planned|later English pass|after the Chinese version stabilizes|after Chinese stabilizes" en/cookbook || true
rg -n "\[[^]]+\]\([^)]+\)" en/cookbook
git diff --check
```

Expected: first command prints nothing for files created in this task; links include all six scenario pages even if Task 3 creates the last three files later; `git diff --check` prints nothing.

- [ ] **Step 7: Commit Task 2**

Run:

```bash
git add en/cookbook/README.md en/cookbook/repo-orientation.md en/cookbook/task-handoff.md en/cookbook/review-and-fix.md
git commit -m "docs: translate english cookbook basics"
```

Expected: commit succeeds.

### Task 3: English Cookbook Advanced Collaboration Scenarios

**Files:**
- Create: `en/cookbook/context-refresh.md`
- Create: `en/cookbook/pr-ci-workflow.md`
- Create: `en/cookbook/multi-agent-delegation.md`

- [ ] **Step 1: Read source files**

Run:

```bash
sed -n '1,320p' zh/cookbook/context-refresh.md
sed -n '1,400p' zh/cookbook/pr-ci-workflow.md
sed -n '1,420p' zh/cookbook/multi-agent-delegation.md
```

Expected: sources cover context refresh, PR/CI workflow, and multi-agent delegation with ownership and risk boundaries.

- [ ] **Step 2: Write `en/cookbook/context-refresh.md`**

Create an English scenario page covering:

- when context refresh is needed,
- what state to recover,
- what files and signals to read,
- prompt examples,
- how to avoid stale assumptions,
- when to stop / escalate.

- [ ] **Step 3: Write `en/cookbook/pr-ci-workflow.md`**

Create an English scenario page covering:

- PR preparation,
- CI reading,
- review handling,
- final merge readiness,
- risky command and remote action boundaries,
- human gates for push, merge, deploy, migrations, permission changes, and unclear review decisions.

- [ ] **Step 4: Write `en/cookbook/multi-agent-delegation.md`**

Create an English scenario page covering:

- when multi-agent delegation is appropriate,
- task decomposition,
- ownership boundaries,
- what subagents should and should not do,
- review and integration,
- final human judgment,
- when repeated delegation patterns may become a skill.

- [ ] **Step 5: Verify advanced cookbook safety language**

Run:

```bash
rg -n "ownership|human gate|stop / escalate|push|merge|deploy|migration|permission|sensitive|CI|review" en/cookbook/context-refresh.md en/cookbook/pr-ci-workflow.md en/cookbook/multi-agent-delegation.md
rg -n "Translation is planned|later English pass|after the Chinese version stabilizes|after Chinese stabilizes" en/cookbook || true
git diff --check
```

Expected: first command shows explicit safety and ownership language; second command prints nothing in `en/cookbook`; `git diff --check` prints nothing.

- [ ] **Step 6: Commit Task 3**

Run:

```bash
git add en/cookbook/context-refresh.md en/cookbook/pr-ci-workflow.md en/cookbook/multi-agent-delegation.md
git commit -m "docs: translate english cookbook collaboration"
```

Expected: commit succeeds.

### Task 4: Entrypoint And Translation Status

**Files:**
- Modify: `README.md`
- Modify: `docs/repo/translation-status.md`

- [ ] **Step 1: Update `README.md`**

Update the repository map and reading path so:

- `en/playbooks/` is described as English v1 draft end-to-end workflow guidance,
- `en/cookbook/` is described as English v1 draft scenario guidance for Claude Code and Codex CLI,
- `en/templates/`, `en/reference/`, and `en/cases/` remain later English passes.

- [ ] **Step 2: Update `docs/repo/translation-status.md`**

Edit the status table so:

- `playbooks` English status is `v1 draft`,
- `cookbook` English status is `v1 draft`,
- `cases`, `templates`, and `reference` remain `Placeholder`.

Add a note that English scenario layer draft completed on 2026-06-17 for `en/playbooks/` and `en/cookbook/`.

- [ ] **Step 3: Verify status wording**

Run:

```bash
rg -n "planned English section|Translation is planned in a later English pass|after the Chinese version stabilizes|after Chinese stabilizes" README.md en/playbooks en/cookbook docs/repo/translation-status.md || true
git diff --check
```

Expected: first command prints nothing; `git diff --check` prints nothing.

- [ ] **Step 4: Commit Task 4**

Run:

```bash
git add README.md docs/repo/translation-status.md
git commit -m "docs: update english scenario translation status"
```

Expected: commit succeeds.

### Task 5: Final Verification

**Files:**
- No planned content changes. Fix only objective issues found by verification.

- [ ] **Step 1: Run local Markdown link checker**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
import re, sys
from urllib.parse import unquote

files = [Path('README.md'), Path('README.zh.md')]
files += sorted(Path('en').rglob('*.md'))
files += sorted(Path('zh').rglob('*.md'))
files += sorted(Path('docs/repo').rglob('*.md'))
files += sorted(Path('docs/superpowers/specs').glob('*.md'))

link_re = re.compile(r'(?<!!)\[[^\]]+\]\(([^)]+)\)')
broken = []

for file in files:
    text = file.read_text(encoding='utf-8')
    for line_no, line in enumerate(text.splitlines(), 1):
        for match in link_re.finditer(line):
            raw = match.group(1).strip()
            target = raw.split('#', 1)[0].strip()
            if not target:
                continue
            if re.match(r'^[a-zA-Z][a-zA-Z0-9+.-]*:', target):
                continue
            if target.startswith('<') and target.endswith('>'):
                target = target[1:-1]
            path = (file.parent / unquote(target)).resolve()
            if not path.exists():
                broken.append((str(file), line_no, raw))

if broken:
    print('BROKEN LINKS:')
    for file, line_no, raw in broken:
        print(f'{file}:{line_no}: {raw}')
else:
    print('BROKEN LINKS: none')

sys.exit(1 if broken else 0)
PY
```

Expected: `BROKEN LINKS: none`.

- [ ] **Step 2: Run stale wording and placeholder scans**

Run:

```bash
rg -n "TODO|TBD|待定|FIXME|lorem" README.md README.zh.md en zh docs/repo || true
rg -n "planned English section|Translation is planned in a later English pass|after the Chinese version stabilizes|after Chinese stabilizes" README.md en/playbooks en/cookbook docs/repo/translation-status.md || true
rg -n "human gate|stop / escalate|ownership|verification signal|push|merge|deploy|migration|permission|sensitive|CI|review" en/playbooks en/cookbook
```

Expected:

- first command only prints intentional historical command references if any,
- second command prints nothing in `README.md`, `en/playbooks`, `en/cookbook`, and translation status,
- third command shows safety and ownership language across scenario docs.

- [ ] **Step 3: Run whitespace verification**

Run:

```bash
git diff --check
```

Expected: no output and exit code 0.

- [ ] **Step 4: Check changed file scope**

Run:

```bash
git diff --name-only HEAD~4..HEAD
```

Expected: changed files are limited to:

- `README.md`
- `docs/repo/translation-status.md`
- files under `en/playbooks/`
- files under `en/cookbook/`

- [ ] **Step 5: Commit final fixes only if needed**

If verification found objective issues and fixes were required, commit with:

```bash
git add README.md docs/repo/translation-status.md en/playbooks en/cookbook
git commit -m "docs: fix english scenario layer verification"
```

Expected: commit succeeds only if fixes were needed. If no fixes were needed, do not create an empty commit.

## Final Review Checklist

- [ ] `en/playbooks/README.md` links to `start-a-new-project.md`.
- [ ] `en/playbooks/start-a-new-project.md` preserves project-level judgment, minimal infra, staged agent collaboration, human decisions, and verification.
- [ ] `en/cookbook/README.md` links to all six scenario pages.
- [ ] Each cookbook scenario includes when to use it, inputs, agent actions, human decisions, prompt examples where applicable, verification or stop conditions, and loop/skill considerations.
- [ ] PR/CI and multi-agent scenarios preserve ownership, risky command, push, merge, CI, review, and integration boundaries.
- [ ] `README.md` and `docs/repo/translation-status.md` accurately mark playbooks and cookbook as English `v1 draft`.
- [ ] `en/templates/`, `en/reference/`, and `en/cases/` remain later English passes.
- [ ] Local Markdown link checker reports `BROKEN LINKS: none`.
- [ ] `git diff --check` passes.
