# 0001: Handbook Information Architecture

Date: 2026-06-13

## Decision

Use a decision-first handbook structure.

The default public entrypoint is `README.md` in English, while `README.zh.md` is the current complete entrypoint for Chinese readers.

Chinese content under `zh/` is the v1 source of truth. English content under `en/` mirrors the structure and starts as placeholders.

## Rationale

The handbook should help readers act quickly:

1. identify project level,
2. choose minimum useful infra,
3. add checklist items by capability,
4. use playbooks and cookbook entries for concrete work,
5. decide when to design loops or capture skills.

This avoids turning `README.md` into a large document and allows each topic to grow independently.

## Consequences

- README files stay short and navigational.
- Long-form content lives in section directories.
- `docs/repo/` can be removed, archived, or folded into `CONTRIBUTING.md` after v1 stabilizes.
