# Task Handoff

这个场景用于把一个边界清楚的任务交给 Claude Code 或 Codex CLI。

好的 handoff 不是只写“帮我做这个”，而是写清目标、项目级别、上下文、允许动作、禁止动作、验证方式、完成标准和什么时候必须交回人类。

## 用途 / Purpose

这个场景帮助人类把一个可执行任务整理成 agent 能接手的工作说明。

完成后，agent 应该知道要做什么、不能做什么、如何验证、什么时候必须停下来交回人类。

## 适用场景 / When to Use

适合：

- 项目已经有最小 AI workflow infra。
- 任务目标清楚。
- 修改范围可以被限制在少数文件或目录。
- 有真实验证命令，或需要明确说明当前没有验证信号。
- 人类希望 agent 执行或先提出一个具体改动方案。

## 不适用场景 / When Not to Use

不适合：

- 目标仍然是开放探索。
- 需要先判断项目级别或补齐基础 infra。
- 涉及 Level 4 风险但没有人类确认门。
- 需要自动 loop。
- 任务需要大范围重构、权限变化、数据模型变化、部署或生产操作，但边界还没写清。

## 输入材料 / Inputs

至少提供：

- 任务目标。
- 项目级别。
- 已知上下文文件。
- 可能涉及的文件或目录。
- 允许动作。
- 禁止动作。
- 验证命令或验证信号。
- 完成标准。
- 需要更新的状态记录位置。
- 人类确认门。

## Handoff Template

复制下面模板到 issue、任务说明、`STATE.md` 或对话里。

```markdown
## Task Handoff

- Task goal:
- Project level:
- Context files:
- Likely files or areas:
- Allowed actions:
- Forbidden actions:
- Verification command or signal:
- Completion criteria:
- State update location:
- Human confirmation gates:
- Known risks:
- Questions before work:
```

填写要求：

- `Task goal` 必须是可验证的结果。
- `Allowed actions` 必须足够小，不能让 agent 自行扩大范围。
- `Forbidden actions` 必须写清生产、权限、认证、支付、敏感数据、安全、部署和大范围重构边界。
- `Verification command or signal` 必须来自真实项目；没有就写“当前没有真实验证信号”。
- `Human confirmation gates` 写清哪些动作执行前必须停下来确认。

## Claude Code Prompt

```text
请根据下面的 Task Handoff 接手任务。

要求：
- 先阅读列出的 context files，以及你判断必要的相邻文件。
- 先复述你对任务目标、范围、允许动作、禁止动作、验证方式和完成标准的理解。
- 如果信息不足、验证信号缺失、范围不清，先提问或标记阻塞，不要直接修改。
- 只做 handoff 中允许的动作。
- 不要做未要求的大范围重构。
- 不要编造不存在的命令、测试、部署流程、团队规则或风险假设。
- 如果需要安装依赖、部署、migrate、改变权限、触碰认证/支付/生产/敏感数据/安全/合规，请停止并要求人类确认。
- 完成后运行或报告验证结果；如果无法运行验证，请说明原因。
- 最后输出修改摘要、验证结果、残余风险和建议的状态更新。

Task Handoff:
<粘贴 handoff 模板内容>
```

## Codex CLI Prompt

```text
请在当前 repo 中根据下面的 Task Handoff 执行任务。

要求：
- 先读取 handoff 中列出的 context files 和必要的相邻文件。
- 在修改前复述任务目标、边界、禁止动作、验证方式和完成标准。
- 如果发现上下文不足、验证命令不存在、项目级别或风险不清，请停止并说明需要人类确认的问题。
- 只修改任务需要的文件。
- 不要做未请求的大范围重构。
- 不要编造命令、测试、部署流程、团队规则或风险假设。
- 不要运行部署、migrate、权限、认证、支付、生产或敏感数据相关命令，除非 handoff 明确批准。
- 完成后运行真实验证命令；如果无法运行，请说明原因和替代检查。
- 输出文件变更、验证结果、残余风险和下一步。

Task Handoff:
<粘贴 handoff 模板内容>
```

## 期望输出 / Expected Output

agent 完成后应该输出：

- 它理解的任务目标和范围。
- 修改了哪些文件。
- 为什么这样改。
- 运行了哪些验证命令。
- 验证结果。
- 没有验证时的原因。
- 残余风险。
- 建议写入 `STATE.md` 或任务记录的状态更新。

## 验证 / Verification

检查：

- 文件变更是否只在 handoff 允许范围内。
- 是否没有触碰禁止动作。
- 验证命令是否真实运行或明确说明无法运行。
- 输出是否包含残余风险。
- `STATE.md` 或任务记录是否需要更新。

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

- 项目级别不清楚，尤其可能是 Level 4。
- 没有真实验证信号。
- 需要扩大范围或修改未授权文件。
- 需要安装依赖、部署、migrate、改变权限、访问外部服务。
- 涉及生产、权限、认证、支付、敏感数据、安全或合规。
- 文档之间互相矛盾。
- agent 无法解释当前状态或下一步。

## Skill Capture Signals

如果多次出现以下情况，可以考虑沉淀为项目级 skill：

- 每次 handoff 都需要同一套字段。
- agent 经常因为相同的边界不清而犯错。
- 某类任务有固定输入、固定步骤和固定验证方式。
- 同一套 stop/escalate 规则反复出现。

如果这种 handoff 模式跨多个项目都有效，可以记录为系统级 skill 候选，但不能自动升级。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
