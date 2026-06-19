# English Cases Template Pass Design

## Context

Chinese v1 is the source of truth for this handbook. English v1 draft content now covers the public entrypoint, core concepts, scenario guidance, loops, skill workflow, project workspace templates, and reference material.

The only remaining English placeholder is `en/cases/`. The Chinese cases section is intentionally template-only: it contains an entrypoint and `CASE.template.md`, but no real case studies yet. This pass should make the English cases section structurally complete without inventing sample cases.

## Goal

Turn `en/cases/` from an untranslated placeholder into an English v1 template-only section.

The section should explain how to record real AI development workflow cases and provide a copyable case template, while making clear that real cases will be added manually later.

## Approach

Use a strict template-only translation approach:

- Translate `zh/cases/README.md` into `en/cases/README.md`.
- Translate `zh/cases/CASE.template.md` into `en/cases/CASE.template.md`.
- Preserve the Chinese source structure, boundaries, and safety warnings.
- Keep Chinese v1 as the source of truth.
- Do not create real, sample, or fictional case files.

## Scope

Create or update:

- `en/cases/README.md`
- `en/cases/CASE.template.md`
- `README.md`
- `docs/repo/translation-status.md`

## Out of Scope

- No real case studies.
- No fictional or sample case studies.
- No new case taxonomy.
- No expansion of Chinese source files.
- No scripts or automation.
- No changes to other English sections unless required for status/link updates.

## Content Requirements

The English cases section must preserve these ideas from the Chinese source:

- Cases are retrospectives of real projects or real tasks.
- A case records project background, project level, starting state, infra used, agent actions, human decisions, verification signals, risks, failures, reusable patterns, and possible skill candidates.
- A case is not a tutorial, cookbook rewrite, prompt collection, or polished success story.
- Cases must not include secrets, credentials, private data, customer data, production details, or internal credentials.
- Sensitive details should be sanitized or represented as abstracted judgment.
- Cases may remain incomplete while work is in progress, but verified and unknown information must be clearly distinguished.
- Cases may propose skill candidates, but cannot treat candidates as adopted skills.
- Skill candidate judgment should link back to the Skill Workflow guidance.

## Status Updates

After this pass:

- `en/cases/` should be marked as English v1 placeholder/template only.
- `README.md` should present cases as a template-only section rather than a future English pass.
- Translation status should indicate there are no remaining untranslated English section placeholders, while real cases are still intentionally absent.
- `zh/cases/` should remain explicitly described as intentionally placeholder/template only.

## Acceptance Criteria

- `en/cases/README.md` is no longer a later-pass placeholder and links to `CASE.template.md`.
- `en/cases/CASE.template.md` exists.
- `en/cases/` contains no Chinese prose.
- No real, sample, or fictional case files are added.
- README and translation status no longer describe cases as a future English pass.
- Status wording clearly says cases are template-only and real cases will be added manually later.
- No broken relative Markdown links are introduced.
- `git diff --check` passes.

## Risks

- Translation may imply real cases already exist. Mitigation: use explicit template-only wording.
- A sample case may be tempting but would conflict with the user's stated plan. Mitigation: only translate README and template.
- Safety warnings may be weakened during translation. Mitigation: preserve the sensitive-information warning in both entrypoint and template.
