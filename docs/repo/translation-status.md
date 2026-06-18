# Translation Status

Chinese content under `zh/` is the v1 source of truth.

English content under `en/` mirrors the Chinese structure. Core concepts, scenario guidance, and project workspace templates are drafted first; reference material and cases remain placeholders.

## Current Policy

- Treat stabilized Chinese v1 content as the source of truth.
- Keep English directories structurally aligned with Chinese directories.
- Translate English content in passes, starting with core concepts and scenario guidance before templates, reference material, and cases.
- Do not mix Chinese and English versions inside the same long-form content file.

## Current Status

| Section | Chinese | English |
| --- | --- | --- |
| guides | v1 stabilized | v1 draft |
| playbooks | v1 stabilized | v1 draft |
| cookbook | v1 stabilized | v1 draft |
| loops | v1 stabilized | v1 draft |
| skill-workflow | v1 stabilized | v1 draft |
| cases | v1 placeholder/template only | Placeholder |
| templates | v1 stabilized | v1 draft |
| reference | v1 stabilized | Placeholder |

## Notes

- Chinese v1 stabilization audit completed on 2026-06-16.
- English core concept draft completed on 2026-06-16 for `README.md`, `en/guides/`, `en/loops/`, and `en/skill-workflow/`.
- English scenario layer draft completed on 2026-06-17 for `en/playbooks/` and `en/cookbook/`.
- English templates draft completed on 2026-06-17 for `en/templates/` and `en/templates/project/`.
- Remaining English placeholders should be translated from the stabilized Chinese source while keeping the same information architecture.
- `zh/cases/` intentionally contains an entrypoint and template only; real cases will be added manually later.
