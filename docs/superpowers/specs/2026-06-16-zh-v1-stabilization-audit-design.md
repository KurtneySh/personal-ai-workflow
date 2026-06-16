# Chinese V1 Stabilization Audit Design

Date: 2026-06-16

## Goal

Run a Chinese v1 stabilization audit before starting English translation.

The audit should verify that the Chinese source-of-truth documentation is internally consistent, navigable, and ready to be treated as a stable translation base. It should identify broken links, stale planned wording, terminology drift, module-map mismatches, placeholder content, and obvious boundary contradictions.

## Scope

Audit the Chinese documentation and repository entrypoints:

```text
README.zh.md
zh/**/*.md
docs/superpowers/specs/*.md
docs/superpowers/plans/*.md
```

The primary implementation output should be a new audit report:

```text
docs/repo/zh-v1-stabilization-audit.md
```

The audit may also make small, evidence-backed fixes when they are clearly mechanical, such as:

- fixing broken relative links,
- updating stale "planned" wording,
- correcting content-map mismatches,
- aligning obvious terminology inconsistencies,
- removing accidental placeholders.

Any larger content rewrite, new module design, or translation work is out of scope for this audit and should be listed as follow-up.

## Non-Goals

This audit will not:

- Translate Chinese docs into English.
- Rewrite major sections for style.
- Add new workflow modules.
- Add real case studies.
- Delete `docs/superpowers/specs/` or `docs/superpowers/plans/`.
- Clean up `docs/repo/` working records beyond adding the audit report.
- Decide final public release packaging.

The audit should prepare the repo for translation and later cleanup decisions, not perform those later phases.

## Audit Areas

### 1. Content Map Consistency

Check whether `README.zh.md` accurately reflects existing modules:

- `zh/guides/`
- `zh/playbooks/`
- `zh/cookbook/`
- `zh/loops/`
- `zh/skill-workflow/`
- `zh/cases/`
- `zh/templates/`
- `zh/reference/`

The audit should identify missing links, stale descriptions, or modules that exist but are not discoverable.

### 2. Link Integrity

Check relative Markdown links in:

- `README.zh.md`
- all `zh/**/*.md`
- relevant `docs/superpowers/specs/*.md`
- relevant `docs/superpowers/plans/*.md`

The audit should distinguish:

- broken local links,
- links to intentionally future English placeholders,
- non-file links that should not be checked as local files.

### 3. Terminology Consistency

Check recurring terms and decide whether usage is consistent enough for Chinese v1:

- agent
- AI workflow
- infra
- project level / Level 1-4
- handoff
- verification / 验证信号
- stop / escalate
- loop
- skill
- project-level skill
- system-level skill candidate
- cookbook
- playbook
- case

The audit should not force full Chinese translation of every English technical term. It should focus on inconsistent or confusing usage.

### 4. Boundary Consistency

Check whether modules maintain clear boundaries:

- guides define project levels and infra decisions,
- templates provide reusable project workspace docs,
- playbooks describe end-to-end flows,
- cookbook describes concrete task scenarios,
- loops describe whether and how to automate repeated execution,
- skill workflow describes capture and promotion boundaries,
- cases record real project evidence,
- reference stores concepts and appendices.

The audit should flag contradictions such as:

- cookbook pages acting like command encyclopedias,
- cases acting like tutorials,
- loop pages encouraging automation without stop conditions,
- skill pages implying automatic promotion to system skills,
- PR / CI or multi-agent docs authorizing remote actions without human gates.

### 5. State And Roadmap Wording

Check for stale status language:

- "planned" items that are now implemented,
- "future" wording that no longer applies,
- "current status" statements that conflict with existing files,
- English placeholder wording that conflicts with Chinese source-of-truth policy.

### 6. Placeholder And Draft Markers

Search for obvious unresolved markers:

- `TODO`
- `TBD`
- `待定`
- `占位`
- `lorem`
- `FIXME`
- accidental plan leakage

The audit must distinguish intentional template blanks from accidental unresolved placeholders.

### 7. Working Records Decision

Review current working-record areas:

- `docs/superpowers/specs/`
- `docs/superpowers/plans/`
- `docs/repo/`

The audit should not delete them. It should record a recommendation:

- keep for now,
- cleanup before public release,
- move to archive later,
- or document that they are development records.

## Audit Report Requirements

Create:

```text
docs/repo/zh-v1-stabilization-audit.md
```

Required sections:

- Summary
- Scope
- Commands Run
- Findings
- Fixes Applied
- Follow-Up Items
- Translation Readiness
- Working Records Recommendation

Findings should be grouped by severity:

- Critical: blocks using Chinese docs as source of truth.
- Important: should fix before translation.
- Minor: can fix opportunistically.
- Follow-up: useful later, not required for Chinese v1 stability.

Each finding should cite file paths and line numbers where possible.

## Fix Policy

During implementation, small fixes are allowed only when they meet all of these conditions:

- the issue is evidence-backed,
- the fix is local and mechanical,
- the intended wording is clear from surrounding docs,
- the fix does not change workflow policy or add new concepts.

Examples allowed:

- link target typo,
- stale planned item,
- inconsistent module link text,
- accidental placeholder outside templates.

Examples not allowed:

- rewriting a whole guide,
- changing project-level definitions,
- changing loop policy,
- changing skill promotion policy,
- adding new cookbook scenarios,
- adding real cases.

Larger issues should be recorded in the audit report as follow-up items.

## Verification

The audit should run fresh checks such as:

```bash
git status --short --branch
find zh -name '*.md' -type f | sort
rg -n "TODO|TBD|待定|占位|FIXME|lorem" README.zh.md zh docs/superpowers docs/repo
git diff --check
```

It should also run a local Markdown link check. If no project tool exists, use a small one-off shell or scripting check during audit execution, without adding it as a repository script.

After any fixes:

```bash
git diff --check
git status --short --branch
```

The final audit report should list all commands run and any checks that could not be run.

## Acceptance Criteria

- `docs/repo/zh-v1-stabilization-audit.md` exists.
- The audit report summarizes Chinese v1 readiness.
- The audit report lists commands run.
- The audit report groups findings by severity.
- Broken local Markdown links are either fixed or listed with rationale.
- Stale planned/current wording is either fixed or listed with rationale.
- Terminology and boundary issues are either fixed if mechanical or listed as follow-up.
- Placeholder findings distinguish intentional template blanks from accidental unresolved markers.
- Working records are not deleted.
- The report recommends what to do with `docs/superpowers/specs/`, `docs/superpowers/plans/`, and `docs/repo/` before public release.
- No English translation is performed.
- Markdown whitespace checks pass.
