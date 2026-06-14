# Cookbook Multi-Agent Delegation Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the Chinese cookbook `Multi-Agent Delegation` scenario and link it from the cookbook entrypoint.

**Architecture:** This is a pure documentation change. The new page `zh/cookbook/multi-agent-delegation.md` owns the scenario content; `zh/cookbook/README.md` only lists it as a current scenario and replaces the exhausted planned-scenarios list with a short future-use note.

**Tech Stack:** Markdown documentation, existing `zh/cookbook/` structure, git verification commands.

---

## File Structure

- Create: `zh/cookbook/multi-agent-delegation.md`
  - Responsibility: explain when and how to split work across multiple agents using explicit ownership, bounded handoffs, reviews, integration, conflict handling, and final verification.
  - Must follow the scenario structure used by `repo-orientation.md`, `task-handoff.md`, `review-and-fix.md`, `context-refresh.md`, and `pr-ci-workflow.md`.
- Modify: `zh/cookbook/README.md`
  - Responsibility: list Multi-Agent Delegation under current scenarios and remove it from planned scenarios.
- Do not modify: English placeholder files, `zh/cases/`, scripts, or unrelated docs.

## Acceptance Checklist

- `zh/cookbook/multi-agent-delegation.md` exists.
- The page includes Purpose, When to Use, When Not to Use, Inputs, Multi-Agent Delegation Template, Controller Responsibilities, Worker Responsibilities, Reviewer Responsibilities, Claude Code Prompt, Codex CLI Prompt, Expected Output, Verification, Stop and Escalate, Conflict Handling, Loop Boundary, Skill Capture Signals, and Related Docs.
- The page includes a copyable Multi-Agent Delegation Template.
- The page defines controller, worker, and reviewer responsibilities.
- Prompts require non-overlapping ownership and explicit forbidden actions.
- Prompts tell workers not to revert or overwrite other agents' work.
- Prompts do not ask agents to run automatic loops.
- Prompts do not authorize push, PR update, merge, deployment, migration, permission changes, or external service access without explicit authorization.
- The page distinguishes itself from task handoff, review-and-fix, context refresh, PR CI workflow, loops, and skill workflow.
- `zh/cookbook/README.md` links Multi-Agent Delegation as a current scenario.
- The planned scenarios section no longer lists multi-agent delegation as future work.
- Related links point to existing files.
- `git diff --check` passes.

---

### Task 1: Add Multi-Agent Delegation Scenario Page

**Files:**
- Create: `zh/cookbook/multi-agent-delegation.md`
- Reference: `docs/superpowers/specs/2026-06-13-cookbook-multi-agent-delegation-design.md`
- Reference: `zh/cookbook/task-handoff.md`
- Reference: `zh/cookbook/review-and-fix.md`
- Reference: `zh/cookbook/context-refresh.md`
- Reference: `zh/cookbook/pr-ci-workflow.md`
- Reference: `zh/loops/README.md`
- Reference: `zh/skill-workflow/README.md`

- [ ] **Step 1: Read the scenario style and design spec**

Run:

```bash
sed -n '1,380p' docs/superpowers/specs/2026-06-13-cookbook-multi-agent-delegation-design.md
sed -n '1,220p' zh/cookbook/task-handoff.md
sed -n '1,260p' zh/cookbook/review-and-fix.md
sed -n '1,300p' zh/cookbook/context-refresh.md
sed -n '1,360p' zh/cookbook/pr-ci-workflow.md
sed -n '1,220p' zh/loops/README.md
sed -n '1,240p' zh/skill-workflow/README.md
```

Expected:

```text
The spec and existing pages are readable. Existing cookbook pages use bilingual section headings, Chinese explanatory text, copyable templates, prompts, verification, stop/escalate, loop boundary, and skill capture sections.
```

- [ ] **Step 2: Create `zh/cookbook/multi-agent-delegation.md`**

Write this complete file:

````markdown
# Multi-Agent Delegation

这个场景用于把一个任务拆给多个 agent，同时保持范围、ownership、review、集成和最终验证可控。

目标不是默认多开 agent，也不是让多个 agent 自动循环修问题，而是在任务确实可以拆分时，让每个 agent 只负责清楚边界内的工作，并由 controller 负责收敛结果。

## 用途 / Purpose

这个场景帮助人类或 controller agent 判断是否应该使用 multi-agent delegation，并把任务拆成互不冲突、可验证、可 review 的子任务。

完成后，应该得到清楚的 agent assignments、ownership 边界、review 计划、integration 顺序和最终验证结果。

## 适用场景 / When to Use

适合：

- 多个子任务可以独立推进。
- 不同文件、目录或模块有清楚 ownership。
- 多个探索问题可以并行回答。
- 需要把 implementation 和 review 分开。
- 一个较大任务可以拆成小而安全的 slices。
- controller 需要收集多个 agent 输出后再统一集成。

## 不适用场景 / When Not to Use

不适合：

- 任务目标还不清楚。
- 多个 agent 必须频繁修改同一个文件。
- ownership 无法拆开。
- 没有真实验证信号。
- 项目级别不清楚，尤其可能是 Level 4。
- 涉及生产、权限、认证、支付、敏感数据、安全、合规、migration、部署或基础设施风险，但没有人类确认门。
- 你只是想让 agent 自动循环修问题；这种情况先读 [Loops](../loops/README.md)。

multi-agent delegation 不是默认策略。并行本身不代表更安全，也不代表更快。

## 输入材料 / Inputs

至少提供：

- 项目级别。
- 总目标。
- 为什么需要 multi-agent。
- agent roles。
- 每个 agent 拥有的文件、目录或问题范围。
- 每个 agent 禁止触碰的文件或动作。
- 共享上下文文件。
- 期望输出格式。
- 验证命令或验证信号。
- integration owner。
- review owner。
- 状态记录位置。
- 人类确认门。
- 已知风险。

如果没有 ownership 边界，就不要开始 multi-agent delegation。

## Multi-Agent Delegation Template

复制下面模板到 `STATE.md`、issue、PR comment、任务说明或对话里。

```markdown
## Multi-Agent Delegation

- Overall goal:
- Project level:
- Why multi-agent:
- Shared context:
- Agent assignments:
  - Agent name or role:
    - Task goal:
    - Owned files or areas:
    - Context files:
    - Forbidden files or actions:
    - Verification:
    - Completion criteria:
- Owned files or areas:
- Forbidden files or actions:
- Allowed actions:
- Verification command or signal:
- Expected output format:
- Integration owner:
- Review owner:
- State record location:
- Human confirmation gates:
- Known risks:
- Open questions:
```

填写要求：

- `Why multi-agent` 必须说明为什么并行比单 agent 更合适。
- `Owned files or areas` 必须具体到文件、目录、模块或只读问题。
- `Forbidden files or actions` 必须写清不能触碰的范围。
- 每个 agent 的 assignment 都必须有 verification 或说明当前没有真实验证信号。
- `Integration owner` 负责最终合并和冲突处理。
- `Review owner` 负责独立 review，不能只依赖 worker 自评。

## Controller Responsibilities

controller 可以是人类，也可以是主控 agent。controller 负责：

- 判断 multi-agent 是否真的有必要。
- 把任务拆成相互独立的 slices。
- 分配不重叠的 ownership。
- 给每个 worker 只提供它完成任务需要的上下文。
- 防止多个 worker 修改同一文件，除非有明确 integration plan。
- 收集 worker 输出。
- 安排 spec compliance review 和 quality review。
- 处理冲突。
- 按可控顺序集成结果。
- 运行最终验证。
- 更新状态记录。
- 判断是否停止或升级给人类。

controller 对最终状态负责。worker report 是证据，不是证明。

## Worker Responsibilities

每个 worker 必须：

- 只在自己的 owned scope 内工作。
- 不要 revert 或 overwrite 其他 agent 的改动。
- 发现任务需要超出 ownership 的文件或动作时停止。
- 不要做未授权的大范围重构。
- 运行指定验证，或说明为什么无法运行。
- 输出 changed files、关键决策、验证结果、残余风险和问题。
- 不要在没有证据时声称完成。

worker 不应该 merge、push、更新 PR metadata、部署、改变共享状态或访问外部服务，除非 assignment 明确授权。

## Reviewer Responsibilities

multi-agent delegation 至少需要三类 review 视角：

- spec reviewer：检查 worker 输出是否满足 assignment 和总体计划。
- quality reviewer：检查清晰度、可维护性、安全边界和 scope control。
- final reviewer：检查所有 worker 输出集成后的整体结果。

worker 的 self-review 有价值，但不能替代独立 review。

## Claude Code Prompt

```text
请根据下面的 Multi-Agent Delegation 判断是否应该拆给多个 agent，并给出可执行的 delegation plan。

要求：
- 先阅读总目标、项目级别、共享上下文和当前 repo 状态。
- 判断任务是否真的适合 multi-agent；如果不适合，请说明原因并建议单 agent 或 human-led 方式。
- 如果适合，请把任务拆成互相独立的 assignments。
- 每个 assignment 必须包含 task goal、owned files or areas、context files、forbidden files or actions、allowed actions、verification、completion criteria 和 expected output。
- ownership 必须尽量不重叠；如果无法避免重叠，必须写清 integration owner 和 conflict handling plan。
- 明确 controller、worker、reviewer 的职责。
- 要求 worker 不要 revert 或 overwrite 其他 agent 的改动。
- 要求 worker 只改 owned scope，超出范围时停止。
- 要求每个 worker 输出 changed files、验证结果、残余风险和问题。
- 集成前必须有 spec review 和 quality review；最终必须有 final verification。
- 不要编造不存在的命令、测试、团队规则或风险假设。
- 不要自动进入 loop。
- 未经明确授权，不要 push、更新 PR metadata、merge、部署、migrate、改变权限或访问外部服务。

请输出：
- 是否适合 multi-agent delegation
- decomposition rationale
- agent assignments
- owned / forbidden files or areas
- verification plan
- review plan
- integration order
- conflict handling plan
- stop / escalate conditions
- state update plan

Multi-Agent Delegation:
<粘贴 Multi-Agent Delegation 模板内容>
```

## Codex CLI Prompt

```text
请在当前 repo 中根据下面的 Multi-Agent Delegation 做 delegation 设计，不要直接开始多 agent 执行。

要求：
- 先读取共享上下文、项目级别、总目标和当前 repo 状态。
- 判断任务是否适合 multi-agent；如果 ownership 无法拆开或验证信号缺失，请停止并说明。
- 如果适合，请输出每个 worker 的 bounded handoff。
- 每个 worker handoff 必须包含 task goal、owned files or areas、context files、forbidden files or actions、allowed actions、verification、completion criteria 和 expected output。
- 明确每个 worker 不得 revert 或 overwrite 其他 agent 的改动。
- 明确 controller 如何收集输出、review、集成和最终验证。
- 如果需要修改同一文件，必须先写 conflict handling plan，不要让多个 worker 直接同时改。
- 不要编造命令、测试、团队规则或风险假设。
- 不要自动 loop。
- 未经明确授权，不要 push、更新 PR metadata、merge、部署、migrate、改变权限或访问外部服务。

请输出：
- multi-agent 是否适合
- 拆分依据
- worker handoffs
- review plan
- integration plan
- verification plan
- conflict handling plan
- stop / escalate conditions
- 需要人类确认的问题

Multi-Agent Delegation:
<粘贴 Multi-Agent Delegation 模板内容>
```

## 期望输出 / Expected Output

controller 输出应该包含：

- 是否适合 multi-agent delegation。
- 拆分依据。
- agent assignments。
- owned 和 forbidden files or areas。
- verification plan。
- integration order。
- review plan。
- stop / escalate conditions。
- state update plan。

worker 输出应该包含：

- 对任务范围的理解。
- changed files 或只读发现。
- 验证命令和结果。
- 残余风险。
- 问题或阻塞。
- 建议的状态更新。

## 验证 / Verification

检查每个 worker 输出：

- 是否只在 owned files or areas 内工作。
- 是否没有触碰 forbidden files or actions。
- 是否运行了指定验证，或说明无法运行的原因。
- 是否报告 changed files、风险和问题。

检查集成结果：

- 是否没有丢失其他 worker 的改动。
- final diff 是否符合总目标。
- 冲突解决是否有证据。
- 是否运行最终验证命令或 checklist。

常用检查：

```bash
git status --short --branch
git diff --check
```

如果项目有测试、lint、typecheck、docs check 或 CI，本场景必须使用真实项目验证信号。

## 停止与升级 / Stop And Escalate

遇到以下情况，controller 必须停止并交回人类：

- 任务无法拆成独立 slices。
- ownership 重叠且无法解决。
- 两个 agent 需要修改同一个文件，但没有 integration plan。
- 项目级别不清楚，尤其可能是 Level 4。
- 没有真实验证信号。
- 任一 worker 需要扩大范围。
- 任一 worker 报告互相矛盾的假设。
- 集成时出现冲突或非预期改动。
- 涉及生产、权限、认证、支付、敏感数据、安全、合规、migration、基础设施、部署或外部服务。
- 需要 push、更新 PR metadata、merge、rerun remote checks、部署、migrate、改变权限或访问外部服务，但没有明确授权。

## Conflict Handling

优先通过 ownership assignment 避免冲突。

如果发生冲突：

- 停止并阅读双方改动。
- 不要让一个 worker 直接覆盖另一个 worker。
- 由 integration owner 基于证据解决冲突。
- 记录保留、修改或丢弃了哪些内容。
- 冲突解决后重新运行相关验证。
- 如果无法解释冲突来源或解决依据，交回人类。

## Loop Boundary

multi-agent delegation 不是自动 loop。

并行运行多个 agent 不代表可以反复自动修问题。如果 delegation 出现在已设计的 loop 里，loop 必须写清：

- 最大轮数。
- 每个 agent 的 ownership。
- 允许动作。
- 禁止动作。
- 验证信号。
- 停止条件。
- 升级条件。
- integration rules。

如果这些边界答不清楚，只做一轮 delegation，然后交回人类或 controller 决策。

## Skill Capture Signals

如果多次出现以下情况，可以考虑沉淀为项目级 skill：

- 项目反复使用同一组 agent roles。
- ownership boundaries 稳定。
- verification 和 review gates 稳定。
- controller / reviewer workflow 在多个任务中重复。
- delegation 明显减少人类协调成本。
- 同一套 delegation pattern 在多个 PR 或任务中有效。

项目级 skill 优先。只有当这个 delegation 模式跨多个项目也稳定有效时，才记录为系统级 skill 候选，并且必须经过人类 review。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
- [Task Handoff](task-handoff.md)
- [Review And Fix](review-and-fix.md)
- [Context Refresh](context-refresh.md)
- [PR CI Workflow](pr-ci-workflow.md)
````

- [ ] **Step 3: Verify required sections exist**

Run:

```bash
rg -n "^## (用途 / Purpose|适用场景 / When to Use|不适用场景 / When Not to Use|输入材料 / Inputs|Multi-Agent Delegation Template|Controller Responsibilities|Worker Responsibilities|Reviewer Responsibilities|Claude Code Prompt|Codex CLI Prompt|期望输出 / Expected Output|验证 / Verification|停止与升级 / Stop And Escalate|Conflict Handling|Loop Boundary|Skill Capture Signals|相关文档 / Related Docs)" zh/cookbook/multi-agent-delegation.md
```

Expected:

```text
All seventeen required section headings are listed.
```

- [ ] **Step 4: Verify prompt safety and ownership boundaries**

Run:

```bash
rg -n "ownership|owned|forbidden|不要 revert|overwrite|不要自动|未经明确授权|不要 push|review|integration|conflict|Loop|Skill" zh/cookbook/multi-agent-delegation.md
```

Expected:

```text
The output includes matches for ownership, forbidden actions, no revert/overwrite, no automatic loop, authorization gates, review, integration, conflict handling, loop boundary, and skill capture.
```

- [ ] **Step 5: Commit the scenario page**

Run:

```bash
git diff --check
git add zh/cookbook/multi-agent-delegation.md
git commit -m "docs: add multi-agent delegation cookbook"
```

Expected:

```text
git diff --check has no output.
Commit succeeds with message "docs: add multi-agent delegation cookbook".
```

---

### Task 2: Link Multi-Agent Delegation From Cookbook Entrypoint

**Files:**
- Modify: `zh/cookbook/README.md`
- Reference: `zh/cookbook/multi-agent-delegation.md`

- [ ] **Step 1: Read the cookbook entrypoint**

Run:

```bash
sed -n '1,180p' zh/cookbook/README.md
```

Expected:

```text
The current scenarios list contains Repo Orientation, Task Handoff, Review And Fix, Context Refresh, and PR CI Workflow. Planned scenarios still mention multi-agent delegation.
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
- [Multi-Agent Delegation](multi-agent-delegation.md)：把可独立推进的任务拆给多个 agent，并控制 ownership、review、集成和最终验证。

## 后续场景 / Planned Scenarios

后续场景等真实使用后再补。
```

- [ ] **Step 3: Verify README links and planned list**

Run:

```bash
rg -n "Multi-Agent Delegation|multi-agent-delegation|multi-agent delegation|后续场景等真实使用后再补" zh/cookbook/README.md
test -f zh/cookbook/multi-agent-delegation.md
```

Expected:

```text
README contains Multi-Agent Delegation and multi-agent-delegation.md.
README does not contain the old planned bullet "- multi-agent delegation。".
multi-agent-delegation.md exists.
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
test -f zh/cookbook/task-handoff.md
test -f zh/cookbook/review-and-fix.md
test -f zh/cookbook/context-refresh.md
test -f zh/cookbook/pr-ci-workflow.md
test -f zh/cookbook/multi-agent-delegation.md
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
git commit -m "docs: link multi-agent delegation cookbook"
```

Expected:

```text
git diff --check has no output.
Commit succeeds with message "docs: link multi-agent delegation cookbook".
```

---

### Final Verification

- [ ] **Step 1: Verify no placeholders or plan leakage**

Run:

```bash
rg -n "TBD|TODO|待定|占位|implement later|fill in" zh/cookbook/multi-agent-delegation.md zh/cookbook/README.md
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
The last two implementation commits only touch zh/cookbook/multi-agent-delegation.md and zh/cookbook/README.md.
```
