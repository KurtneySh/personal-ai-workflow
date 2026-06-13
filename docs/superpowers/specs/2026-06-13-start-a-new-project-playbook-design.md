# Start a New Project Playbook Design

Date: 2026-06-13

## Goal

Add the first executable playbook for the AI workflow handbook.

The playbook should connect the existing guides and project workspace templates into an end-to-end workflow for building an AI-readable project workspace.

## Scope

Create one Chinese playbook:

```text
zh/playbooks/start-a-new-project.md
```

The playbook covers two paths:

1. start from an empty repository,
2. retrofit an existing repository with AI workflow infrastructure.

Out of scope for this phase:

- a full Claude Code cookbook,
- a full Codex CLI cookbook,
- PR and CI workflows,
- multi-agent delegation workflows,
- loop design beyond stop/escalate guidance,
- English translation.

## Design Direction

Use a single-document, two-path structure.

The playbook should be directly executable. Each major step should include:

- input,
- action,
- output,
- verification,
- AI prompt.

The main flow stays tool-neutral, while key steps include short Claude Code and Codex CLI prompts.

## File Structure

Create:

```text
zh/playbooks/start-a-new-project.md
```

The document should use this structure:

```markdown
# Start a New Project Playbook

## 用途 / Purpose
## 适用场景 / When to Use
## 成功标准 / Success Criteria
## 停止与升级条件 / Stop and Escalate
## 路径 A：从空仓库开始
## 路径 B：改造已有项目
## 关键步骤的 Claude Code / Codex CLI 提示
## 最终检查清单 / Final Checklist
## 相关文档 / Related Docs
```

## Success Criteria

The workflow is successful when:

- `README.md` exists,
- `STATE.md` exists,
- Level 2+ projects also have `SPEC.md` and `AGENTS.md`,
- at least one real verification command can run,
- `STATE.md` records current stage, next step, and blockers,
- `AGENTS.md` records verification requirements, forbidden actions, and completion criteria,
- AI-generated commands, tests, deployment steps, and team rules have been reviewed by a human,
- an agent can use the files to understand the project and take over a small low-risk task.

## Stop And Escalate Conditions

The playbook must tell readers to stop and ask for human judgment when:

- project level cannot be determined, especially if Level 4 might apply,
- the project involves production, permissions, authentication, payment, sensitive data, security, or compliance,
- no real verification command can be found,
- AI invents commands, tests, deployment flows, or team rules,
- `README.md`, `SPEC.md`, `STATE.md`, and `AGENTS.md` contradict each other,
- the repository has many uncommitted changes and the work would touch the same files,
- the workflow requires new dependencies, data model changes, permission boundary changes, or broad refactoring,
- the agent cannot explain the current project state or next step.

## Path A: Empty Repository

The empty-repo path should include these steps:

1. write a one-sentence project goal,
2. determine project level,
3. choose minimum infrastructure,
4. copy project templates,
5. ask AI to draft required workspace files,
6. human-review key fields,
7. establish at least one verification command,
8. update `STATE.md` with next step and blockers,
9. ask the agent to perform a small smoke-test task using the new context.

Each step should include input, action, output, verification, and AI prompt.

## Path B: Existing Repository Retrofit

The existing-repo path should include these steps:

1. scan the existing repository,
2. determine project level,
3. inventory existing infrastructure,
4. identify missing infrastructure,
5. generate or update `README.md`, `STATE.md`, `SPEC.md`, and `AGENTS.md` from existing evidence,
6. verify that commands are real and runnable,
7. human-review goals, boundaries, forbidden actions, and completion criteria,
8. update `STATE.md` with current state and next step,
9. ask the agent to summarize the project and perform one low-risk task.

Each step should include input, action, output, verification, and AI prompt.

## Tool Prompt Guidance

The playbook should not become a full cookbook.

It should include short tool prompts for key steps:

- repository scan,
- project level judgment,
- missing infrastructure analysis,
- template generation,
- verification command validation,
- final agent smoke test.

Claude Code prompt example:

```text
请先阅读 README.md、SPEC.md、STATE.md、AGENTS.md。
判断当前项目级别，并列出缺失的 AI workflow infra。
不要修改文件，先输出计划和需要人类确认的问题。
```

Codex CLI prompt example:

```text
请在当前 repo 内检查 README.md、SPEC.md、STATE.md、AGENTS.md 和可用脚本。
判断当前项目级别，列出缺失 infra，并给出最小补齐方案。
不要编造不存在的命令；如果找不到验证命令，请明确说明。
```

## Final Checklist

The playbook should end with this checklist:

```markdown
- [ ] 项目级别已判断
- [ ] `README.md` 已创建或更新
- [ ] `STATE.md` 已创建或更新
- [ ] Level 2+ 项目有 `SPEC.md`
- [ ] Level 2+ 项目有 `AGENTS.md`
- [ ] 验证命令真实可运行
- [ ] `STATE.md` 写清下一步和阻塞
- [ ] `AGENTS.md` 写清禁止操作和完成标准
- [ ] 人类 review 过 AI 生成的命令和规则
- [ ] agent 能复述项目状态并接手一个小任务
```

## Related Docs

The playbook should link to:

- `zh/guides/project-levels.md`,
- `zh/guides/infra-by-level.md`,
- `zh/guides/infra-checklists.md`,
- `zh/templates/project/README.md`.

## Quality Rules

- The playbook should be executable, not just conceptual.
- The two paths should be clearly separated.
- Each major step should include input, action, output, verification, and AI prompt.
- Prompts must tell AI not to invent commands, tests, deployment flows, or team rules.
- Stop/escalate conditions must appear before the step-by-step paths.
- The playbook should not duplicate full template contents.
- The playbook should not become a full Claude Code or Codex CLI cookbook.
- The playbook should be scoped to project workspace setup, not PR, CI, loop, or multi-agent workflows.

## Verification

Implementation should verify:

- `zh/playbooks/start-a-new-project.md` exists,
- all required top-level sections exist,
- both paths contain input, action, output, verification, and AI prompt,
- stop/escalate conditions are present,
- related links point to existing files,
- no unresolved drafting markers or empty filler instructions are present,
- Markdown formatting passes `git diff --check`.
