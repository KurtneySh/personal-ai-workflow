# Cookbook Review And Fix Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a `zh/cookbook/review-and-fix.md` scenario for handling concrete test, lint, review, and CI feedback with Claude Code or Codex CLI.

**Architecture:** Create one scenario page that follows the existing cookbook structure, with a copyable feedback template, Claude Code and Codex CLI prompts, verification, stop/escalate rules, loop boundary guidance, skill capture signals, and related links. Then update the cookbook entrypoint so the new scenario is listed as current rather than planned.

**Tech Stack:** Markdown documentation, Git, ripgrep validation, Conventional Commits.

---

## File Structure

Create or modify these files:

```text
zh/cookbook/review-and-fix.md
zh/cookbook/README.md
```

Do not modify English stub files or create the `zh/cases/` module in this plan.

## Task 1: Review And Fix Scenario Page

**Files:**
- Create: `zh/cookbook/review-and-fix.md`

- [ ] **Step 1: Create the scenario page**

Create `zh/cookbook/review-and-fix.md` with this content:

````markdown
# Review And Fix

这个场景用于把具体的 test、lint、typecheck、review 或 CI 反馈交给 Claude Code 或 Codex CLI 处理。

目标不是让 agent 看到失败就自动循环修复，而是先理解反馈、判断原因、提出或执行一个边界清楚的修复，并用真实信号验证结果。

## 用途 / Purpose

这个场景帮助人类把具体反馈整理成 agent 能处理的修复任务。

完成后，agent 应该说明反馈是什么、可能原因是什么、改了哪些文件、验证结果如何，以及是否还有残余风险。

## 适用场景 / When to Use

适合：

- test 失败。
- lint 或 typecheck 报错。
- review comment 指出具体问题。
- CI 失败并有可读取的错误日志。
- 文档 review note 明确指出需要修正的地方。

## 不适用场景 / When Not to Use

不适合：

- 反馈只是“感觉不对”或偏好不明确。
- 需要重新判断产品方向或架构方向。
- 需要自动 loop，但还没有设计 loop 边界。
- 需要扩大修改范围、改数据模型、改权限、部署或触碰生产风险。
- 涉及 Level 4 风险但没有人类确认门。

## 输入材料 / Inputs

至少提供：

- 反馈来源。
- 完整失败日志、review comment 或 CI 摘要。
- 项目级别。
- 可能涉及的文件或目录。
- 允许动作。
- 禁止动作。
- 验证命令或验证信号。
- 最大修复范围。
- 人类确认门。

只写“修一下反馈”不够。agent 需要证据、范围和验证方式。

## Feedback Fix Template

复制下面模板到 issue、review 回复、`STATE.md` 或对话里。

```markdown
## Feedback Fix

- Feedback source:
- Feedback text or log:
- Project level:
- Likely files or areas:
- Allowed actions:
- Forbidden actions:
- Verification command or signal:
- Maximum fix scope:
- Human confirmation gates:
- Notes or hypotheses:
```

填写要求：

- `Feedback source` 写清来自 test、lint、typecheck、review、CI 还是文档 review。
- `Feedback text or log` 尽量粘贴原始失败信息，不要只写概括。
- `Allowed actions` 必须足够小，不能让 agent 自行扩大范围。
- `Forbidden actions` 必须写清生产、权限、认证、支付、敏感数据、安全、部署、migration 和外部服务边界。
- `Verification command or signal` 必须是真实项目信号；没有就写“当前没有真实验证信号”。
- `Maximum fix scope` 写清最多允许改哪些文件、目录或行为。

## Claude Code Prompt

```text
请根据下面的 Feedback Fix 处理具体反馈。

要求：
- 先阅读反馈内容和相关项目上下文。
- 先复述失败或 review 要求是什么。
- 判断反馈类型：test / lint / typecheck / review / CI / docs。
- 在修改前说明你判断的可能原因和最小修复方案。
- 只做 Feedback Fix 中允许的动作。
- 不要做未授权的大范围重构。
- 不要编造不存在的命令、测试、部署流程、团队规则或风险假设。
- 如果需要安装依赖、部署、migrate、改变权限、访问外部服务，或触碰认证、支付、生产、敏感数据、安全、合规，请停止并要求人类确认。
- 修复后运行或报告真实验证信号。
- 如果无法运行验证，请说明原因。
- 最后输出修改文件、修复说明、验证结果、是否解决原反馈、残余风险和需要人类确认的问题。
- 不要自动进入 loop；如果需要多轮修复，先说明是否满足 loop 条件。

Feedback Fix:
<粘贴 Feedback Fix 模板内容>
```

## Codex CLI Prompt

```text
请在当前 repo 中根据下面的 Feedback Fix 处理具体反馈。

要求：
- 先读取反馈、相关文件和必要的项目上下文。
- 修改前复述原始失败、review 要求或 CI 问题。
- 判断反馈类型，并说明可能原因。
- 只实施最小必要修复。
- 不要扩大修改范围。
- 不要编造命令、测试、部署流程、团队规则或风险假设。
- 不要运行部署、migrate、权限、认证、支付、生产或敏感数据相关命令，除非 Feedback Fix 明确批准。
- 修复后运行同一个失败命令或 handoff 中指定的验证命令。
- 如果验证失败，请区分：同一失败重复、新错误、无法运行验证。
- 输出变更文件、验证结果、是否解决原反馈、残余风险和下一步建议。
- 不要自动 loop；如果你认为需要 loop，请只输出 loop 判断和需要人类确认的边界。

Feedback Fix:
<粘贴 Feedback Fix 模板内容>
```

## 期望输出 / Expected Output

agent 完成后应该输出：

- 反馈类型。
- 原始失败或 review 要求摘要。
- 可能原因。
- 修改了哪些文件。
- 修复策略。
- 运行了哪些验证命令。
- 验证结果。
- 原反馈是否已解决。
- 残余风险。
- 需要人类确认的问题。

## 验证 / Verification

优先验证原反馈：

- 重新运行同一个失败命令，或运行 handoff 中指定的验证命令。
- 检查原始 review comment 是否已被回应。
- 运行 `git diff --check`。
- 检查变更文件是否超出允许范围。
- 如果验证无法运行，明确记录原因。

验证结果要区分：

- verification passed。
- verification failed with a new error。
- verification could not be run。
- same failure repeated。

常用检查：

```bash
git status --short
git diff --check
```

Expected:

- `git status --short` 只显示预期文件。
- `git diff --check` 无输出。

## 停止与升级 / Stop And Escalate

遇到以下情况，agent 必须停止并交回人类：

- 反馈模糊或只是主观偏好。
- 修复需要改变产品方向或架构方向。
- 同一失败在一次边界清楚的修复后重复出现。
- 没有真实验证信号。
- 修复需要超出允许范围。
- 需要安装依赖、部署、migrate、改变权限或访问外部服务。
- 涉及生产、权限、认证、支付、敏感数据、安全或合规。
- 文档、测试、review 或 CI 反馈互相矛盾。
- agent 无法解释原因或下一步。

## 什么时候转成 loop / When This Becomes A Loop

默认不要从 loop 开始。

一次性修复足够的情况：

- 反馈具体。
- 允许动作清楚。
- 有一个真实验证命令可以确认结果。
- 修复范围小。

只有同时写清以下内容，才考虑开发反馈 loop：

- 目标。
- 允许动作。
- 禁止动作。
- 验证信号。
- 停止条件。
- 升级条件。
- 最大轮数。

设计 loop 前先阅读 [Loops](../loops/README.md)。如果任何 loop 边界答不清楚，就继续按人类确认的单步修复推进。

## Skill Capture Signals

如果多次出现以下情况，可以考虑沉淀为项目级 skill：

- 同一类反馈反复出现。
- 修复流程有稳定输入和验证方式。
- 项目有固定 review、CI 或 lint 约定。
- agent 反复需要同一套 stop/escalate 规则。
- 某类 feedback handling 模式跨多个任务有效。

项目级 skill 优先。只有当这个模式跨多个项目也稳定有效时，才记录为系统级 skill 候选，并且必须经过人类 review。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
- [Task Handoff](task-handoff.md)
````

- [ ] **Step 2: Verify required sections**

Run:

```bash
rg -n "Review And Fix|用途 / Purpose|适用场景 / When to Use|不适用场景 / When Not to Use|输入材料 / Inputs|Feedback Fix Template|Claude Code Prompt|Codex CLI Prompt|期望输出 / Expected Output|验证 / Verification|停止与升级 / Stop And Escalate|什么时候转成 loop / When This Becomes A Loop|Skill Capture Signals|相关文档 / Related Docs" zh/cookbook/review-and-fix.md
```

Expected: matches for all required sections.

- [ ] **Step 3: Verify template fields and safety language**

Run:

```bash
rg -n "Feedback source|Feedback text or log|Project level|Allowed actions|Forbidden actions|Verification command or signal|Maximum fix scope|Human confirmation gates|不要编造|不要自动 loop|真实验证信号|不能自动升级|系统级 skill 候选" zh/cookbook/review-and-fix.md
```

Expected: matches for template fields, no-invention guidance, no automatic loop guidance, verification signal, and skill boundary.

- [ ] **Step 4: Verify related docs exist**

Run:

```bash
for f in zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md zh/playbooks/start-a-new-project.md zh/loops/README.md zh/skill-workflow/README.md zh/cookbook/task-handoff.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 5: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/cookbook/review-and-fix.md
```

Expected: no output.

- [ ] **Step 6: Commit the scenario page**

Run:

```bash
git add zh/cookbook/review-and-fix.md
git commit -m "docs: add review and fix cookbook"
```

Expected: commit succeeds.

## Task 2: Cookbook Entrypoint Update

**Files:**
- Modify: `zh/cookbook/README.md`

- [ ] **Step 1: Add Review And Fix to current scenarios**

Find this block:

```markdown
- [Repo Orientation](repo-orientation.md)：让 agent 在修改文件前读懂一个已有仓库。
- [Task Handoff](task-handoff.md)：把一个边界清楚的任务交给 Claude Code 或 Codex CLI。
```

Replace it with:

```markdown
- [Repo Orientation](repo-orientation.md)：让 agent 在修改文件前读懂一个已有仓库。
- [Task Handoff](task-handoff.md)：把一个边界清楚的任务交给 Claude Code 或 Codex CLI。
- [Review And Fix](review-and-fix.md)：处理具体 test、lint、review 或 CI 反馈。
```

- [ ] **Step 2: Update planned scenarios**

Find this planned scenarios block:

```markdown
后续可以继续补充：

- review / test / lint feedback 修复。
- 长会话或中断后的 context refresh。
- PR / CI 工作流。
- multi-agent delegation。
```

Replace it with:

```markdown
后续可以继续补充：

- 长会话或中断后的 context refresh。
- 更完整的 PR / CI 工作流。
- multi-agent delegation。
```

- [ ] **Step 3: Verify entrypoint links and scope**

Run:

```bash
rg -n "Review And Fix|review-and-fix.md|更完整的 PR / CI 工作流|review / test / lint feedback 修复" zh/cookbook/README.md
```

Expected: matches `Review And Fix`, `review-and-fix.md`, and `更完整的 PR / CI 工作流`; no match for the old `review / test / lint feedback 修复` planned bullet.

- [ ] **Step 4: Verify related scenario files exist**

Run:

```bash
for f in zh/cookbook/repo-orientation.md zh/cookbook/task-handoff.md zh/cookbook/review-and-fix.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 5: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/cookbook/README.md
```

Expected: no output.

- [ ] **Step 6: Commit entrypoint update**

Run:

```bash
git add zh/cookbook/README.md
git commit -m "docs: link review and fix cookbook"
```

Expected: commit succeeds.

## Task 3: Final Acceptance Verification

**Files:**
- Verify: `zh/cookbook/review-and-fix.md`
- Verify: `zh/cookbook/README.md`

- [ ] **Step 1: Verify required files exist**

Run:

```bash
for f in zh/cookbook/README.md zh/cookbook/review-and-fix.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 2: Verify scenario structure**

Run:

```bash
rg -n "用途 / Purpose|适用场景 / When to Use|不适用场景 / When Not to Use|输入材料 / Inputs|Feedback Fix Template|Claude Code Prompt|Codex CLI Prompt|期望输出 / Expected Output|验证 / Verification|停止与升级 / Stop And Escalate|什么时候转成 loop / When This Becomes A Loop|Skill Capture Signals|相关文档 / Related Docs" zh/cookbook/review-and-fix.md
```

Expected: matches for all required sections.

- [ ] **Step 3: Verify prompt behavior and loop boundary**

Run:

```bash
rg -n "先复述|可能原因|最小修复|不要编造|不要自动 loop|不要运行部署|verification passed|same failure repeated|设计 loop 前先阅读|人类 review" zh/cookbook/review-and-fix.md
```

Expected: matches for diagnosis before editing, no-invention rules, no automatic loop, verification states, loop boundary, and human-reviewed skill promotion.

- [ ] **Step 4: Verify cookbook README update**

Run:

```bash
rg -n "Review And Fix|review-and-fix.md|更完整的 PR / CI 工作流" zh/cookbook/README.md
```

Expected: README lists Review And Fix as current and PR/CI as a later deeper workflow.

- [ ] **Step 5: Verify old planned bullet is gone**

Run:

```bash
rg -n "review / test / lint feedback 修复" zh/cookbook/README.md || true
```

Expected: no output.

- [ ] **Step 6: Verify English files unchanged**

Run:

```bash
git status --short en
```

Expected: no output.

- [ ] **Step 7: Verify no unresolved drafting markers**

Run:

```bash
rg -n "TBD|TODO|待定|正文不完整|草稿未清理|临时文本" zh/cookbook/review-and-fix.md zh/cookbook/README.md || true
```

Expected: no output.

- [ ] **Step 8: Run final Markdown whitespace check**

Run:

```bash
git diff --check
```

Expected: no output.

- [ ] **Step 9: Review recent commits**

Run:

```bash
git log --oneline -8
```

Expected: review-and-fix commits use Conventional Commits messages beginning with `docs:`.
