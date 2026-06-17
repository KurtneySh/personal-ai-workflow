# English Core Concepts Pass Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create the first useful English reader path by translating the handbook entrypoint and core concept modules.

**Architecture:** Keep Chinese v1 as the source of truth and create idiomatic English reader-facing docs for the default entrypoint, guides, loops, and skill workflow. Leave scenario-heavy modules as explicit placeholders for later passes, and update repo working records to reflect partial English v1 progress.

**Tech Stack:** Markdown documentation, local shell checks, `rg`, `find`, `git diff --check`, and a local Markdown link checker.

---

## File Structure

- Modify `README.md`: English-first public entrypoint, 3-minute reading path, repository map, language status.
- Modify `en/guides/README.md`: guide module entrypoint.
- Create `en/guides/project-levels.md`: English translation/adaptation of `zh/guides/project-levels.md`.
- Create `en/guides/infra-by-level.md`: English translation/adaptation of `zh/guides/infra-by-level.md`.
- Create `en/guides/infra-checklists.md`: English translation/adaptation of `zh/guides/infra-checklists.md`.
- Modify `en/loops/README.md`: English translation/adaptation of `zh/loops/README.md`.
- Modify `en/skill-workflow/README.md`: English translation/adaptation of `zh/skill-workflow/README.md`.
- Modify `docs/repo/translation-status.md`: mark English core concept modules as v1 draft and leave non-core modules as placeholders.

## Shared Translation Rules

- Preserve these terms exactly: `agent`, `infra`, `loop`, `skill`, `handoff`, `verification signal`, `human gate`, `stop / escalate`.
- Preserve human responsibility boundaries for push, merge, deployment, migration, permission changes, sensitive data handling, loop execution, and system-level skill promotion.
- Prefer natural English over sentence-by-sentence literal translation.
- Keep structure close enough to Chinese v1 that future translation reviews can map English sections back to Chinese source sections.
- Do not edit `zh/` unless a link or status line is objectively inconsistent with this pass.

### Task 1: English Entrypoint

**Files:**
- Modify: `README.md`

- [ ] **Step 1: Read source entrypoints**

Run:

```bash
sed -n '1,140p' README.md
sed -n '1,120p' README.zh.md
sed -n '1,120p' docs/repo/translation-status.md
```

Expected: `README.md` still contains stale language status saying English will mirror Chinese after Chinese stabilizes; `README.zh.md` contains the current 3-minute path.

- [ ] **Step 2: Rewrite `README.md`**

Replace the file with an English-first entrypoint containing:

- title `# AI Workflow Handbook`,
- short purpose statement,
- `## 3-Minute Reading Path`,
- links to translated core docs under `en/`,
- `## Repository Map`,
- `## Current Language Status`,
- link to `README.zh.md`.

Use these required links:

```markdown
[Project Levels](en/guides/project-levels.md)
[Infra By Level](en/guides/infra-by-level.md)
[Infra Checklist](en/guides/infra-checklists.md)
[Loops](en/loops/README.md)
[Skill Workflow](en/skill-workflow/README.md)
[Chinese source entrypoint](README.zh.md)
```

- [ ] **Step 3: Check entrypoint links**

Run:

```bash
rg -n "after the Chinese version stabilizes|after Chinese stabilizes|Translation is planned" README.md || true
rg -n "en/guides/project-levels.md|en/guides/infra-by-level.md|en/guides/infra-checklists.md|en/loops/README.md|en/skill-workflow/README.md|README.zh.md" README.md
```

Expected: first command prints nothing; second command prints all required link targets.

- [ ] **Step 4: Commit Task 1**

Run:

```bash
git add README.md
git commit -m "docs: update english handbook entrypoint"
```

Expected: commit succeeds.

### Task 2: English Guides

**Files:**
- Modify: `en/guides/README.md`
- Create: `en/guides/project-levels.md`
- Create: `en/guides/infra-by-level.md`
- Create: `en/guides/infra-checklists.md`

- [ ] **Step 1: Read Chinese guide sources**

Run:

```bash
sed -n '1,180p' zh/guides/project-levels.md
sed -n '1,180p' zh/guides/infra-by-level.md
sed -n '1,260p' zh/guides/infra-checklists.md
```

Expected: source documents define Level 1-4, recommended infra by level, and optional checklist categories.

- [ ] **Step 2: Write `en/guides/README.md`**

Replace placeholder content with a guide entrypoint that links to:

```markdown
- [Project Levels](project-levels.md)
- [Infra By Level](infra-by-level.md)
- [Infra Checklist](infra-checklists.md)
```

It must state that guides help choose the smallest useful AI workflow infra for a project.

- [ ] **Step 3: Write `en/guides/project-levels.md`**

Create an idiomatic English version covering:

- Level 1: personal experiment or small script,
- Level 2: personal project expected to continue,
- Level 3: collaborative project,
- Level 4: production, business-critical, or high-risk project,
- how to use the level as a default, not a permanent label,
- what changes when a project moves up a level.

- [ ] **Step 4: Write `en/guides/infra-by-level.md`**

Create an idiomatic English version covering:

- minimum useful infra for each level,
- why overbuilding infra is a cost,
- how README, status docs, specs, plans, tests, CI, review, and issue tracking appear at different levels,
- where agent handoff and verification expectations increase.

- [ ] **Step 5: Write `en/guides/infra-checklists.md`**

Create an idiomatic English checklist version covering:

- context infra,
- instruction infra,
- verification infra,
- collaboration infra,
- risk infra,
- loop infra,
- skill infra,
- how to use checklist items as optional additions rather than a required maturity model.

- [ ] **Step 6: Check guide links and stale placeholders**

Run:

```bash
rg -n "Translation is planned|after the Chinese version stabilizes|after Chinese stabilizes" en/guides || true
rg -n "\\[[^]]+\\]\\([^)]+\\)" en/guides
```

Expected: first command prints nothing; second command prints the guide links.

- [ ] **Step 7: Commit Task 2**

Run:

```bash
git add en/guides/README.md en/guides/project-levels.md en/guides/infra-by-level.md en/guides/infra-checklists.md
git commit -m "docs: translate english guide concepts"
```

Expected: commit succeeds.

### Task 3: English Loops

**Files:**
- Modify: `en/loops/README.md`

- [ ] **Step 1: Read Chinese loop source**

Run:

```bash
sed -n '1,340p' zh/loops/README.md
```

Expected: source covers loop suitability, loop design, stop conditions, escalation, and human-owned decisions.

- [ ] **Step 2: Rewrite `en/loops/README.md`**

Replace placeholder content with an English version covering:

- what a loop means in this handbook,
- when a task is loop-ready,
- when a task is not loop-ready,
- required loop design fields,
- verification signals,
- stop / escalate conditions,
- human gates for risky actions,
- how loop output can reveal future skill opportunities.

- [ ] **Step 3: Check loop boundary language**

Run:

```bash
rg -n "human gate|stop / escalate|push|merge|deploy|migration|permission|sensitive|system-level skill" en/loops/README.md
rg -n "Translation is planned|after the Chinese version stabilizes|after Chinese stabilizes" en/loops/README.md || true
```

Expected: first command shows explicit safety boundary language; second command prints nothing.

- [ ] **Step 4: Commit Task 3**

Run:

```bash
git add en/loops/README.md
git commit -m "docs: translate english loop guidance"
```

Expected: commit succeeds.

### Task 4: English Skill Workflow

**Files:**
- Modify: `en/skill-workflow/README.md`

- [ ] **Step 1: Read Chinese skill workflow source**

Run:

```bash
sed -n '1,380p' zh/skill-workflow/README.md
```

Expected: source covers system-level skills, project-level skills, capture criteria, loop-aware skill discovery, review, and maintenance.

- [ ] **Step 2: Rewrite `en/skill-workflow/README.md`**

Replace placeholder content with an English version covering:

- what a skill is in this handbook,
- system-level skills vs project-level skills,
- what should not become a skill,
- signals that a workflow is worth capturing,
- how loop workflows let agents propose skill candidates,
- how non-loop workflows rely more on human coordination,
- promotion and maintenance rules,
- human gates for system-level skill installation.

- [ ] **Step 3: Check skill workflow boundary language**

Run:

```bash
rg -n "system-level skill|project-level skill|human gate|loop|agent|promotion|maintenance" en/skill-workflow/README.md
rg -n "Translation is planned|after the Chinese version stabilizes|after Chinese stabilizes" en/skill-workflow/README.md || true
```

Expected: first command shows the main concepts; second command prints nothing.

- [ ] **Step 4: Commit Task 4**

Run:

```bash
git add en/skill-workflow/README.md
git commit -m "docs: translate english skill workflow"
```

Expected: commit succeeds.

### Task 5: Translation Status and Final Verification

**Files:**
- Modify: `docs/repo/translation-status.md`

- [ ] **Step 1: Update translation status**

Edit `docs/repo/translation-status.md` so that:

- `guides` English status is `v1 draft`,
- `loops` English status is `v1 draft`,
- `skill-workflow` English status is `v1 draft`,
- `playbooks`, `cookbook`, `cases`, `templates`, and `reference` remain `Placeholder`,
- notes mention that English core concepts were drafted on 2026-06-16.

- [ ] **Step 2: Run local Markdown link checker**

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

- [ ] **Step 3: Run placeholder and stale-status scans**

Run:

```bash
rg -n "TODO|TBD|待定|FIXME|lorem" README.md README.zh.md en zh docs/repo || true
rg -n "after the Chinese version stabilizes|after Chinese stabilizes|Translation is planned after the Chinese version stabilizes" README.md en docs/repo || true
```

Expected: first command only prints intentional search-command references if any; second command prints no translated core concept files. It may still print placeholder-only modules outside this pass if their wording is intentionally left unchanged, and those should be listed in the final report.

- [ ] **Step 4: Run whitespace verification**

Run:

```bash
git diff --check
```

Expected: no output and exit code 0.

- [ ] **Step 5: Review scope boundaries**

Run:

```bash
git diff --name-only HEAD~4..HEAD
```

Expected: changed files are limited to `README.md`, files under `en/guides/`, `en/loops/README.md`, `en/skill-workflow/README.md`, and `docs/repo/translation-status.md`.

- [ ] **Step 6: Commit Task 5**

Run:

```bash
git add docs/repo/translation-status.md
git commit -m "docs: update english translation status"
```

Expected: commit succeeds.

## Final Review Checklist

- [ ] `README.md` reads as the English default entrypoint.
- [ ] `en/guides/README.md` links to all three translated guide files.
- [ ] `en/loops/README.md` includes loop readiness, design, stop, and human gate guidance.
- [ ] `en/skill-workflow/README.md` distinguishes system-level and project-level skills.
- [ ] No translated core document says translation is still planned after Chinese stabilizes.
- [ ] Markdown link checker reports `BROKEN LINKS: none`.
- [ ] `git diff --check` passes.
- [ ] `docs/repo/translation-status.md` accurately marks English core concepts as `v1 draft`.
