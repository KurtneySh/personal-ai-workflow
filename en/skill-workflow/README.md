# Skill Workflow

A skill is how this handbook turns repeated AI development workflows into reusable capability.

The default stance is conservative: do not turn every repeated action into a skill. Capture stable, verifiable, clearly bounded workflows first as project-level skills. Consider promotion to a system-level skill candidate only after the workflow has repeatedly worked across tasks or projects.

## Default Stance

- Do not rush to create a skill by default.
- First ask whether the repeated workflow is stable, teachable, and verifiable.
- If the workflow still depends on the current repo, team rules, commands, or risk boundaries, capture it as a project-level skill.
- Consider a system-level skill candidate only after multiple real uses show that the workflow can stand outside one project.
- An agent may propose a skill candidate, but it must not create, install, or promote a system-level skill on its own.
- A system-level skill requires human review because it can change future agent behavior across multiple projects.

## What Is A Skill

In this handbook, a skill is not just a prompt. It is a reusable AI working capability that can:

- Recognize when the workflow applies.
- Identify the required inputs and context.
- Follow a fixed sequence of steps.
- Respect action boundaries and forbidden actions.
- Verify results with a real verification signal.
- Know when to stop / escalate or hand off to a human.

## Project-Level Skill

A project-level skill is a reusable workflow or rule bound to one project.

It may live in:

- `AGENTS.md`
- `STATE.md`
- A project runbook
- A checklist
- A task template
- A future local skill file for the project

A project-level skill is appropriate when:

- The workflow depends on this repo's commands, directories, architecture, or conventions.
- The workflow depends on this team's review, delivery, or coordination rules.
- The workflow touches risk boundaries that are specific to this project.
- The workflow has not yet proved that it can transfer to other projects.

A project-level skill is not a lesser skill. For many workflows, staying project-level is the right long-term choice.

## System-Level Skill

A system-level skill is a cross-project capability that can stand outside any one repo.

It is similar to a formal `SKILL.md` capability package and should have:

- Stable trigger conditions.
- A clear scope.
- Explicit non-applicable cases.
- Reliable verification.
- Examples and non-examples.
- A maintenance owner.

A system-level skill affects future agent behavior across multiple projects, so an agent may only propose it as a candidate. Promotion and installation require a human gate.

## What Should Not Become A Skill

Do not capture a workflow as a skill when:

- It happened once and has no repeated evidence.
- It depends mainly on human judgment in the moment.
- Its inputs, outputs, or boundaries are still unclear.
- It has no observable verification signal.
- It would encode a temporary workaround as permanent guidance.
- It would let an agent expand its own authority without review.
- Its value is only that it saves a few words in a prompt.

When these conditions apply, keep the pattern in task notes, a runbook, a checklist, or a project-level draft until the evidence is stronger.

## Skill Candidate Sources

Skill candidates usually come from four signals:

- A human repeatedly coordinates the same kind of task, writes similar prompts, or corrects the same kind of agent error.
- After a loop ends, an agent notices repeated steps, repeated verification rules, or reusable recovery paths.
- Project infra has already stabilized the rule in places such as `README.md`, `STATE.md`, `SPEC.md`, `AGENTS.md`, CI, review conventions, or a runbook.
- Multiple real cases show the same problem shape and the same working response.

## When To Capture

A workflow is worth capturing as a skill candidate when most of these are true:

- It appears repeatedly, not just once.
- It is stable enough to describe.
- Its inputs, outputs, and boundaries are clear.
- It can be taught to an agent instead of relying only on human improvisation.
- It has a real verification signal.
- It reduces coordination cost compared with rewriting ad hoc prompts each time.
- It is likely to help future tasks.
- It can explain where it does not apply.

If these criteria are not clear, do not make a system-level skill. Keep recording it in task notes, a project runbook, a checklist, or a project-level skill draft.

## Project-Level Skill vs System-Level Skill

| Question | Project-level skill | System-level skill candidate |
| --- | --- | --- |
| Scope | One repo, product, team, or task domain | Multiple tasks or projects |
| Evidence required | Repeatedly works inside the current project | Repeatedly works across tasks or projects |
| Location | `AGENTS.md`, `STATE.md`, runbook, checklist, task template | Formal skill package or system-level skill candidate document |
| Owner | Project owner, reviewer, or maintainer | Explicit maintenance owner |
| Verification | Real commands, review, CI, checklist, or verification signal from this project | Verification rules and examples that do not depend on one project |
| Promotion condition | Already reduces repeated coordination inside the project | Has proved reusable outside the current project |
| Retirement condition | Project rules changed, triggers disappeared, or it repeatedly causes errors | No longer works across projects, lacks maintenance, or has unclear triggers |

Use these rules:

- Choose a project-level skill when the workflow depends on current repo commands, directories, architecture, team rules, or risk boundaries.
- Propose a system-level skill candidate only when the workflow has been verified across multiple tasks or projects and does not depend on hidden project context.
- Do not promote a project-level skill to a system-level skill just because it was useful once.

## Human, Agent, And Loop Boundaries

Skill judgment depends on who is observing the repetition.

### Human-Led Workflows

When a human coordinates agent work, the human is responsible for judging which workflows repeat, which prompts are rewritten, and which agent errors keep being corrected.

An agent may help produce a skill candidate report, but it does not make the final judgment.

The report should answer:

- What is the repeated pattern?
- How many times has it appeared?
- Where does the evidence come from?
- Is it a better fit for a project-level skill or a system-level skill candidate?
- What verification is needed?
- Where does it not apply?

### Loop-Led Workflows

When a task is designed as a loop with a clear goal, action boundaries, verification signal, stop condition, and escalation condition, the agent may propose a project-level skill draft when the loop ends.

The agent still only proposes a draft. The draft is not an adopted project rule until a human reviews it and chooses to write it into `AGENTS.md`, a runbook, a checklist, or a task template.

Loop workflows are useful because they already contain the structure a skill needs: trigger, allowed actions, forbidden actions, verification, stop / escalate rules, and handoff points.

### Non-Loop Workflows

Non-loop workflows rely more on human coordination. The human usually sees repetition across separate tasks, review comments, handoffs, and prompt rewrites before the agent can reliably identify the pattern.

In non-loop work, an agent may summarize evidence and propose a candidate, but the human must decide whether the workflow is stable enough to capture, whether it should stay project-level, and whether any promotion should be considered.

### System-Level Skill Promotion

Promotion to a system-level skill must be confirmed by a human.

An agent may:

- Summarize evidence.
- Draft a system-level skill candidate.
- Identify scope, non-goals, failure modes, and verification.

An agent must not:

- Automatically create a system-level skill.
- Automatically install a skill.
- Automatically promote a project-level skill to a system-level skill.
- Change the default working style of future projects without human review.

The human gate for system-level skill installation is mandatory. The gate should confirm evidence, scope, maintenance ownership, verification signal, and failure modes before installation.

## Templates

These are documentation templates. They can be copied into a project's `AGENTS.md`, `STATE.md`, runbook, checklist, or task instructions.

### Project Skill Template

```markdown
## Project Skill: <name>

- Skill name:
- Trigger:
- Project context:
- Allowed actions:
- Forbidden actions:
- Inputs:
- Steps:
- Verification:
- State updates:
- Owner or reviewer:
- Examples:
- Not applicable when:
- Last reviewed:
```

Fill it in with these rules:

- `Trigger` explains when the project-level skill should be used.
- `Project context` names the repo conventions, commands, directories, risk boundaries, or team rules it depends on.
- `Allowed actions` and `Forbidden actions` must be specific enough that the agent cannot expand the scope on its own.
- `Verification` must point to real commands, review, CI, a human checklist, or an explicit verification signal.
- `Not applicable when` lists counterexamples so the skill is not overgeneralized.

### System Skill Candidate Template

```markdown
## System Skill Candidate: <name>

- Candidate name:
- Problem it solves:
- Evidence from repeated use:
- Scope:
- Non-goals:
- Required inputs:
- Workflow steps:
- Tool or context requirements:
- Verification:
- Failure modes:
- Examples:
- Non-examples:
- Maintenance owner:
- Promotion decision:
```

Fill it in with these rules:

- `Evidence from repeated use` must name the tasks or projects where the capability repeatedly worked.
- `Scope` and `Non-goals` must prevent overgeneralization.
- `Verification` cannot only say "looks right to a human"; it must describe observable judgment signals.
- `Failure modes` explains when to stop / escalate or hand off to a human.
- `Promotion decision` records candidate status, approval status, or rejection rationale. It must not let an agent approve promotion automatically.

## Prompt Guidance

These prompts are for judgment and drafting. They are not permission to automatically create or install a system-level skill.

### Human-Led: Skill Candidate Report

#### Claude Code

```text
Read the current repo's README, STATE, optional SPEC/AGENTS files, relevant runbooks, and recent task records.

Judge whether the repeated workflow is worth capturing as a skill.

Requirements:
- First describe the repeated pattern.
- Distinguish project-level skill from system-level skill candidate.
- List which files, tasks, or review feedback provide evidence.
- Define scope, boundaries, non-applicable cases, and verification.
- If evidence is insufficient, recommend continued recording instead of creating a skill.
- Do not automatically create, install, or promote a system-level skill.

Output:
- Capture recommendation
- Recommended type: project-level skill / system-level skill candidate / do not capture yet
- Rationale
- Evidence
- Risks and non-applicable cases
- Suggested next steps
```

#### Codex CLI

```text
Based on current repo files and real task records, judge whether a repeated workflow is worth capturing as a skill.

First read README.md, STATE.md, and, if present, SPEC.md, AGENTS.md, runbooks, and checklists.

Requirements:
- Use only information that actually exists in the repo.
- Decide whether it is a better fit for a project-level skill, a system-level skill candidate, or no capture yet.
- If repeated evidence, verification signal, or clear boundaries are missing, say so explicitly.
- Do not automatically create, install, or promote a system-level skill.

Output:
- Repeated pattern
- Recommended type
- Evidence
- Project dependencies
- Verification method
- Non-applicable cases
- Questions requiring human confirmation
```

### Loop-Led: Project-Level Skill Draft

#### Claude Code

```text
Based on the loop record that just finished, judge whether there is a project-level skill worth capturing.

Requirements:
- Judge only when the loop already had a clear goal, action boundaries, verification signal, stop condition, and escalation condition.
- Draft only a project-level skill. Do not create a system-level skill.
- Define trigger, project context, allowed actions, forbidden actions, verification, and not applicable when.
- If repeated evidence is insufficient, output "do not capture yet."
- The draft must wait for human review before it is written into project rules.
```

#### Codex CLI

```text
Read the state record for this task or loop and judge whether a project-level skill should be drafted.

Requirements:
- Use only existing records. Do not invent steps that did not happen.
- If you find a reusable pattern, output a draft using the Project Skill Template.
- If repeated evidence, verification signal, or boundaries are missing, explain why this should not be captured yet.
- Do not modify AGENTS.md, STATE.md, or any system-level skill directory unless a human explicitly confirms.
```

### System-Level Skill Candidate Evaluation

#### Claude Code

```text
Evaluate whether a project-level skill is ready to become a system-level skill candidate.

Requirements:
- Check for repeated evidence across tasks or projects.
- Check whether it can stand outside hidden context from the current repo.
- Define scope, non-goals, required inputs, verification, failure modes, examples, and non-examples.
- If evidence is insufficient, recommend maintaining it as a project-level skill.
- Do not create, install, or promote a system-level skill. Output only the candidate evaluation.
```

#### Codex CLI

```text
Using the current repo's project-level skill, task records, and case records, evaluate whether it is suitable as a system-level skill candidate.

Requirements:
- Output only an evaluation and candidate draft.
- Explicitly describe cross-task or cross-project evidence.
- Explicitly describe which parts still depend on the current project.
- If transferability cannot be proved, recommend against promotion.
- Do not write to the system-level skill directory. Do not install a skill.
```

## Maintenance

- After each real use, check whether the skill reduced repeated coordination.
- Update the skill when commands, project rules, verification signal, permission boundaries, or risk boundaries change.
- Retire the skill when its trigger no longer appears or it repeatedly causes incorrect behavior.
- The owner of a project-level skill may be a project maintainer, reviewer, or the person responsible for the workflow.
- A system-level skill candidate must record a maintenance owner and promotion decision.
- Keep enough context for the next human or agent to understand why the skill exists.

## Related Docs

- [Loops](../loops/README.md)
- [Playbooks](../playbooks/README.md)
- [Project-Level Judgment](../guides/project-levels.md)
- [Infra Recommendations By Project Level](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
