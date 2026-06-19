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
