# Cookbook Context Refresh Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the Chinese cookbook `Context Refresh` scenario and link it from the cookbook entrypoint.

**Architecture:** This is a pure documentation change. The new page `zh/cookbook/context-refresh.md` owns the scenario content; `zh/cookbook/README.md` only lists it as a current scenario and removes it from planned scenarios.

**Tech Stack:** Markdown documentation, existing `zh/cookbook/` structure, git verification commands.

---

## File Structure

- Create: `zh/cookbook/context-refresh.md`
  - Responsibility: explain when and how to recover working context after long sessions, interruptions, agent switches, or context compaction.
  - Must follow the scenario structure used by `repo-orientation.md`, `task-handoff.md`, and `review-and-fix.md`.
- Modify: `zh/cookbook/README.md`
  - Responsibility: list Context Refresh under current scenarios and keep only remaining future scenarios under planned scenarios.
- Do not modify: English placeholder files, `zh/cases/`, scripts, or unrelated docs.

## Acceptance Checklist

- `zh/cookbook/context-refresh.md` exists.
- The page includes Purpose, When to Use, When Not to Use, Inputs, Context Refresh Template, Claude Code Prompt, Codex CLI Prompt, Expected Output, Verification, Stop and Escalate, Relationship To Other Cookbook Scenarios, Loop Boundary, Skill Capture Signals, and Related Docs.
- The page includes a copyable Context Refresh Template.
- Prompts require read-only context recovery before any resume decision.
- Prompts forbid invented commands, tests, deployment flows, team rules, and risk assumptions.
- Prompts do not ask agents to continue implementation or run automatic loops.
- The page distinguishes context refresh from repo orientation, task handoff, review-and-fix, and loops.
- `zh/cookbook/README.md` links Context Refresh as a current scenario.
- The planned scenarios list no longer says context refresh is future work.
- Related links point to existing files.
- `git diff --check` passes.

---

### Task 1: Add Context Refresh Scenario Page

**Files:**
- Create: `zh/cookbook/context-refresh.md`
- Reference: `docs/superpowers/specs/2026-06-13-cookbook-context-refresh-design.md`
- Reference: `zh/cookbook/repo-orientation.md`
- Reference: `zh/cookbook/task-handoff.md`
- Reference: `zh/cookbook/review-and-fix.md`

- [ ] **Step 1: Read the scenario style and design spec**

Run:

```bash
sed -n '1,260p' docs/superpowers/specs/2026-06-13-cookbook-context-refresh-design.md
sed -n '1,220p' zh/cookbook/repo-orientation.md
sed -n '1,220p' zh/cookbook/task-handoff.md
sed -n '1,260p' zh/cookbook/review-and-fix.md
```

Expected:

```text
The spec and existing scenario pages are readable. Existing pages use bilingual section headings and Chinese explanatory text.
```

- [ ] **Step 2: Create `zh/cookbook/context-refresh.md`**

Write this complete file:

````markdown
# Context Refresh

这个场景用于在长会话、中断、agent 切换或上下文压缩之后，恢复当前项目状态。

目标不是让 agent 继续实现，也不是顺手修复问题，而是先把“上一轮目标是什么、现在 repo 到哪了、哪些事情已经完成、哪些信息可能过期、下一步是否安全”讲清楚。

## 用途 / Purpose

这个场景帮助人类要求 agent 做一次只读 context refresh，再决定是否继续任务、重新 handoff、处理反馈、停止或升级给人类。

完成后应该得到一份可验证的状态恢复摘要，而不是文件修改。

## 适用场景 / When to Use

适合：

- 长时间 AI coding session 被打断。
- 对话上下文被压缩或总结。
- 从一个 agent 切换到另一个 agent。
- 人类隔了一段时间回到 repo，想确认上一轮任务是否完成。
- 当前有 local changes 或 recent commits，但下一步不清楚。
- 需要判断应该 resume、verify、handoff、fix、stop 还是 ask human。

## 不适用场景 / When Not to Use

不适合：

- 你只是第一次进入陌生 repo；这种情况先用 [Repo Orientation](repo-orientation.md)。
- 你已经有边界清楚的新任务；这种情况用 [Task Handoff](task-handoff.md)。
- 你已经有明确 test、lint、review 或 CI 反馈；这种情况用 [Review And Fix](review-and-fix.md)。
- 你希望 agent 直接继续实现或自动修复。
- 你准备设计自动 loop，但还没有写清 loop 边界。
- 项目可能涉及 Level 4 风险，但还没有人类确认风险边界。

这个场景不给 agent 修改文件的权限。

## 输入材料 / Inputs

尽量提供：

- 上一轮目标。
- 上一次完成到哪一步。
- 预期 branch 或 workspace。
- 已知改动文件或目录。
- 上一次验证命令或验证结果。
- 状态记录位置，例如 `STATE.md`、issue、PR、plan 或对话摘要。
- 允许动作。
- 禁止动作。
- 人类确认门。
- 已知阻塞或担忧。

缺少输入也可以开始，但 agent 必须把缺失信息标记为未知，不能编造状态。

## Context Refresh Template

复制下面模板到 `STATE.md`、issue、PR comment、任务说明或对话里。

```markdown
## Context Refresh

- Last known goal:
- Last completed step:
- Expected branch or workspace:
- Known changed files or areas:
- Last verification command or result:
- State record location:
- Allowed actions:
- Forbidden actions:
- Human confirmation gates:
- Known blockers or concerns:
- Question to answer before resuming:
```

填写要求：

- `Last known goal` 写成可验证的结果，不要只写“继续做项目”。
- `Last completed step` 写最后一个确定完成的动作、commit、验证或 review 结果。
- `Expected branch or workspace` 用来发现 agent 是否在错误分支或错误 worktree。
- `Allowed actions` 默认应该是只读检查。
- `Forbidden actions` 必须写清不要修改文件、不要安装依赖、不要部署、不要 migrate、不要触碰生产、权限、认证、支付、敏感数据、安全和合规。
- `Question to answer before resuming` 写清本次 refresh 最重要的判断，例如“上一轮 cookbook 是否已经提交并 push？”。

## Claude Code Prompt

```text
请根据下面的 Context Refresh 做一次只读上下文恢复，不要修改文件。

要求：
- 先阅读提供的 state record，以及相关 README、STATE、SPEC、AGENTS、plan、issue、PR 或对话摘要。
- 检查当前分支和工作区状态：git status --short --branch。
- 检查最近提交：git log --oneline -5。
- 如果存在 local changes，只阅读 diff 或相关文件来判断状态，不要修改。
- 对照 Last known goal，判断哪些事情有证据表明已完成，哪些还未完成，哪些假设可能过期。
- 标记文档、commit、工作区状态或用户描述之间的矛盾。
- 判断建议下一步属于：resume / verify / create new handoff / handle feedback / stop / ask human。
- 只基于真实存在的文件、命令、commit 和用户提供的上下文。
- 不要编造不存在的命令、测试、部署流程、团队规则或风险假设。
- 不要修改文件，不要安装依赖，不要运行部署、migrate、权限、认证、支付、生产、敏感数据、安全、合规或外部服务相关命令。
- 不要继续实现；本次只输出 context refresh 结果和下一步建议。

请输出：
- 当前 repo 状态
- branch 和 dirty worktree 摘要
- 你理解的上一轮目标
- 已完成事项及证据
- 未完成事项及证据
- changed files 或 recent commits
- 过期假设、矛盾或未知信息
- 验证状态
- 建议下一步
- 需要人类确认的问题

Context Refresh:
<粘贴 context refresh 模板内容>
```

## Codex CLI Prompt

```text
请在当前 repo 中根据下面的 Context Refresh 做一次只读上下文恢复。

要求：
- 不要修改文件。
- 读取 state record、README、STATE、SPEC、AGENTS、plan、issue、PR 或对话摘要中存在且相关的内容。
- 运行或检查：git status --short --branch。
- 运行或检查：git log --oneline -5。
- 如果有 local changes，只用 git diff 或文件读取来理解状态，不要改动。
- 对照 Last known goal，判断已完成、未完成、风险、未知信息和可能过期的假设。
- 如果分支、worktree、state record、recent commits 或用户描述互相矛盾，请停止在只读分析层面，并列出需要人类确认的问题。
- 只基于真实存在的文件、命令、commit 和用户提供的上下文。
- 不要编造命令、测试、部署流程、团队规则或风险假设。
- 不要安装依赖，不要运行部署、migrate、权限、认证、支付、生产、敏感数据、安全、合规或外部服务相关命令。
- 不要继续实现或自动修复；本次只输出恢复结果和下一步建议。

请输出：
- 当前 repo 状态
- branch 和 dirty worktree 摘要
- 上一轮目标
- 已完成事项及证据
- 未完成事项及证据
- changed files 或 recent commits
- 过期假设、矛盾或未知信息
- 验证状态
- 建议下一步：resume / verify / create new handoff / handle feedback / stop / ask human
- 需要人类确认的问题

Context Refresh:
<粘贴 context refresh 模板内容>
```

## 期望输出 / Expected Output

agent 完成后应该输出：

- 当前 repo 状态。
- branch 和 dirty worktree 摘要。
- 它理解的上一轮目标。
- 已完成事项和证据。
- 未完成事项和证据。
- changed files 或 recent commits。
- 过期假设、矛盾或未知信息。
- 验证状态。
- 建议下一步。
- 需要人类确认的问题。

好的 context refresh 应该引用真实文件、commit、命令输出或用户提供的上下文。找不到的信息要标记为未知。

## 验证 / Verification

这个场景验证的是 context refresh 本身，而不是产品行为。

常用检查：

```bash
git status --short --branch
git log --oneline -5
```

如果存在 local changes，可以加：

```bash
git diff --check
```

检查 agent 输出：

- branch 状态是否正确。
- changed files 是否没有遗漏。
- recent commits 是否没有误读。
- 未知信息是否明确标为未知。
- 建议下一步是否有证据支撑。
- 是否没有修改文件。

## 停止与升级 / Stop And Escalate

遇到以下情况，agent 必须停止并交回人类：

- 当前 branch 或 workspace 和预期不一致。
- 存在 local changes，但无法判断是谁改的或是否允许继续。
- recent commits 和上一轮目标互相矛盾。
- state record、plan、README、issue、PR 或用户描述互相矛盾。
- 验证状态缺失或含糊。
- 下一步需要修改文件。
- 下一步需要安装依赖、部署、migrate、改变权限、访问外部服务。
- 涉及生产、权限、认证、支付、敏感数据、安全或合规。
- 项目可能是 Level 4，但风险边界没有确认。
- agent 无法基于证据解释当前状态。

## 和其他 Cookbook 场景的关系 / Relationship To Other Cookbook Scenarios

- [Repo Orientation](repo-orientation.md)：用于第一次理解 repo。Context Refresh 用于从上一轮目标或状态记录恢复当前工作状态。
- [Task Handoff](task-handoff.md)：用于执行边界清楚的新任务。Context Refresh 只判断是否需要新的 handoff。
- [Review And Fix](review-and-fix.md)：用于处理明确反馈。Context Refresh 如果发现失败日志或 review feedback，应该建议进入 Review And Fix，而不是直接修。
- [Loops](../loops/README.md)：用于设计自动循环。Context Refresh 本身不是 loop。

## Loop Boundary

默认不要把 context refresh 做成自动 loop。

一次 refresh 足够的情况：

- 只是会话中断或换 agent。
- 目标、branch、状态记录和验证结果能互相对上。
- 下一步可以清楚归类为 resume、verify、handoff、fix、stop 或 ask human。

如果 context refresh 反复发生，优先检查：

- `STATE.md` 是否记录了真实进度。
- plan 是否有勾选状态。
- commit cadence 是否足够小。
- stop condition 是否清楚。
- agent handoff 是否写清了上下文入口。

如果 context refresh 出现在已设计的 loop 里，loop 必须写清：

- 每轮读取哪些状态。
- 哪些证据可信。
- 什么算完成。
- 什么情况停止。
- 什么情况升级给人类。
- 最大轮数。

如果这些边界答不清楚，只 refresh 一次，然后把控制权交回人类。

## Skill Capture Signals

如果多次出现以下情况，可以考虑沉淀为项目级 skill：

- 每次恢复会话都要重复同一套 refresh checklist。
- agent 经常漏读同一批 state records、plan、branch 或 commit 信息。
- 项目有稳定的状态文件、branch 规则、验证命令和完成信号。
- context compaction 或 agent handoff 很常见。
- refresh 输出需要固定格式才能被后续 handoff 或 review 使用。

项目级 skill 优先。只有当这个 refresh 模式跨多个项目也稳定有效时，才记录为系统级 skill 候选，并且必须经过人类 review。

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
````

- [ ] **Step 3: Verify required sections exist**

Run:

```bash
rg -n "^## (用途 / Purpose|适用场景 / When to Use|不适用场景 / When Not to Use|输入材料 / Inputs|Context Refresh Template|Claude Code Prompt|Codex CLI Prompt|期望输出 / Expected Output|验证 / Verification|停止与升级 / Stop And Escalate|和其他 Cookbook 场景的关系 / Relationship To Other Cookbook Scenarios|Loop Boundary|Skill Capture Signals|相关文档 / Related Docs)" zh/cookbook/context-refresh.md
```

Expected:

```text
All fourteen required section headings are listed.
```

- [ ] **Step 4: Verify prompts preserve read-only boundary**

Run:

```bash
rg -n "不要修改文件|不要继续实现|不要自动修复|不要编造|git status --short --branch|git log --oneline -5" zh/cookbook/context-refresh.md
```

Expected:

```text
The output includes matches for read-only behavior, no implementation, no invented commands, and the two git inspection commands.
```

- [ ] **Step 5: Commit the scenario page**

Run:

```bash
git diff --check
git add zh/cookbook/context-refresh.md
git commit -m "docs: add context refresh cookbook"
```

Expected:

```text
git diff --check has no output.
Commit succeeds with message "docs: add context refresh cookbook".
```

---

### Task 2: Link Context Refresh From Cookbook Entrypoint

**Files:**
- Modify: `zh/cookbook/README.md`
- Reference: `zh/cookbook/context-refresh.md`

- [ ] **Step 1: Read the cookbook entrypoint**

Run:

```bash
sed -n '1,180p' zh/cookbook/README.md
```

Expected:

```text
The current scenarios list contains Repo Orientation, Task Handoff, and Review And Fix. Planned scenarios still mention context refresh.
```

- [ ] **Step 2: Update current and planned scenarios**

Change the scenario lists in `zh/cookbook/README.md` to this exact content:

```markdown
## 当前场景 / Current Scenarios

- [Repo Orientation](repo-orientation.md)：让 agent 在修改文件前读懂一个已有仓库。
- [Task Handoff](task-handoff.md)：把一个边界清楚的任务交给 Claude Code 或 Codex CLI。
- [Review And Fix](review-and-fix.md)：处理具体 test、lint、review 或 CI 反馈。
- [Context Refresh](context-refresh.md)：在长会话、中断、agent 切换或上下文压缩之后恢复当前项目状态。

## 后续场景 / Planned Scenarios

后续可以继续补充：

- 更完整的 PR / CI 工作流。
- multi-agent delegation。

这些场景先不创建文件，等真实使用后再补。
```

- [ ] **Step 3: Verify README links and planned list**

Run:

```bash
rg -n "Context Refresh|context-refresh|长会话或中断后的 context refresh|PR / CI|multi-agent" zh/cookbook/README.md
test -f zh/cookbook/context-refresh.md
```

Expected:

```text
README contains Context Refresh and context-refresh.md.
README does not contain "长会话或中断后的 context refresh".
context-refresh.md exists.
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
git commit -m "docs: link context refresh cookbook"
```

Expected:

```text
git diff --check has no output.
Commit succeeds with message "docs: link context refresh cookbook".
```

---

### Final Verification

- [ ] **Step 1: Verify no placeholders or plan leakage**

Run:

```bash
rg -n "TBD|TODO|待定|占位|implement later|fill in" zh/cookbook/context-refresh.md zh/cookbook/README.md
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
The last two implementation commits only touch zh/cookbook/context-refresh.md and zh/cookbook/README.md.
```
