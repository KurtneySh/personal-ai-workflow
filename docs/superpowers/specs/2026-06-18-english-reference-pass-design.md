# English Reference Pass Design

## Context

Chinese v1 is the source of truth for this handbook. English v1 draft content already covers the public entrypoint, core concepts, scenario guidance, loops, skill workflow, and project workspace templates.

The English `reference` section is still a placeholder, while the Chinese reference article `zh/reference/ai-development-workflow-best-practices.md` is stabilized. This pass translates that article into English and updates the English handbook status so reference material is no longer treated as a later pass.

## Goal

Translate the stabilized Chinese reference article into an English v1 draft while preserving its structure, argument, and role as the handbook's philosophy / appendix material.

The English reference should help readers understand the broader working philosophy behind the handbook: agentic local tools, AI-readable project workspaces, feedback loops, skills, loops, multi-agent boundaries, human judgment, and risk controls.

## Approach

Use a strict reference translation approach:

- Preserve the Chinese source file's frontmatter, title intent, section order, and conceptual structure.
- Translate into clear English with light phrasing improvements where needed.
- Keep Chinese v1 as the source of truth.
- Avoid adding English-only sections, glossary entries, new concepts, or rewritten structure.

This keeps the English reference useful without turning it into a competing source document.

## Scope

Create or update:

- `en/reference/README.md`
- `en/reference/ai-development-workflow-best-practices.md`
- `README.md`
- `docs/repo/translation-status.md`

## Out of Scope

- No changes to Chinese source files.
- No expansion or translation of `cases`.
- No glossary split.
- No new reference subpages beyond the translated article.
- No scripts or automation.
- No structural rewrite of the reference article for English readers.

## Content Requirements

The English reference article must preserve these ideas from the Chinese source:

- Do not treat AI as a chat box, and do not jump straight to full automation.
- First make the project AI-readable, executable, and verifiable.
- Move development entrypoints from web chat to local agentic tools that can read repositories, edit files, run commands, and inspect errors.
- Build an AI-readable workspace with project rules, goals, state, docs, skills, tests, scripts, and logs.
- Establish feedback loops before automation.
- Treat skills as reusable compounding assets.
- Treat loops as opt-in, not default, and require hard stop conditions.
- Design boundaries before using multiple agents.
- Move the human role upward into system design, review, direction, and risk judgment.
- Manage understanding debt and safety debt.
- Preserve the recommended v1 workflow.

## Status Updates

After this pass:

- `en/reference/` should be marked as English v1 draft.
- `README.md` should present reference material as available English v1 draft content.
- Translation status should indicate that only cases remain as English placeholder content.
- `zh/cases/` should remain explicitly described as intentionally placeholder/template only.

## Acceptance Criteria

- `en/reference/README.md` is no longer a placeholder and links to the translated article.
- `en/reference/ai-development-workflow-best-practices.md` exists and mirrors the Chinese source structure.
- English reference content contains no Chinese prose.
- README and translation status no longer describe reference material as a later English pass.
- Cases remain described as future/manual placeholder content.
- No broken relative Markdown links are introduced.
- `git diff --check` passes.

## Risks

- Translation may accidentally become a rewrite. Mitigation: preserve the source section structure and concept order.
- Status wording may overclaim that cases are complete. Mitigation: keep cases as the only remaining placeholder.
- Reference links may point to files that do not exist. Mitigation: verify relative Markdown links after implementation.
