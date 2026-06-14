# Cookbook PR CI Workflow Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the Chinese cookbook `PR CI Workflow` scenario and link it from the cookbook entrypoint.

**Architecture:** This is a pure documentation change. The new page `zh/cookbook/pr-ci-workflow.md` owns the scenario content; `zh/cookbook/README.md` only lists it as a current scenario and removes it from planned scenarios.

**Tech Stack:** Markdown documentation, existing `zh/cookbook/` structure, git verification commands.

---

## File Structure

- Create: `zh/cookbook/pr-ci-workflow.md`
  - Responsibility: explain how to use Claude Code or Codex CLI across PR readiness, PR summary, CI triage, review comment handling, context refresh, and pre-merge checks.
  - Must follow the scenario structure used by `repo-orientation.md`, `task-handoff.md`, `review-and-fix.md`, and `context-refresh.md`.
- Modify: `zh/cookbook/README.md`
  - Responsibility: list PR CI Workflow under current scenarios and keep only remaining future scenarios under planned scenarios.
- Do not modify: English placeholder files, `zh/cases/`, scripts, or unrelated docs.

## Acceptance Checklist

- `zh/cookbook/pr-ci-workflow.md` exists.
- The page includes Purpose, When to Use, When Not to Use, Inputs, PR CI Workflow Template, Workflow Stages, Claude Code Prompt, Codex CLI Prompt, Expected Output, Verification, Stop and Escalate, When This Becomes A Loop, Skill Capture Signals, and Related Docs.
- The page includes a copyable PR CI Workflow Template.
- The page includes staged lifecycle guidance.
- Prompts distinguish read-only PR/CI triage from authorized local fixes.
- Prompts forbid invented commands, tests, CI checks, review comments, deployment flows, team rules, and risk assumptions.
- Prompts do not ask agents to push, update PR metadata, merge, rerun remote checks, deploy, migrate, change permissions, or access external services without explicit authorization.
- Prompts do not ask agents to run automatic loops.
- The page distinguishes itself from repo orientation, task handoff, review-and-fix, context refresh, and loops.
- `zh/cookbook/README.md` links PR CI Workflow as a current scenario.
- The planned scenarios list no longer says PR / CI workflow is future work.
- Related links point to existing files.
- `git diff --check` passes.

---

### Task 1: Add PR CI Workflow Scenario Page

**Files:**
- Create: `zh/cookbook/pr-ci-workflow.md`
- Reference: `docs/superpowers/specs/2026-06-13-cookbook-pr-ci-workflow-design.md`
- Reference: `zh/cookbook/repo-orientation.md`
- Reference: `zh/cookbook/task-handoff.md`
- Reference: `zh/cookbook/review-and-fix.md`
- Reference: `zh/cookbook/context-refresh.md`

- [ ] **Step 1: Read the scenario style and design spec**

Run:

```bash
sed -n '1,340p' docs/superpowers/specs/2026-06-13-cookbook-pr-ci-workflow-design.md
sed -n '1,220p' zh/cookbook/repo-orientation.md
sed -n '1,220p' zh/cookbook/task-handoff.md
sed -n '1,260p' zh/cookbook/review-and-fix.md
sed -n '1,300p' zh/cookbook/context-refresh.md
```

Expected:

```text
The spec and existing scenario pages are readable. Existing pages use bilingual section headings, Chinese explanatory text, copyable templates, prompts, verification, stop/escalate, and skill capture sections.
```

- [ ] **Step 2: Create `zh/cookbook/pr-ci-workflow.md`**

Write this complete file:

````markdown
# PR CI Workflow

这个场景用于在 PR 和 CI 生命周期里组织 Claude Code 或 Codex CLI 的协作。

目标不是把 agent 变成 GitHub 或 CI 平台的自动驾驶，也不是让它看到失败就无限修，而是让它在准备 PR、读取 CI、处理 review、更新状态和合并前检查时，始终基于真实证据、明确权限和可验证结果行动。

## 用途 / Purpose

这个场景帮助人类把 PR / CI 工作拆成可交给 agent 的判断和执行边界。

完成后，agent 应该知道当前 PR 或 branch 处于什么状态、下一步应该是准备 summary、等待 CI、处理反馈、刷新上下文、停止还是交回人类。

## 适用场景 / When to Use

适合：

- 准备把一个 branch 发成 PR。
- 需要 agent 草拟或更新 PR summary、test plan、risk notes。
- 检查 branch 是否可以 push。
- 读取 CI 状态或失败日志。
- 把 CI failure 转成边界清楚的反馈修复任务。
- 把 review comment 转成边界清楚的反馈修复任务。
- 判断 PR 是否可以进入人工 review 或 merge 前检查。
- PR 工作被中断、换 agent 或上下文过期。

## 不适用场景 / When Not to Use

不适合：

- 第一次理解陌生 repo；这种情况先用 [Repo Orientation](repo-orientation.md)。
- 执行具体实现任务；这种情况用 [Task Handoff](task-handoff.md)。
- 处理明确 test、lint、typecheck、review 或 CI 反馈；这种情况用 [Review And Fix](review-and-fix.md)。
- 长会话中断后不确定当前状态；这种情况先用 [Context Refresh](context-refresh.md)。
- 你想让 agent 自动循环修 CI，但还没有设计 loop 边界。
- 需要 push、更新 PR、merge、rerun remote checks、部署、migrate、改权限或访问外部服务，但没有明确授权。
- 涉及 Level 4 风险但没有人类确认门。

这个场景不是 GitHub、GitLab、CI provider 或 `gh` 命令大全。

## 输入材料 / Inputs

至少提供：

- 项目级别。
- branch 或 PR 标识。
- target branch。
- PR 目标或改动摘要。
- 已改文件或目录。
- required checks 或验证命令。
- 当前 CI 状态或日志。
- review comments。
- 允许动作。
- 禁止动作。
- push 权限。
- PR update 权限。
- merge 权限。
- 人类确认门。
- 状态记录位置。

如果缺少 branch state、diff scope 或 verification signal，PR / CI 任务就是不完整的。agent 必须标记未知信息，不能编造。

## PR CI Workflow Template

复制下面模板到 `STATE.md`、issue、PR comment、任务说明或对话里。

```markdown
## PR CI Workflow

- Workflow goal:
- Project level:
- Branch or PR:
- Target branch:
- Change summary:
- Changed files or areas:
- Required checks:
- Current CI status or logs:
- Review comments:
- Allowed actions:
- Forbidden actions:
- Push permission:
- PR update permission:
- Merge permission:
- Human confirmation gates:
- State record location:
- Open questions:
```

填写要求：

- `Workflow goal` 写清本次是准备 PR、读取 CI、处理 review、更新 PR 还是 merge 前检查。
- `Branch or PR` 和 `Target branch` 用来确认 agent 没有在错误分支工作。
- `Required checks` 必须来自真实项目；没有就写“当前没有真实 required checks”。
- `Current CI status or logs` 不要只写“CI 挂了”，尽量提供具体失败 check 和日志。
- `Allowed actions` 和 `Forbidden actions` 必须区分只读检查、本地修改、push、PR update、merge 和远程操作。
- `Push permission`、`PR update permission`、`Merge permission` 默认应为 no，除非人类明确授权。

## Workflow Stages

### 1. Pre-PR readiness

确认当前 branch、dirty worktree、diff scope、最近提交、验证命令和残余风险。

常见输出：

- 是否可以准备 PR summary。
- 是否还有本地未提交改动。
- 是否缺少验证信号。
- 是否需要先做 [Context Refresh](context-refresh.md)。

### 2. PR summary and test plan

让 agent 只基于真实 diff、commit、测试结果和用户提供的上下文输出：

- summary。
- test plan。
- risk notes。
- reviewer notes。
- known limitations。

不要让 agent 编造未运行的测试或不存在的 CI checks。

### 3. CI status triage

把 CI 状态归类为：

- passing。
- failing with logs。
- pending。
- unavailable。
- unknown。

如果无法读取远程 CI 状态，agent 必须明确说明，而不是假设 CI 通过或失败。

### 4. CI failure handling

如果失败日志具体，把它转成 [Review And Fix](review-and-fix.md) 的 `Feedback Fix Template`。

不要直接进入无限修复。一次修复后如果同一失败重复出现，应停止并交回人类或重新判断 loop 边界。

### 5. Review comment handling

把明确、可执行的 review comment 转成边界清楚的 feedback fix。

如果 comment 是主观偏好、架构方向、产品判断或互相矛盾的意见，先要求人类澄清，不要让 agent 自行决定方向。

### 6. Context refresh

遇到以下情况，先用 [Context Refresh](context-refresh.md)：

- PR 工作中断。
- agent 切换。
- branch、commit、CI、review 状态不清楚。
- local changes 或 recent commits 的归属不清楚。

### 7. Pre-merge check

合并前必须确认：

- 当前 branch 和 target branch 正确。
- required checks 状态明确。
- review comments 已处理或明确保留。
- 本地 diff 和 PR summary 一致。
- merge 权限由人类明确授权。
- 没有未确认的生产、权限、认证、支付、敏感数据、安全、合规、migration 或部署风险。

## Claude Code Prompt

```text
请根据下面的 PR CI Workflow 做一次 PR / CI 工作流判断。

要求：
- 先读取提供的 branch、PR、state record、README、STATE、SPEC、AGENTS、CI 配置、review comments 或日志。
- 检查当前 branch 和工作区状态：git status --short --branch。
- 检查最近提交：git log --oneline -5。
- 检查本地 diff scope；如果存在 local changes，只基于 diff 和文件读取判断，不要擅自修改。
- 如果可以读取 PR description、CI status 或 review comments，请只基于真实内容总结。
- 判断当前阶段：pre-PR readiness / PR summary / CI triage / CI failure handling / review comment handling / context refresh / pre-merge check。
- 判断建议下一步属于：prepare PR summary / push after verification / wait for CI / handle feedback / refresh context / stop / ask human。
- 如果需要修复具体 CI、test、lint、typecheck 或 review 反馈，请输出可交给 Review And Fix 的 Feedback Fix，不要直接无限修。
- 只基于真实存在的文件、命令、commit、CI 状态、review comments 和用户提供的上下文。
- 不要编造命令、测试、CI checks、review comments、部署流程、团队规则或风险假设。
- 未经明确授权，不要 push、更新 PR metadata、merge、rerun remote checks、部署、migrate、改变权限或访问外部服务。
- 不要触碰生产、权限、认证、支付、敏感数据、安全或合规边界，除非 workflow 明确授权且有人类确认门。
- 不要自动进入 loop；如果你认为需要 loop，只输出 loop 判断和需要人类确认的边界。

请输出：
- 当前 branch / PR 状态
- changed files 或 changed areas
- 当前 workflow stage
- verification commands 或 required checks
- CI 状态分类
- review 状态分类
- 建议下一步
- 如果需要，给出 PR summary / test plan / risk notes
- 如果需要，给出 Feedback Fix 草案
- 风险、未知信息和需要人类确认的问题

PR CI Workflow:
<粘贴 PR CI Workflow 模板内容>
```

## Codex CLI Prompt

```text
请在当前 repo 中根据下面的 PR CI Workflow 做一次 PR / CI 工作流判断。

要求：
- 先读取 workflow 中列出的 state record、branch、PR 信息、CI 日志、review comments 和必要项目上下文。
- 检查当前 branch 和工作区状态：git status --short --branch。
- 检查最近提交：git log --oneline -5。
- 检查本地 diff scope；如果有 local changes，不要擅自修改。
- 如果无法读取远程 PR 或 CI 状态，请明确说明 remote state unavailable，不要假设。
- 判断当前阶段：pre-PR readiness / PR summary / CI triage / CI failure handling / review comment handling / context refresh / pre-merge check。
- 判断下一步：prepare PR summary / push after verification / wait for CI / handle feedback / refresh context / stop / ask human。
- 如果需要处理具体反馈，请把它整理成 Review And Fix 的输入，而不是直接自动修复。
- 只基于真实存在的文件、命令、commit、CI 状态、review comments 和用户提供的上下文。
- 不要编造命令、测试、CI checks、review comments、部署流程、团队规则或风险假设。
- 未经明确授权，不要 push、更新 PR metadata、merge、rerun remote checks、部署、migrate、改变权限或访问外部服务。
- 不要自动 loop；如果需要 loop，请只输出 loop 边界和需要人类确认的问题。

请输出：
- branch / PR 状态
- changed files 或 changed areas
- workflow stage
- verification commands 或 required checks
- CI 状态分类
- review 状态分类
- 建议下一步
- PR summary / test plan / risk notes，如果本次目标需要
- Feedback Fix 草案，如果发现具体反馈
- 风险、未知信息和需要人类确认的问题

PR CI Workflow:
<粘贴 PR CI Workflow 模板内容>
```

## 期望输出 / Expected Output

agent 完成后应该输出：

- branch 和 worktree 状态。
- PR 或 branch 目标。
- changed files 或 changed areas。
- verification commands 或 required checks。
- CI 状态分类。
- review 状态分类。
- 建议下一步。
- 本次需要时的 PR summary、test plan 和 risk notes。
- 应转成 [Review And Fix](review-and-fix.md) 的反馈项。
- 风险和未知信息。
- push、PR update、merge、部署、migration 或外部服务访问前的人类确认门。

## 验证 / Verification

本地验证可以包括：

```bash
git status --short --branch
git diff --check
git log --oneline -5
```

项目验证命令必须来自 README、package scripts、CI config、任务 handoff 或人类提供的上下文。

PR / CI 验证需要确认：

- required checks 是 passing、failing、pending、unavailable 还是 unknown。
- 使用 CI 日志时引用了具体失败信号。
- review comments 被准确总结，没有忽略 unresolved comments。
- PR summary 和实际 diff 一致。
- push 或 merge 权限是明确的。
- 无法读取远程 PR / CI 状态时，输出明确说明。

## 停止与升级 / Stop And Escalate

遇到以下情况，agent 必须停止并交回人类：

- branch 或 PR 身份不清楚。
- target branch 不清楚，但下一步依赖它。
- working tree 有非预期 local changes。
- 无法读取 CI 状态，但下一步依赖它。
- failure logs 缺失、截断或含糊。
- review comments 是主观偏好、架构方向、产品判断或互相矛盾。
- required checks 失败，但没有边界清楚的修复范围。
- 需要 push、更新 PR metadata、merge、rerun remote checks、部署、migrate、改变权限或访问外部服务，但没有明确授权。
- 改动涉及生产、权限、认证、支付、敏感数据、安全、合规、migration 或基础设施。
- 项目可能是 Level 4，但风险边界没有确认。
- agent 无法基于证据解释建议下一步。

## 什么时候转成 loop / When This Becomes A Loop

默认不要把 PR / CI 工作做成自动 loop。

一次性 PR / CI triage 足够的情况：

- branch state 清楚。
- CI status 清楚。
- feedback 具体。
- verification command 真实存在。
- push、PR update、merge 权限明确。

只有同时写清以下内容，才考虑 CI-fix loop：

- 目标。
- 允许动作。
- 禁止动作。
- 验证信号。
- 停止条件。
- 升级条件。
- 远程操作权限。
- 最大轮数。

review-fix loop 只适合具体、可执行、范围有限的 review comments。

设计 loop 前先阅读 [Loops](../loops/README.md)。如果任何 loop 边界答不清楚，就继续按人类确认的单步推进。

## Skill Capture Signals

如果多次出现以下情况，可以考虑沉淀为项目级 skill：

- 项目有稳定 PR template。
- CI checks 和验证命令稳定。
- review comment handling 有重复模式。
- push、PR update、merge 或 stop/escalate gate 反复出现。
- agent 总是需要同一套 PR summary 和 test plan 格式。
- 这个 workflow 在多个 PR 中有效。

项目级 skill 优先。只有当这个 PR / CI workflow 跨多个项目也稳定有效时，才记录为系统级 skill 候选，并且必须经过人类 review。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
- [Repo Orientation](repo-orientation.md)
- [Task Handoff](task-handoff.md)
- [Review And Fix](review-and-fix.md)
- [Context Refresh](context-refresh.md)
````

- [ ] **Step 3: Verify required sections exist**

Run:

```bash
rg -n "^## (用途 / Purpose|适用场景 / When to Use|不适用场景 / When Not to Use|输入材料 / Inputs|PR CI Workflow Template|Workflow Stages|Claude Code Prompt|Codex CLI Prompt|期望输出 / Expected Output|验证 / Verification|停止与升级 / Stop And Escalate|什么时候转成 loop / When This Becomes A Loop|Skill Capture Signals|相关文档 / Related Docs)" zh/cookbook/pr-ci-workflow.md
```

Expected:

```text
All fourteen required section headings are listed.
```

- [ ] **Step 4: Verify prompt safety boundaries**

Run:

```bash
rg -n "不要编造|未经明确授权|不要 push|不要自动|Review And Fix|Context Refresh|git status --short --branch|git log --oneline -5" zh/cookbook/pr-ci-workflow.md
```

Expected:

```text
The output includes matches for anti-invention, authorization gates, no automatic loops, links to related scenarios, and the two git inspection commands.
```

- [ ] **Step 5: Commit the scenario page**

Run:

```bash
git diff --check
git add zh/cookbook/pr-ci-workflow.md
git commit -m "docs: add pr ci workflow cookbook"
```

Expected:

```text
git diff --check has no output.
Commit succeeds with message "docs: add pr ci workflow cookbook".
```

---

### Task 2: Link PR CI Workflow From Cookbook Entrypoint

**Files:**
- Modify: `zh/cookbook/README.md`
- Reference: `zh/cookbook/pr-ci-workflow.md`

- [ ] **Step 1: Read the cookbook entrypoint**

Run:

```bash
sed -n '1,180p' zh/cookbook/README.md
```

Expected:

```text
The current scenarios list contains Repo Orientation, Task Handoff, Review And Fix, and Context Refresh. Planned scenarios still mention fuller PR / CI workflow.
```

- [ ] **Step 2: Update current and planned scenarios**

Change the scenario lists in `zh/cookbook/README.md` to this exact content:

```markdown
## 当前场景 / Current Scenarios

- [Repo Orientation](repo-orientation.md)：让 agent 在修改文件前读懂一个已有仓库。
- [Task Handoff](task-handoff.md)：把一个边界清楚的任务交给 Claude Code 或 Codex CLI。
- [Review And Fix](review-and-fix.md)：处理具体 test、lint、review 或 CI 反馈。
- [Context Refresh](context-refresh.md)：在长会话、中断、agent 切换或上下文压缩之后恢复当前项目状态。
- [PR CI Workflow](pr-ci-workflow.md)：在 PR 准备、CI 读取、review 处理和合并前检查中组织 agent 协作。

## 后续场景 / Planned Scenarios

后续可以继续补充：

- multi-agent delegation。

这些场景先不创建文件，等真实使用后再补。
```

- [ ] **Step 3: Verify README links and planned list**

Run:

```bash
rg -n "PR CI Workflow|pr-ci-workflow|更完整的 PR / CI 工作流|multi-agent" zh/cookbook/README.md
test -f zh/cookbook/pr-ci-workflow.md
```

Expected:

```text
README contains PR CI Workflow and pr-ci-workflow.md.
README does not contain "更完整的 PR / CI 工作流".
multi-agent remains in planned scenarios.
pr-ci-workflow.md exists.
```

- [ ] **Step 4: Verify related links exist**

Run:

```bash
test -f zh/guides/project-levels.md
test -f zh/guides/infra-by-level.md
test -f zh/guides/infra-checklists.md
test -f zh/playbooks/start-a-new-project.md
test -f zh/loops/README.md
test -f zh/skill-workflow/README.md
test -f zh/cookbook/repo-orientation.md
test -f zh/cookbook/task-handoff.md
test -f zh/cookbook/review-and-fix.md
test -f zh/cookbook/context-refresh.md
test -f zh/cookbook/pr-ci-workflow.md
```

Expected:

```text
All commands exit 0.
```

- [ ] **Step 5: Commit the entrypoint update**

Run:

```bash
git diff --check
git add zh/cookbook/README.md
git commit -m "docs: link pr ci workflow cookbook"
```

Expected:

```text
git diff --check has no output.
Commit succeeds with message "docs: link pr ci workflow cookbook".
```

---

### Final Verification

- [ ] **Step 1: Verify no placeholders or plan leakage**

Run:

```bash
rg -n "TBD|TODO|待定|占位|implement later|fill in" zh/cookbook/pr-ci-workflow.md zh/cookbook/README.md
```

Expected:

```text
No output.
```

- [ ] **Step 2: Verify markdown whitespace**

Run:

```bash
git diff --check HEAD~2..HEAD
```

Expected:

```text
No output.
```

- [ ] **Step 3: Verify git status**

Run:

```bash
git status --short --branch
```

Expected:

```text
Only expected branch-ahead status or clean status remains. There are no uncommitted changes.
```

- [ ] **Step 4: Review final diff summary**

Run:

```bash
git show --stat --oneline HEAD~1..HEAD
git show --stat --oneline HEAD~2..HEAD
```

Expected:

```text
The last two implementation commits only touch zh/cookbook/pr-ci-workflow.md and zh/cookbook/README.md.
```
