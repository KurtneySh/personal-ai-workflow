# Chinese V1 Stabilization Audit Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Audit Chinese v1 documentation for internal consistency, navigability, and translation readiness, then record the findings in `docs/repo/zh-v1-stabilization-audit.md`.

**Architecture:** This is primarily an audit/reporting task. The audit gathers evidence from repository files and command output, applies only small mechanical fixes when clearly safe, and writes a structured report; larger content changes become follow-up items.

**Tech Stack:** Markdown documentation, shell commands, `rg`, `find`, `git`, and a one-off local Markdown link check command.

---

## File Structure

- Create: `docs/repo/zh-v1-stabilization-audit.md`
  - Responsibility: final audit report with commands run, findings, fixes, follow-ups, translation readiness, and working-record recommendations.
- Optional modify: `README.zh.md` and `zh/**/*.md`
  - Only for small evidence-backed mechanical fixes: broken local links, stale planned wording, content-map mismatches, accidental non-template placeholders, or obvious terminology inconsistencies.
- Do not modify: `README.md`, English placeholder files, `docs/superpowers/specs/`, `docs/superpowers/plans/`, or existing `docs/repo/` working records except for adding the audit report.

## Acceptance Checklist

- `docs/repo/zh-v1-stabilization-audit.md` exists.
- The report summarizes Chinese v1 readiness.
- The report lists commands run.
- Findings are grouped by severity.
- Broken local Markdown links are either fixed or listed with rationale.
- Stale planned/current wording is either fixed or listed with rationale.
- Terminology and boundary issues are either fixed if mechanical or listed as follow-up.
- Placeholder findings distinguish intentional template blanks from accidental unresolved markers.
- Working records are not deleted.
- The report recommends what to do with `docs/superpowers/specs/`, `docs/superpowers/plans/`, and `docs/repo/` before public release.
- No English translation is performed.
- `git diff --check` passes.

---

### Task 1: Run Read-Only Audit

**Files:**
- Read: `README.zh.md`
- Read: `zh/**/*.md`
- Read: `docs/superpowers/specs/*.md`
- Read: `docs/superpowers/plans/*.md`
- Read: `docs/repo/*`
- Create temporary local output only if useful; do not commit temporary files.

- [ ] **Step 1: Capture repository status and file inventory**

Run:

```bash
git status --short --branch
find zh -name '*.md' -type f | sort
find docs/superpowers/specs -name '*.md' -type f | sort
find docs/superpowers/plans -name '*.md' -type f | sort
find docs/repo -name '*.md' -type f | sort
```

Expected:

```text
Repo status is clean except for expected branch-ahead state. Chinese docs, specs, plans, and repo records are listed.
```

- [ ] **Step 2: Check content map against actual modules**

Run:

```bash
sed -n '1,160p' README.zh.md
find zh -maxdepth 2 -type d | sort
find zh -maxdepth 3 -type f | sort
```

Expected:

```text
README.zh.md content map can be compared against existing zh modules and files.
```

Record:

- modules listed in `README.zh.md`,
- modules present under `zh/`,
- missing or stale descriptions,
- modules that exist but are not discoverable.

- [ ] **Step 3: Run placeholder and draft marker scan**

Run:

```bash
rg -n "TODO|TBD|待定|占位|FIXME|lorem" README.zh.md zh docs/superpowers docs/repo || true
```

Expected:

```text
All matches are available for classification as intentional template blanks, audit/search terms, or accidental unresolved markers.
```

Record:

- intentional template matches,
- expected audit/spec command matches,
- accidental unresolved markers that require a fix or follow-up.

- [ ] **Step 4: Run terminology scan**

Run:

```bash
rg -n "agent|AI workflow|infra|Level [1-4]|项目级别|handoff|verification|验证信号|stop|escalate|loop|skill|project-level skill|system-level skill|cookbook|playbook|case" README.zh.md zh
```

Expected:

```text
Terminology usage is visible across Chinese docs.
```

Record:

- terms that are consistent enough for v1,
- terms that drift in a confusing way,
- terms that should remain English technical terms for now,
- follow-up items for glossary or translation.

- [ ] **Step 5: Run local Markdown link check**

Run this one-off Python command from the repository root:

```bash
python3 - <<'PY'
from pathlib import Path
import re
import sys
from urllib.parse import unquote

files = [Path('README.zh.md')]
files += sorted(Path('zh').rglob('*.md'))
files += sorted(Path('docs/superpowers/specs').glob('*.md'))
files += sorted(Path('docs/superpowers/plans').glob('*.md'))

link_re = re.compile(r'(?<!!)\[[^\]]+\]\(([^)]+)\)')
broken = []
skipped = []

for file in files:
    text = file.read_text(encoding='utf-8')
    for line_no, line in enumerate(text.splitlines(), 1):
        for match in link_re.finditer(line):
            raw = match.group(1).strip()
            target = raw.split('#', 1)[0].strip()
            if not target:
                continue
            if re.match(r'^[a-zA-Z][a-zA-Z0-9+.-]*:', target):
                skipped.append((str(file), line_no, raw, 'external-or-uri'))
                continue
            if target.startswith('<') and target.endswith('>'):
                target = target[1:-1]
            target = unquote(target)
            path = (file.parent / target).resolve()
            if not path.exists():
                broken.append((str(file), line_no, raw))

if broken:
    print('BROKEN LINKS:')
    for file, line_no, raw in broken:
        print(f'{file}:{line_no}: {raw}')
else:
    print('BROKEN LINKS: none')

if skipped:
    print('SKIPPED NON-LOCAL LINKS:')
    for file, line_no, raw, reason in skipped:
        print(f'{file}:{line_no}: {raw} ({reason})')

sys.exit(1 if broken else 0)
PY
```

Expected:

```text
Either "BROKEN LINKS: none" or a list of broken local Markdown links to fix or record.
```

Record:

- broken local links,
- skipped external or URI links,
- any false positives.

- [ ] **Step 6: Inspect boundaries and stale wording**

Run:

```bash
rg -n "planned|Planned|后续|未来|占位|自动|自动创建|自动升级|push|merge|deploy|migrate|权限|认证|支付|敏感数据|安全|合规" README.zh.md zh
```

Expected:

```text
Status wording, automation boundaries, and risky action boundaries are visible for review.
```

Record:

- stale planned/future wording,
- contradictions around automation, loops, skills, remote actions, or risky work,
- items that are fine as intentional future-status wording.

- [ ] **Step 7: Commit nothing for Task 1**

Run:

```bash
git status --short --branch
```

Expected:

```text
No files changed by the read-only audit task.
```

---

### Task 2: Apply Small Mechanical Fixes If Needed

**Files:**
- Optional modify: `README.zh.md`
- Optional modify: `zh/**/*.md`
- Do not modify: English files, specs, plans, or existing working records.

- [ ] **Step 1: Decide whether fixes are allowed**

For each issue found in Task 1, apply a fix only if all are true:

```text
- evidence-backed
- local and mechanical
- intended wording or target is clear from surrounding docs
- does not change workflow policy
- does not add a new concept
```

Expected:

```text
Each candidate issue is categorized as "fix now" or "report as follow-up".
```

- [ ] **Step 2: Apply allowed fixes with focused edits**

Use `apply_patch` for manual file edits.

Allowed examples:

```text
- fix a broken local Markdown link target,
- remove stale "planned" wording for modules already implemented,
- correct a content-map link,
- align obvious naming such as PR CI Workflow vs PR / CI Workflow when context is clear,
- remove accidental placeholder markers outside templates.
```

Expected:

```text
Only small mechanical fixes are applied. Larger changes are left for the audit report.
```

- [ ] **Step 3: Verify fixes**

Run:

```bash
git diff --check
git status --short --branch
```

If link fixes were applied, rerun the one-off link check from Task 1 Step 5.

Expected:

```text
Whitespace check passes. Link check has no broken local links, or remaining broken links are documented with rationale.
```

- [ ] **Step 4: Commit mechanical fixes if any**

If files were changed, run:

```bash
git add README.zh.md zh
git commit -m "docs: stabilize zh v1 documentation"
```

Expected:

```text
Commit succeeds if there were mechanical fixes. If there were no fixes, skip this commit and record "no mechanical fixes applied" in the report.
```

---

### Task 3: Write Audit Report

**Files:**
- Create: `docs/repo/zh-v1-stabilization-audit.md`

- [ ] **Step 1: Create the audit report**

Write `docs/repo/zh-v1-stabilization-audit.md` with this structure:

```markdown
# Chinese V1 Stabilization Audit

Date: 2026-06-16

## Summary

- Chinese v1 readiness:
- Translation readiness:
- Blocking issues:
- Important issues:
- Mechanical fixes applied:

## Scope

Audited:

- `README.zh.md`
- `zh/**/*.md`
- `docs/superpowers/specs/*.md`
- `docs/superpowers/plans/*.md`
- `docs/repo/*.md`

Out of scope:

- English translation.
- Major rewrites.
- New workflow modules.
- Real case studies.
- Deleting working records.

## Commands Run

```bash
<list exact commands run during the audit>
```

## Findings

### Critical

- None, or list each finding with file and line evidence.

### Important

- None, or list each finding with file and line evidence.

### Minor

- None, or list each finding with file and line evidence.

### Follow-Up

- None, or list each follow-up with rationale.

## Fixes Applied

- No mechanical fixes applied.

or:

- `<file>`: `<what changed and why>`

## Link Integrity

- Broken local links:
- Skipped non-local links:
- Remaining link risks:

## Placeholder And Draft Marker Review

- Intentional template blanks:
- Expected audit/spec/plans matches:
- Accidental unresolved markers:

## Terminology And Boundary Review

- Stable terms:
- Terms to revisit before English translation:
- Boundary issues:

## Translation Readiness

- Ready for English structure pass:
- Needs follow-up before translation:
- Suggested translation order:

## Working Records Recommendation

- `docs/superpowers/specs/`:
- `docs/superpowers/plans/`:
- `docs/repo/`:
- Public release recommendation:

## Follow-Up Items

- [ ] Item:
```

Replace every placeholder line with the actual audit result. Do not leave angle-bracket placeholders.

- [ ] **Step 2: Verify report sections**

Run:

```bash
rg -n "^## (Summary|Scope|Commands Run|Findings|Fixes Applied|Link Integrity|Placeholder And Draft Marker Review|Terminology And Boundary Review|Translation Readiness|Working Records Recommendation|Follow-Up Items)" docs/repo/zh-v1-stabilization-audit.md
rg -n "<list exact commands|<file>|<what changed|Item:" docs/repo/zh-v1-stabilization-audit.md || true
```

Expected:

```text
All required report sections are listed. The placeholder scan has no unresolved angle-bracket placeholders. "Item:" only appears if it is part of a real follow-up item with content.
```

- [ ] **Step 3: Verify report references and whitespace**

Run:

```bash
git diff --check
test -f docs/repo/zh-v1-stabilization-audit.md
```

Expected:

```text
git diff --check has no output. Audit report exists.
```

- [ ] **Step 4: Commit audit report**

Run:

```bash
git add docs/repo/zh-v1-stabilization-audit.md
git commit -m "docs: add zh v1 stabilization audit"
```

Expected:

```text
Commit succeeds with message "docs: add zh v1 stabilization audit".
```

---

### Final Verification

- [ ] **Step 1: Verify final status and whitespace**

Run:

```bash
git diff --check HEAD~2..HEAD
git status --short --branch
```

Expected:

```text
Whitespace check has no output. There are no uncommitted changes.
```

- [ ] **Step 2: Verify audit report acceptance**

Run:

```bash
test -f docs/repo/zh-v1-stabilization-audit.md
rg -n "Chinese v1 readiness|Translation readiness|Working Records Recommendation|docs/superpowers/specs|docs/superpowers/plans|docs/repo" docs/repo/zh-v1-stabilization-audit.md
```

Expected:

```text
Audit report exists and includes readiness plus working-record recommendations.
```

- [ ] **Step 3: Verify no English translation happened**

Run:

```bash
git diff --name-status HEAD~2..HEAD
```

Expected:

```text
Changed files are limited to the audit report and any small Chinese doc fixes. No English translation files are changed.
```
