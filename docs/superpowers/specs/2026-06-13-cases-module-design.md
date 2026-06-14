# Cases Module Design

Date: 2026-06-13

## Goal

Add a lightweight Chinese `zh/cases/` module for recording real AI development workflow cases.

The module should give the repository a place to capture project background, infra choices, agent actions, human judgment, verification signals, risks, and possible skill candidates. It should not invent example cases or turn into a tutorial.

## Scope

Create:

```text
zh/cases/README.md
zh/cases/CASE.template.md
```

The README should explain the purpose and boundaries of cases. The template should provide a reusable structure for future real project writeups.

## Non-Goals

This version will not:

- Add any real or fictional case writeup.
- Add English translations.
- Add scripts or executable automation.
- Replace cookbook pages.
- Replace skill workflow.
- Require every project to use the same exact case format.
- Clean up `docs/repo/` working records.

Cases should remain evidence-driven records, not success stories.

## Module Positioning

`zh/cases/` should sit between practice and abstraction:

- `zh/playbooks/` explains end-to-end workflows.
- `zh/cookbook/` explains concrete reusable scenarios.
- `zh/loops/` explains whether repeated automatic execution is appropriate.
- `zh/skill-workflow/` explains when repeated workflows should become skill candidates.
- `zh/cases/` records what happened in real projects and what can be learned from it.

The cases module should help future readers see not only what agent did, but also what humans judged, what risks appeared, what verification signals mattered, and which parts should or should not be reused.

## README Requirements

Create:

```text
zh/cases/README.md
```

Required sections:

- Purpose
- What A Case Should Capture
- What A Case Is Not
- Suggested Workflow
- Case Template
- Skill Capture Boundary
- Related Docs

The README should state:

- cases should be based on real projects or real tasks,
- cases should include human decisions and verification, not only agent actions,
- cases may be incomplete while work is ongoing,
- cases should avoid sensitive information,
- cases should not publish secrets, private data, customer data, production details, or internal credentials,
- cases can propose skill candidates, but do not automatically create or promote skills.

## Template Requirements

Create:

```text
zh/cases/CASE.template.md
```

The template should be copyable and structured.

Required fields:

- Case title
- Date
- Project background
- Project level
- Initial goal
- Starting state
- Infra used
- Playbooks used
- Cookbook scenarios used
- Loop usage
- Skill workflow usage
- Agent actions
- Human decisions
- Key decisions
- Verification signals
- Risks and handling
- What worked
- What failed or was confusing
- Skill candidates
- Reusable patterns
- Non-reusable parts
- Follow-up actions

The template should encourage evidence:

- file paths,
- commands,
- commits,
- PRs,
- review comments,
- CI or verification results,
- explicit human decisions.

The template should avoid pretending every case is successful.

## Case Quality Rules

Cases should:

- record concrete project context,
- include the project level and why,
- identify which infra was actually used,
- distinguish agent actions from human decisions,
- include verification signals or say when they were missing,
- record risks and stop/escalate moments,
- identify what should not be reused,
- propose skill candidates only with evidence.

Cases should not:

- expose secrets, credentials, private customer data, or sensitive production details,
- rewrite cookbook content as a story,
- hide failures or human interventions,
- present unverified agent output as fact,
- turn one-off outcomes into general rules without evidence.

## Cookbook Entrypoint

`README.zh.md` already lists `zh/cases/` in the content map. This change does not need to alter the root README unless wording needs a small link update.

The cases README should link to the relevant modules instead:

- `../guides/project-levels.md`
- `../guides/infra-by-level.md`
- `../guides/infra-checklists.md`
- `../playbooks/start-a-new-project.md`
- `../cookbook/README.md`
- `../loops/README.md`
- `../skill-workflow/README.md`

## Acceptance Criteria

- `zh/cases/README.md` exists.
- `zh/cases/CASE.template.md` exists.
- README explains cases as real project records, not tutorials or cookbook replacements.
- README explains that cases can be incomplete while work is ongoing.
- README includes safety guidance for secrets, private data, production details, and sensitive information.
- README links related existing modules.
- Template includes all required fields.
- Template distinguishes agent actions from human decisions.
- Template includes verification, risks, failures/confusion, reusable patterns, non-reusable parts, and skill candidates.
- Template does not include a fictional example case.
- English stub files are unchanged.
- Related links point to existing files.
- Markdown whitespace checks pass.
