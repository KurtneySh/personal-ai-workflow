# Project Workspace Templates Design

Date: 2026-06-13

## Goal

Add the first project template set for the AI workflow handbook.

This phase focuses only on project workspace templates: the minimal documents that help a project become AI-readable, executable, verifiable, and easy to resume.

## Scope

Create the Chinese project template section:

```text
zh/templates/project/
  README.md
  README.template.md
  SPEC.template.md
  STATE.template.md
  AGENTS.template.md
```

Out of scope for this phase:

- task templates,
- collaboration templates,
- risk templates,
- skill templates,
- case templates,
- English translations,
- script-based template generation.

## Design Direction

Use the "index teaches, templates copy" model.

`zh/templates/project/README.md` explains how to choose and use the templates by project level.

The four `.template.md` files stay practical and copyable. Each template includes short guidance, then a `可复制模板 / Copyable Template` section that can be copied into another project.

## Language Style

Use Chinese-first writing with key bilingual labels for future translation alignment.

Examples:

- `用途 / Purpose`
- `推荐级别 / Recommended Levels`
- `必填部分 / Required Sections`
- `可选部分 / Optional Sections`
- `AI 生成提示 / AI Generation Prompt`
- `常见错误 / Common Mistakes`
- `可复制模板 / Copyable Template`

## Files And Responsibilities

### `zh/templates/project/README.md`

Purpose: index and usage guide for project workspace templates.

It should explain:

- what this template set is for,
- how Level 1-4 projects should use the templates,
- which templates are required or optional by level,
- how to ask AI to generate first drafts,
- common mistakes when using templates.

Recommended structure:

```markdown
# Project Workspace Templates

## 用途 / Purpose
## 适用方式 / How to Use
## Level 1-4 推荐表
## 模板选择建议
## 让 AI 生成初稿
## 常见错误 / Common Mistakes
```

### `zh/templates/project/README.template.md`

Purpose: project entrypoint template for humans and agents.

This template should help a reader quickly understand:

- what the project is,
- who it is for,
- how to run it,
- how to verify it,
- where to find project context.

Copyable sections:

```markdown
# 项目名称

## 项目是什么
## 适合谁使用
## 快速开始
## 常用命令
## 验证方式
## 目录结构
## 当前状态
## 相关文档
```

### `zh/templates/project/SPEC.template.md`

Purpose: project goal and boundary template.

This template should reduce repeated explanation of product direction, scope, and key decisions.

Copyable sections:

```markdown
# SPEC

## 背景
## 目标
## 非目标
## 用户 / 使用场景
## 核心功能
## 约束
## 成功标准
## 关键决策
## 风险与开放问题
```

### `zh/templates/project/STATE.template.md`

Purpose: short-term project state and handoff template.

This template is expected to change often. It should optimize for clear resumption rather than long-term polish.

Copyable sections:

```markdown
# STATE

## 当前阶段
## 当前目标
## 下一步
## 阻塞
## 最近完成
## 最近决策
## 已知问题
## 给下一个 agent 的上下文
```

### `zh/templates/project/AGENTS.template.md`

Purpose: agent behavior contract template.

This template should define how agents work in a project. It should not become a generic prompt. It should record project-specific rules, verification commands, forbidden actions, completion criteria, and human approval gates.

Copyable sections:

```markdown
# AGENTS

## 项目上下文
## 工作原则
## 常用命令
## 验证要求
## 代码与文档约定
## 禁止操作
## 需要人类确认的操作
## 完成标准
## Claude Code Notes
## Codex CLI Notes
```

The Claude Code and Codex CLI notes are for tool differences only. They do not replace the main project rules.

## Level Rules

Level 1:

- `README.md` is required.
- `STATE.md` is recommended.
- `SPEC.md` is optional.
- `AGENTS.md` is optional; short agent rules can live in the README.

Level 2:

- `README.md` is required.
- `SPEC.md` is required.
- `STATE.md` is required.
- `AGENTS.md` is required.

Level 3:

- all Level 2 templates are required.
- `AGENTS.md` should include collaboration, review, ownership, and handoff rules.

Level 4:

- all Level 3 templates are required.
- `AGENTS.md` should include permission boundaries, human approval gates, high-risk operations, and rollback expectations.
- `SPEC.md` should make security, privacy, compliance, production, or other high-risk constraints explicit.

## Shared Template Structure

Each `.template.md` file should use this structure:

```markdown
# <Template Name> Template

## 用途 / Purpose
## 推荐级别 / Recommended Levels
## 必填部分 / Required Sections
## 可选部分 / Optional Sections
## AI 生成提示 / AI Generation Prompt
## 常见错误 / Common Mistakes
## 可复制模板 / Copyable Template
```

The guidance sections should stay concise. The copyable section is the primary artifact.

## Quality Rules

- Templates should be examples, not mandatory standards.
- Templates should say which sections are required and which are optional.
- Templates should include prompts that ask AI to read the current repository before drafting.
- AI generation prompts must tell the agent to mark assumptions and avoid inventing commands that do not exist.
- `AGENTS.template.md` must include human approval gates and forbidden actions.
- `STATE.template.md` must emphasize frequent updates and handoff clarity.
- `SPEC.template.md` must distinguish goals from non-goals.
- `README.template.md` must include verification entrypoints.

## Verification

Implementation should verify:

- all five files exist,
- `README.zh.md` can link to `zh/templates/project/README.md`,
- no template contains unfinished-marker text or empty filler instructions,
- Markdown formatting passes `git diff --check`,
- content remains scoped to project workspace templates.
