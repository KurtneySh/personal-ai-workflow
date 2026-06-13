# AI Workflow Handbook Foundation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build the repository foundation for the bilingual AI workflow handbook: public entry files, mirrored language structure, Chinese core guides, English placeholders, and temporary repo working records.

**Architecture:** This plan implements the decision-first handbook shell from the approved design spec. `README.md` and `README.zh.md` act as navigation hubs; Chinese content under `zh/` is the v1 source of truth; English content under `en/` mirrors the structure with placeholders; `docs/repo/` stores temporary buildout records.

**Tech Stack:** Markdown documentation, Git, Conventional Commits.

---

## File Structure

Create or modify these files:

```text
.gitignore
README.md
README.zh.md

zh/guides/project-levels.md
zh/guides/infra-by-level.md
zh/guides/infra-checklists.md
zh/reference/ai-development-workflow-best-practices.md

en/guides/README.md
en/playbooks/README.md
en/cookbook/README.md
en/loops/README.md
en/skill-workflow/README.md
en/cases/README.md
en/templates/README.md
en/reference/README.md

docs/repo/translation-status.md
docs/repo/maintenance-guide.md
docs/repo/decisions/0001-handbook-information-architecture.md
```

Keep `.superpowers/` untracked and local-only. Move the existing Chinese best-practices document into `zh/reference/` during Task 1.

## Task 1: Repository Hygiene And Source Document Move

**Files:**
- Modify: `.gitignore`
- Move: `AI 开发工作流最佳实践.md` to `zh/reference/ai-development-workflow-best-practices.md`

- [ ] **Step 1: Add local brainstorm artifacts to `.gitignore`**

Write `.gitignore` with this content:

```gitignore
.superpowers/
```

- [ ] **Step 2: Move the Chinese best-practices document into `zh/reference/`**

Run:

```bash
mkdir -p zh/reference
git mv "AI 开发工作流最佳实践.md" zh/reference/ai-development-workflow-best-practices.md
```

Expected: if the source file is untracked, `git mv` fails with `fatal: not under version control`. In that case, use:

```bash
mv "AI 开发工作流最佳实践.md" zh/reference/ai-development-workflow-best-practices.md
git add zh/reference/ai-development-workflow-best-practices.md
```

- [ ] **Step 3: Verify local-only artifacts are ignored**

Run:

```bash
git status --short
```

Expected: `.superpowers/` does not appear. The moved reference document and `.gitignore` appear as pending changes.

- [ ] **Step 4: Commit repository hygiene changes**

Run:

```bash
git add .gitignore zh/reference/ai-development-workflow-best-practices.md
git commit -m "docs: organize workflow reference source"
```

Expected: commit succeeds with a Conventional Commits message.

## Task 2: Root Navigation Entrypoints

**Files:**
- Create: `README.md`
- Create: `README.zh.md`

- [ ] **Step 1: Write English public entrypoint**

Create `README.md` with this content:

```markdown
# AI Workflow Handbook

This repository is a handbook for building practical AI-assisted development workflows.

It is designed to help developers decide:

- what level of AI workflow infrastructure a project needs,
- which documents and feedback loops make an agent useful,
- how to use Claude Code and Codex CLI in concrete development scenarios,
- when to automate work as a loop,
- when a repeated workflow should become a reusable skill,
- how humans and agents should divide responsibility.

## Current Language Status

Chinese is currently the source of truth for v1 content.

Start here if you read Chinese:

- [中文入口](README.zh.md)

The English documentation will mirror the Chinese structure after the Chinese version stabilizes.

## Repository Map

- `zh/guides/`: project levels, infrastructure recommendations, and checklists.
- `zh/playbooks/`: end-to-end workflows.
- `zh/cookbook/`: scenario-driven Claude Code and Codex CLI usage.
- `zh/loops/`: loop readiness, loop design, and stop conditions.
- `zh/skill-workflow/`: when and how to capture reusable skills.
- `zh/cases/`: practical case studies.
- `zh/templates/`: example document templates.
- `zh/reference/`: concepts, philosophy, glossary, and appendix material.
- `en/`: English mirror structure and translation placeholders.
```

- [ ] **Step 2: Write Chinese navigation entrypoint**

Create `README.zh.md` with this content:

```markdown
# AI 开发工作流手册

这个仓库用于沉淀 AI 开发工作流的知识库和使用说明书。

它不把 AI 当成聊天框，而是把项目建设成 AI 可读、可执行、可验证、可复用的工作台。

## 3 分钟使用路径

1. 阅读 [项目级别判断](zh/guides/project-levels.md)，判断当前项目属于 Level 1-4 的哪一类。
2. 阅读 [按级别推荐 Infra](zh/guides/infra-by-level.md)，选择最小可用的 AI 工作流基础设施。
3. 阅读 [Infra Checklist](zh/guides/infra-checklists.md)，按能力补充上下文、规则、验证、协作、风险和 loop 约束。
4. 遇到具体任务时，查 `zh/playbooks/` 和 `zh/cookbook/`。
5. 遇到重复流程时，查 `zh/skill-workflow/` 判断是否值得沉淀为 skill。
6. 想让 agent 自动循环时，先查 `zh/loops/` 判断是否适合 loop 化。

## 内容地图

- `zh/guides/`：项目级别、infra 推荐和 checklist。
- `zh/playbooks/`：从目标到完成的端到端任务流程。
- `zh/cookbook/`：按场景使用 Claude Code 和 Codex CLI。
- `zh/loops/`：loop 是否该做、怎么设计、怎么停止。
- `zh/skill-workflow/`：什么时候把流程沉淀成 skill。
- `zh/cases/`：实战案例，记录项目背景、infra 选择、agent 行动和人的判断。
- `zh/templates/`：纯文档模板示例。
- `zh/reference/`：概念、哲学、术语和附录材料。

## 当前状态

- 中文内容是 v1 的 source of truth。
- `README.md` 是面向未来公开分享的英文默认入口。
- 英文目录先保持同构占位，等中文内容稳定后再翻译。
- 模板只是示例，不是强制标准。
```

- [ ] **Step 3: Verify root navigation links**

Run:

```bash
test -f README.md && test -f README.zh.md && test -f zh/reference/ai-development-workflow-best-practices.md
```

Expected: command exits with status 0.

- [ ] **Step 4: Commit root entrypoints**

Run:

```bash
git add README.md README.zh.md
git commit -m "docs: add bilingual handbook entrypoints"
```

Expected: commit succeeds.

## Task 3: Chinese Project Level Guide

**Files:**
- Create: `zh/guides/project-levels.md`

- [ ] **Step 1: Create the guides directory**

Run:

```bash
mkdir -p zh/guides
```

Expected: directory exists.

- [ ] **Step 2: Write project level guide**

Create `zh/guides/project-levels.md` with this structure and content:

```markdown
# 项目级别判断

项目级别不是身份标签，而是选择 AI 工作流 infra 的起点。

判断顺序：

1. 这个项目是否会长期维护？
2. 是否有多人协作、review 或交接？
3. 是否接触生产系统、权限、敏感数据、支付、认证、安全或合规？
4. 是否需要 CI/CD、发布、回滚或审计？

## Level 1：个人小项目 / 实验项目

适用场景：

- 一次性脚本
- 快速原型
- 个人学习项目
- 可以接受手动修复和低成本重做的项目

推荐目标：

- 让 agent 能理解项目是什么。
- 让 agent 至少能运行一个验证命令。
- 不为短期项目增加过多流程。

升级信号：

- 项目开始反复迭代。
- 你经常需要重新解释上下文。
- 错误开始难以靠记忆追踪。

## Level 2：个人长期维护项目

适用场景：

- 会持续维护的个人工具
- 有明确目标和边界的 side project
- 需要反复让 agent 参与开发的项目

推荐目标：

- 把目标、范围、状态和 agent 规则写进文件。
- 让 agent 能在本地完成读代码、修改、验证的闭环。
- 记录重要决策，避免重复解释。

升级信号：

- 开始有合作者。
- 需要 review 或交接。
- 需要稳定的 CI 或发布流程。

## Level 3：多人协作项目

适用场景：

- 多人共同维护
- 需要 PR review
- 有明确 ownership
- agent 的修改需要被稳定审查和交接

推荐目标：

- 明确 ownership、review、CI 和 handoff。
- 降低 agent 并行工作带来的上下文冲突。
- 让人负责关键判断，agent 负责可验证执行。

升级信号：

- 涉及生产部署。
- 涉及权限、敏感数据、认证、支付或合规。
- 修改失败会造成高成本影响。

## Level 4：生产 / 高风险项目

适用场景：

- 生产系统
- 高权限系统
- 涉及敏感数据、支付、认证、安全或合规
- 影响大量用户或关键业务流程

推荐目标：

- 明确风险、权限、审批、回滚和审计。
- 给 loop 设置硬停止条件。
- 把人的判断权保留在架构、权限、依赖、发布和回滚上。

## 使用原则

- 从最低可用级别开始，不为了形式添加文件。
- 如果项目有高风险特征，即使是个人项目，也临时采用 Level 4 的相关 checklist。
- Level 是推荐起点，真正要看风险、协作成本和验证能力。
```

- [ ] **Step 3: Verify the guide exists and contains all four levels**

Run:

```bash
rg -n "Level 1|Level 2|Level 3|Level 4" zh/guides/project-levels.md
```

Expected: four matches, one for each project level.

- [ ] **Step 4: Commit project level guide**

Run:

```bash
git add zh/guides/project-levels.md
git commit -m "docs: define project level guide"
```

Expected: commit succeeds.

## Task 4: Chinese Infrastructure Guides

**Files:**
- Create: `zh/guides/infra-by-level.md`
- Create: `zh/guides/infra-checklists.md`

- [ ] **Step 1: Write infra-by-level guide**

Create `zh/guides/infra-by-level.md` with this content:

```markdown
# 按项目级别推荐 Infra

AI 工作流 infra 的目标不是文档越多越好，而是让 agent 拥有稳定上下文、可执行反馈和明确边界。

## Level 1：个人小项目 / 实验项目

最小 infra：

- `README.md`：项目是什么、如何运行。
- `STATE.md`：当前状态、下一步、已知问题。
- 至少一个验证命令：`test`、`run`、`lint` 或等价命令。

可选：

- task brief
- 简短 prompt / command log

## Level 2：个人长期维护项目

最小 infra：

- Level 1 全部
- `SPEC.md`：目标、范围、非目标、关键决策。
- `AGENTS.md`：agent 工作规则。
- `docs/decisions/`：重要决策记录。
- tests 或明确验证脚本。

可选：

- skill index
- reusable task briefs
- release notes

## Level 3：多人协作项目

最小 infra：

- Level 2 全部
- `CONTRIBUTING.md`
- `CODEOWNERS` 或 ownership 文档
- PR checklist
- CI checks
- issue / task template
- handoff notes
- multi-agent delegation contract

可选：

- team skill registry
- architecture decision records
- review playbooks

## Level 4：生产 / 高风险项目

最小 infra：

- Level 3 全部
- risk register
- security / privacy checklist
- rollback plan
- human approval gates
- deployment checklist
- incident / audit notes
- loop stop conditions
- permission boundaries for agents

可选：

- threat model
- staging verification matrix
- change freeze rules
- postmortem template

## 使用方式

先选择项目级别，再用 `infra-checklists.md` 按能力补充。

不要为了满足某个级别而机械创建所有文件。真正需要的是上下文、规则、验证、协作和风险控制能力。
```

- [ ] **Step 2: Write capability-based checklist guide**

Create `zh/guides/infra-checklists.md` with this content:

```markdown
# Infra Checklist

这份 checklist 按能力组织，而不是按文件名组织。

同一个能力可以通过不同文件实现。关键是让 agent 能稳定理解、执行、验证和停止。

## Checklist Item Schema

每个 infra 项都应回答：

- Item
- Purpose
- Recommended levels
- Required when
- Example file
- How AI uses it
- Common mistakes

## 1. Context / 上下文

目的：让 agent 知道项目是什么、当前状态是什么、边界在哪里。

基础项：

- `README.md`
- `STATE.md`

增强项：

- `SPEC.md`
- `docs/decisions/`
- architecture notes
- glossary

常见错误：

- README 只有安装命令，没有项目目标。
- STATE 长期不更新。
- 决策只留在聊天记录里。

## 2. Instructions / 行为规则

目的：让 agent 知道在这个项目里应该怎么工作。

基础项：

- Level 1 可写在 README 内。
- Level 2+ 推荐 `AGENTS.md`。

增强项：

- coding conventions
- dependency policy
- branch / commit policy
- forbidden actions

常见错误：

- 写成通用 prompt，而不是项目规则。
- 没有验证命令。
- 没有说明哪些操作必须请求人类确认。

## 3. Feedback / 验证闭环

目的：让 agent 能判断修改是否正确。

基础项：

- 至少一个明确验证命令。

增强项：

- test
- lint
- typecheck
- build
- browser verification
- CI
- smoke test checklist

常见错误：

- 只要求 agent “看起来没问题”。
- 没有记录验证命令的预期结果。
- 本地验证和 CI 验证不一致。

## 4. Task Flow / 任务流

目的：让人和 agent 对齐目标、验收标准和停止条件。

基础项：

- task brief
- acceptance criteria

增强项：

- issue template
- PR checklist
- review checklist
- handoff notes
- changelog

常见错误：

- 任务只写“优化一下”。
- 没有完成标准。
- 没有停止条件。

## 5. Reuse / 复利沉淀

目的：把重复成功流程变成可复用资产。

增强项：

- skill workflow notes
- skill index
- reusable prompts
- playbook links
- case notes

常见错误：

- 每次重新 prompt。
- 只保存结果，不保存流程和判断。
- 没有记录失败模式。

## 6. Automation / Loop

目的：判断是否值得自动循环，以及如何安全停止。

增强项：

- loop readiness checklist
- iteration budget
- verification command
- stop conditions
- escalation rules
- human approval gates

常见错误：

- 没有验证命令就 loop。
- 没有最大迭代次数。
- 让 agent 在高风险操作上自动继续。

## 7. Collaboration / 协作

目的：降低多人或多 agent 协作中的职责不清和合并风险。

基础项：

- ownership
- PR checklist
- handoff notes

增强项：

- delegation contract
- worktree policy
- reducer role
- conflict resolution notes

常见错误：

- 多个 agent 修改同一组文件但没有 ownership。
- 没有 reducer 负责最终合并。
- review 只看总结，不看关键 diff。

## 8. Risk / 风险

目的：控制权限、数据、部署和不可逆操作。

基础项：

- risk register
- rollback plan
- security / privacy checklist
- human approval gates

增强项：

- threat model
- incident notes
- audit trail

常见错误：

- 让 agent 同时接触私有数据、不可信输入和对外通信。
- 没有回滚计划。
- 没有明确哪些操作必须人类审批。
```

- [ ] **Step 3: Verify guide links and headings**

Run:

```bash
rg -n "Level 1|Level 2|Level 3|Level 4|Context|Instructions|Feedback|Automation|Risk" zh/guides
```

Expected: matches appear in both infrastructure guide files.

- [ ] **Step 4: Commit infrastructure guides**

Run:

```bash
git add zh/guides/infra-by-level.md zh/guides/infra-checklists.md
git commit -m "docs: add infrastructure selection guides"
```

Expected: commit succeeds.

## Task 5: English Mirror Placeholders

**Files:**
- Create: `en/guides/README.md`
- Create: `en/playbooks/README.md`
- Create: `en/cookbook/README.md`
- Create: `en/loops/README.md`
- Create: `en/skill-workflow/README.md`
- Create: `en/cases/README.md`
- Create: `en/templates/README.md`
- Create: `en/reference/README.md`

- [ ] **Step 1: Create English mirror directories**

Run:

```bash
mkdir -p en/guides en/playbooks en/cookbook en/loops en/skill-workflow en/cases en/templates en/reference
```

Expected: all directories exist.

- [ ] **Step 2: Add `en/guides/README.md`**

Create `en/guides/README.md` with this content:

```markdown
# Guides

This section mirrors the Chinese source-of-truth content.

Translation is planned after the Chinese version stabilizes.

For the current content, see the matching section under `zh/`.
```

- [ ] **Step 3: Add `en/playbooks/README.md`**

Create `en/playbooks/README.md` with this content:

```markdown
# Playbooks

This section mirrors the Chinese source-of-truth content.

Translation is planned after the Chinese version stabilizes.

For the current content, see the matching section under `zh/`.
```

- [ ] **Step 4: Add `en/cookbook/README.md`**

Create `en/cookbook/README.md` with this content:

```markdown
# Cookbook

This section mirrors the Chinese source-of-truth content.

Translation is planned after the Chinese version stabilizes.

For the current content, see the matching section under `zh/`.
```

- [ ] **Step 5: Add `en/loops/README.md`**

Create `en/loops/README.md` with this content:

```markdown
# Loops

This section mirrors the Chinese source-of-truth content.

Translation is planned after the Chinese version stabilizes.

For the current content, see the matching section under `zh/`.
```

- [ ] **Step 6: Add `en/skill-workflow/README.md`**

Create `en/skill-workflow/README.md` with this content:

```markdown
# Skill Workflow

This section mirrors the Chinese source-of-truth content.

Translation is planned after the Chinese version stabilizes.

For the current content, see the matching section under `zh/`.
```

- [ ] **Step 7: Add `en/cases/README.md`**

Create `en/cases/README.md` with this content:

```markdown
# Cases

This section mirrors the Chinese source-of-truth content.

Translation is planned after the Chinese version stabilizes.

For the current content, see the matching section under `zh/`.
```

- [ ] **Step 8: Add `en/templates/README.md`**

Create `en/templates/README.md` with this content:

```markdown
# Templates

This section mirrors the Chinese source-of-truth content.

Translation is planned after the Chinese version stabilizes.

For the current content, see the matching section under `zh/`.
```

- [ ] **Step 9: Add `en/reference/README.md`**

Create `en/reference/README.md` with this content:

```markdown
# Reference

This section mirrors the Chinese source-of-truth content.

Translation is planned after the Chinese version stabilizes.

For the current content, see the matching section under `zh/`.
```

- [ ] **Step 10: Verify all English placeholders exist**

Run:

```bash
for f in en/guides/README.md en/playbooks/README.md en/cookbook/README.md en/loops/README.md en/skill-workflow/README.md en/cases/README.md en/templates/README.md en/reference/README.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 11: Commit English placeholders**

Run:

```bash
git add en
git commit -m "docs: add english mirror placeholders"
```

Expected: commit succeeds.

## Task 6: Temporary Repo Working Records

**Files:**
- Create: `docs/repo/translation-status.md`
- Create: `docs/repo/maintenance-guide.md`
- Create: `docs/repo/decisions/0001-handbook-information-architecture.md`

- [ ] **Step 1: Create repo working record directories**

Run:

```bash
mkdir -p docs/repo/decisions
```

Expected: directory exists.

- [ ] **Step 2: Write translation status**

Create `docs/repo/translation-status.md` with this content:

```markdown
# Translation Status

Chinese content under `zh/` is the v1 source of truth.

English content under `en/` mirrors the Chinese structure and starts as placeholders.

## Current Policy

- Write and stabilize Chinese content first.
- Keep English directories structurally aligned with Chinese directories.
- Translate English content after Chinese documents become stable enough to share.
- Do not mix Chinese and English versions inside the same long-form content file.

## Initial Status

| Section | Chinese | English |
| --- | --- | --- |
| guides | In progress | Placeholder |
| playbooks | Planned | Placeholder |
| cookbook | Planned | Placeholder |
| loops | Planned | Placeholder |
| skill-workflow | Planned | Placeholder |
| cases | Planned | Placeholder |
| templates | Planned | Placeholder |
| reference | In progress | Placeholder |
```

- [ ] **Step 3: Write maintenance guide**

Create `docs/repo/maintenance-guide.md` with this content:

```markdown
# Repository Maintenance Guide

This file records temporary buildout rules for the handbook.

## Content Boundaries

- `zh/` and `en/` are reader-facing handbook content.
- `docs/repo/` is temporary repo working material.
- `docs/superpowers/` stores design specs and implementation plans.
- `.superpowers/` is local brainstorm tooling output and must stay untracked.

## Adding New Handbook Content

When adding a new reader-facing document:

1. Put the Chinese source under the matching `zh/` section.
2. Add or update the matching English placeholder under `en/`.
3. Link the document from `README.zh.md` if it is part of the main reading path.
4. Update `docs/repo/translation-status.md`.

## Commit Style

Use Conventional Commits.

Examples:

- `docs: add project level guide`
- `docs: add loop readiness checklist`
- `docs: add skill workflow examples`
- `chore: update translation status`
```

- [ ] **Step 4: Write first decision record**

Create `docs/repo/decisions/0001-handbook-information-architecture.md` with this content:

```markdown
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
```

- [ ] **Step 5: Verify repo records exist**

Run:

```bash
test -f docs/repo/translation-status.md && test -f docs/repo/maintenance-guide.md && test -f docs/repo/decisions/0001-handbook-information-architecture.md
```

Expected: command exits with status 0.

- [ ] **Step 6: Commit repo working records**

Run:

```bash
git add docs/repo
git commit -m "docs: record handbook maintenance decisions"
```

Expected: commit succeeds.

## Task 7: Final Verification

**Files:**
- Verify all files created or moved by Tasks 1-6.

- [ ] **Step 1: Check no local brainstorm artifacts are tracked**

Run:

```bash
git status --short
```

Expected: `.superpowers/` does not appear.

- [ ] **Step 2: Check required files exist**

Run:

```bash
for f in README.md README.zh.md zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md zh/reference/ai-development-workflow-best-practices.md docs/repo/translation-status.md docs/repo/maintenance-guide.md docs/repo/decisions/0001-handbook-information-architecture.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 3: Check there are no stale placeholder markers in Chinese guides**

Run:

```bash
rg -n "PLACEHOLDER|未完成|缺失内容" README.zh.md zh/guides docs/repo || true
```

Expected: no output.

- [ ] **Step 4: Check root README language strategy is explicit**

Run:

```bash
rg -n "Chinese is currently the source of truth|README.zh.md" README.md
```

Expected: matches for both phrases.

- [ ] **Step 5: Review commit history format**

Run:

```bash
git log --oneline -8
```

Expected: new commits use Conventional Commits, such as `docs: ...` or `chore: ...`.
