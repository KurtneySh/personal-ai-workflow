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

## 和其他 Cookbook 场景的关系 / Relationship To Other Cookbook Scenarios

- [Repo Orientation](repo-orientation.md)：用于 delegation 前理解 repo。缺少 repo 结构、命令、风险边界或验证信号时，先 orientation，再 delegation。
- [Task Handoff](task-handoff.md)：delegation 是协调层，用来拆分多个 bounded handoffs；每个 worker 仍然需要一个边界清楚的 handoff。
- [Review And Fix](review-and-fix.md)：任何 agent 发现的 test、lint、typecheck、review 或 CI 反馈，都应该转成边界清楚的 feedback fix。
- [Context Refresh](context-refresh.md)：如果 controller 失去 branch、ownership、commits 或 agent outputs 的上下文，先 refresh context，再继续 delegation。
- [PR CI Workflow](pr-ci-workflow.md)：PR / CI 工作可以使用 delegation 做独立修复或 review，但 delegation 本身不授权 push、PR update 或 merge。
- [Loops](../loops/README.md)：delegation 不是 loop。并行工作不代表可以反复自动执行。
- [Skill Workflow](../skill-workflow/README.md)：重复出现的 delegation pattern 可以成为项目级 skill 候选，但不能自动升级为系统级 skill。

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
- [PR CI Workflow](pr-ci-workflow.md)
