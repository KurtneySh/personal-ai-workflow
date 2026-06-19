# English Cases Template Pass Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn `en/cases/` into an English v1 template-only section without adding real, sample, or fictional case studies.

**Architecture:** Keep `en/cases/README.md` as the cases section entrypoint and add one copyable `CASE.template.md` translated from the Chinese source. Status updates are limited to `README.md` and `docs/repo/translation-status.md`, where cases are marked as template-only rather than a future English pass.

**Tech Stack:** Markdown documentation only; verification uses shell commands, `rg`, `find`, `git diff --check`, and a local Markdown relative-link checker.

---

## File Structure

- Modify `en/cases/README.md`: replace the later-pass placeholder with a translated template-only cases entrypoint.
- Create `en/cases/CASE.template.md`: English translation of the Chinese case template.
- Modify `README.md`: describe `en/cases/` as English v1 placeholder/template content rather than a future pass.
- Modify `docs/repo/translation-status.md`: mark cases as English v1 placeholder/template only and note that real cases remain manual future content.

## Task 1: Cases Entrypoint

**Files:**
- Modify: `en/cases/README.md`

- [ ] **Step 1: Read current placeholder and Chinese cases entrypoint**

Run:

```bash
sed -n '1,220p' en/cases/README.md
sed -n '1,260p' zh/cases/README.md
```

Expected: `en/cases/README.md` is a later-pass placeholder; the Chinese source explains case purpose, boundaries, suggested workflow, template usage, skill capture boundary, and related docs.

- [ ] **Step 2: Replace `en/cases/README.md`**

Write this exact content:

```markdown
# Cases

Cases record retrospectives of real projects or real tasks that used an AI development workflow.

They focus on what happened, why judgments were made, which infra was used, what agents did, what humans decided, which verification signals existed, and which workflows may be worth turning into skill candidates.

## Purpose

This directory is for capturing practical cases.

A case is not a success story or a generic lesson. A good case should help your future self or a reader understand the project background, how the AI workflow was used, what worked, what failed, and which judgments should not be automated.

A case may remain incomplete while work is in progress, but it must clearly mark which information has been verified and which information is still unknown.

## What A Case Should Capture

Recommended fields:

- Project background.
- Project level and the reasoning behind it.
- Initial goal and starting state.
- Infra actually used.
- Playbooks, cookbook scenarios, loops, or skill workflow used.
- What agents did.
- What humans decided.
- Key decisions and tradeoffs.
- Verification signals such as commands, commits, PRs, CI, reviews, or human checklists.
- Risks, failures, blockers, and how they were handled.
- Which patterns can be reused.
- Which parts should not be reused.
- Which workflows may be worth capturing as skill candidates.

## What A Case Is Not

A case is not:

- A tutorial.
- A rewritten cookbook entry.
- A prompt collection.
- A summary that records only the result and not the judgment process.
- A way to package one-off experience as a general rule.
- A success story that hides failures, human intervention, or verification gaps.
- A place to publish secrets, credentials, private data, customer data, production details, or internal credentials.

If a case contains sensitive information, sanitize it first or record only the abstracted judgment. Do not write identifiable private details into the case.

## Suggested Workflow

1. Decide the project level first.
2. Record the initial goal and starting state.
3. Record the infra, playbooks, cookbook scenarios, and loop design actually used.
4. Record agent actions and human decisions separately.
5. Record real verification signals; explicitly say when there was no verification.
6. Record risks, failures, stop / escalate moments, and human judgment.
7. Review which patterns can be reused and which should not be reused.
8. If a repeated workflow appears, propose only a skill candidate; do not automatically create or promote it into a skill.

## Case Template

Copy [CASE.template.md](CASE.template.md) as the starting point for a new case.

Name case files by real project or real task, for example:

```text
2026-06-13-my-project-bootstrap.md
2026-06-13-ci-feedback-loop.md
```

A case can start as an incomplete draft, but it should state its current status and missing information.

## Skill Capture Boundary

A case can propose skill candidates, but it cannot treat a candidate as an adopted skill.

When a repeated workflow appears in a case, answer these questions first:

- How many times has this pattern appeared?
- Which tasks, commits, PRs, reviews, or verification results provide evidence?
- Does it depend on the current project, or can it be reused across projects?
- Is it better suited as a project-level skill or a system-level skill candidate?
- What are the counterexamples or unsuitable scenarios?
- Who needs to review it?

For the detailed judgment process, see [Skill Workflow](../skill-workflow/README.md).

## Related Docs

- [Project Levels](../guides/project-levels.md)
- [Infra By Level](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [Start A New Project Playbook](../playbooks/start-a-new-project.md)
- [Cookbook](../cookbook/README.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
```

- [ ] **Step 3: Verify entrypoint links and no placeholder wording**

Run:

```bash
rg -n 'Translation is planned|planned in a later English pass|future English pass|matching section under `zh/`' en/cases/README.md
```

Expected: no output.

- [ ] **Step 4: Commit Task 1**

Run:

```bash
git add en/cases/README.md
git commit -m "docs: translate english cases entrypoint"
```

Expected: commit succeeds.

## Task 2: Case Template

**Files:**
- Create: `en/cases/CASE.template.md`

- [ ] **Step 1: Read Chinese case template**

Run:

```bash
sed -n '1,260p' zh/cases/CASE.template.md
```

Expected: the source template includes metadata, project background, project level, infra used, playbooks, cookbook scenarios, loop usage, skill workflow usage, agent actions, human decisions, verification signals, risks, skill candidates, reusable patterns, non-reusable parts, and follow-up actions.

- [ ] **Step 2: Create `en/cases/CASE.template.md`**

Write this exact content:

```markdown
# Case: <case title>

> This template is for recording real projects or real tasks. Do not write secrets, credentials, private data, customer data, production details, or internal credentials into a case.

## Metadata

- Date:
- Case owner:
- Current status:
- Related repo or project:
- Related task, issue, PR, or commit:

## Project Background

- Project background:
- Product or task context:
- Team or personal context:
- Constraints:

## Project Level

- Level:
- Why this level:
- Risk notes:
- Human confirmation gates:

## Initial Goal

- Initial goal:
- Success criteria:
- Non-goals:
- Deadline or timebox:

## Starting State

- Starting repo state:
- Existing docs or infra:
- Known blockers:
- Unknowns:

## Infra Used

- README / STATE / SPEC / AGENTS:
- Project templates:
- Verification commands:
- CI or review process:
- Worktree or branch strategy:
- Other infra:

## Playbooks Used

- Playbooks:
- Why these playbooks:
- What changed after using them:

## Cookbook Scenarios Used

- Cookbook scenarios:
- Why these scenarios:
- What worked:
- What did not work:

## Loop Usage

- Was a loop used:
- Loop type:
- Goal:
- Allowed actions:
- Forbidden actions:
- Verification signal:
- Stop conditions:
- Escalation conditions:
- Maximum rounds:
- Result:

## Skill Workflow Usage

- Was skill workflow considered:
- Repeated pattern observed:
- Project-level skill candidate:
- System-level skill candidate:
- Evidence:
- Human review needed:

## Agent Actions

Record what agents actually did.

- Agent or tool:
- Assigned task:
- Files or areas touched:
- Commands run:
- Output or result:
- Verification reported:
- Residual risks reported:

## Human Decisions

Record what humans decided.

- Decision:
- Context:
- Alternatives considered:
- Why this decision:
- Evidence used:
- Impact:

## Key Decisions

- Decision:
- Owner:
- Date:
- Evidence:
- Reversible:
- Follow-up:

## Verification Signals

- Command, CI, review, or checklist:
- Result:
- Evidence link or commit:
- What this verified:
- What this did not verify:

## Risks And Handling

- Risk:
- Severity:
- How it appeared:
- Handling:
- Stop or escalate decision:
- Remaining risk:

## What Worked

- Pattern:
- Evidence:
- Why it worked:
- Reuse conditions:

## What Failed Or Was Confusing

- Problem:
- Evidence:
- Cause or hypothesis:
- Human intervention:
- What to change next time:

## Skill Candidates

- Candidate name:
- Project-level or system-level candidate:
- Repeated pattern:
- Evidence:
- Required inputs:
- Verification:
- Not applicable when:
- Review owner:

## Reusable Patterns

- Pattern:
- Where it applies:
- Required context:
- Verification:
- Limits:

## Non-Reusable Parts

- Part:
- Why not reusable:
- Risk of overgeneralizing:
- What to do instead:

## Follow-Up Actions

- Action:
- Owner:
- Priority:
- Due date or review point:
- Status:
```

- [ ] **Step 3: Verify no accidental Chinese prose**

Run:

```bash
rg -n "[一-龥]" en/cases
```

Expected: no output.

- [ ] **Step 4: Verify no real or sample case files were added**

Run:

```bash
find en/cases -maxdepth 1 -type f | sort
```

Expected output:

```text
en/cases/CASE.template.md
en/cases/README.md
```

- [ ] **Step 5: Run formatting check**

Run:

```bash
git diff --check
```

Expected: no output.

- [ ] **Step 6: Commit Task 2**

Run:

```bash
git add en/cases/CASE.template.md
git commit -m "docs: translate english case template"
```

Expected: commit succeeds.

## Task 3: README And Translation Status

**Files:**
- Modify: `README.md`
- Modify: `docs/repo/translation-status.md`

- [ ] **Step 1: Update README repository map**

Change the `en/cases/` bullet to:

```markdown
- `en/cases/`: English v1 placeholder/template-only section for practical case studies; real cases will be added manually later.
```

- [ ] **Step 2: Update README language status**

Change the current language status paragraph to:

```markdown
Chinese v1 is the source of truth. English v1 draft content now covers the public entrypoint, core concepts, the new-project playbook, cookbook scenarios, loops, skill workflow, project workspace templates, reference material, and template-only case documentation. Real case studies will be added manually later.
```

- [ ] **Step 3: Update translation status overview**

Change this paragraph:

```markdown
English content under `en/` mirrors the Chinese structure. Core concepts, scenario guidance, project workspace templates, and reference material are drafted first; cases remain a placeholder.
```

To:

```markdown
English content under `en/` mirrors the Chinese structure. Core concepts, scenario guidance, project workspace templates, reference material, and template-only case documentation are drafted; real case studies will be added manually later.
```

- [ ] **Step 4: Update translation policy**

Change the policy bullet about pass order to:

```markdown
- Translate English content in passes while keeping Chinese v1 as the source of truth; cases are template-only until real cases are added manually.
```

- [ ] **Step 5: Update translation status table**

Change the cases row to:

```markdown
| cases | v1 placeholder/template only | v1 placeholder/template only |
```

- [ ] **Step 6: Add cases template pass note**

Add this note after the English reference note:

```markdown
- English cases template draft completed on 2026-06-18 for `en/cases/`.
```

- [ ] **Step 7: Update final cases note**

Change the final cases note to:

```markdown
- `zh/cases/` and `en/cases/` intentionally contain entrypoints and templates only; real cases will be added manually later.
```

- [ ] **Step 8: Run Markdown relative-link verification**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
import re

roots = [Path('README.md'), Path('en/cases'), Path('docs/repo/translation-status.md')]
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

- [ ] **Step 9: Run stale wording scan**

Run:

```bash
rg -n 'en/cases/`: future English pass|cases remain a later English pass|cases remain a placeholder|Translation is planned|planned in a later English pass|matching section under `zh/`' README.md en/cases docs/repo/translation-status.md
```

Expected: no output.

- [ ] **Step 10: Run no-Chinese scan**

Run:

```bash
rg -n "[一-龥]" en/cases
```

Expected: no output.

- [ ] **Step 11: Confirm no real/sample case files were added**

Run:

```bash
find en/cases -maxdepth 1 -type f | sort
```

Expected output:

```text
en/cases/CASE.template.md
en/cases/README.md
```

- [ ] **Step 12: Run formatting check**

Run:

```bash
git diff --check
```

Expected: no output.

- [ ] **Step 13: Review final diff**

Run:

```bash
git diff -- README.md docs/repo/translation-status.md en/cases
```

Expected: diff only includes English cases entrypoint, case template, and status updates; no Chinese source files are changed and no real/sample case files are added.

- [ ] **Step 14: Commit Task 3**

Run:

```bash
git add README.md docs/repo/translation-status.md
git commit -m "docs: update english cases template status"
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
docs: update english cases template status
docs: translate english case template
docs: translate english cases entrypoint
docs: add english cases template pass plan
docs: add english cases template pass design
```

- [ ] **Step 2: Run final relative-link verification**

Run the relative-link checker from Task 3 Step 8.

Expected output:

```text
BROKEN LINKS: none
```

- [ ] **Step 3: Run final content scans**

Run:

```bash
rg -n "[一-龥]" en/cases
rg -n 'en/cases/`: future English pass|cases remain a later English pass|cases remain a placeholder|Translation is planned|planned in a later English pass|matching section under `zh/`' README.md en/cases docs/repo/translation-status.md
find en/cases -maxdepth 1 -type f | sort
git diff --check
```

Expected:

```text
en/cases/CASE.template.md
en/cases/README.md
```

The two `rg` commands and `git diff --check` produce no output.

- [ ] **Step 4: Confirm repository status**

Run:

```bash
git status --short --branch
```

Expected: clean working tree on the implementation branch, except allowed local-only `.superpowers/` content if present.
