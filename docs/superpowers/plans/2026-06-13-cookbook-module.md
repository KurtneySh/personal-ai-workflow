# Cookbook Module Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the first Chinese `zh/cookbook/` module with scenario-driven Claude Code and Codex CLI usage guidance.

**Architecture:** Create a compact cookbook entrypoint plus two scenario pages: repository orientation and task handoff. Each scenario embeds prompts in context, includes verification and stop/escalate guidance, and links back to project levels, infra, loops, and skill workflow.

**Tech Stack:** Markdown documentation, Git, ripgrep validation, Conventional Commits.

---

## File Structure

Create or modify these files:

```text
zh/cookbook/README.md
zh/cookbook/repo-orientation.md
zh/cookbook/task-handoff.md
README.zh.md
```

Do not modify English stub files or create the `zh/cases/` module in this plan.

## Task 1: Cookbook Entrypoint

**Files:**
- Create: `zh/cookbook/README.md`

- [ ] **Step 1: Create the cookbook directory**

Run:

```bash
mkdir -p zh/cookbook
```

Expected: directory exists.

- [ ] **Step 2: Create the cookbook entrypoint**

Create `zh/cookbook/README.md` with this content:

```markdown
# Cookbook

Cookbook 用于把 AI 开发工作流落到具体任务场景里。

它不是 Claude Code 或 Codex CLI 的命令大全，而是一组场景化做法：什么时候使用、给 agent 什么上下文、怎么写 prompt、怎么验证、什么时候停止，以及什么信号说明这个流程值得沉淀为 skill。

## 定位 / Positioning

- `zh/playbooks/`：端到端工作流，例如从空仓库启动项目。
- `zh/cookbook/`：具体任务场景，例如让 agent 读懂 repo 或接手一个小任务。
- `zh/loops/`：判断一个任务是否适合自动循环。
- `zh/skill-workflow/`：判断重复场景是否值得沉淀为 skill。

Cookbook 里的 prompt 不是魔法指令。它们仍然依赖真实项目上下文、明确边界和可验证结果。

## 当前场景 / Current Scenarios

- [Repo Orientation](repo-orientation.md)：让 agent 在修改文件前读懂一个已有仓库。
- [Task Handoff](task-handoff.md)：把一个边界清楚的任务交给 Claude Code 或 Codex CLI。

## 后续场景 / Planned Scenarios

后续可以继续补充：

- review / test / lint feedback 修复。
- 长会话或中断后的 context refresh。
- PR / CI 工作流。
- multi-agent delegation。

这些场景先不创建文件，等真实使用后再补。

## 如何使用一个场景 / How To Use A Scenario

1. 判断项目级别。
2. 阅读当前项目状态。
3. 选择匹配的 cookbook 场景。
4. 填入输入、约束和禁止事项。
5. 运行对应 Claude Code 或 Codex CLI prompt。
6. 用真实信号验证结果。
7. 记录状态、停止或升级给人类，必要时考虑是否沉淀为 skill。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
```

- [ ] **Step 3: Verify entrypoint sections**

Run:

```bash
rg -n "Cookbook|定位 / Positioning|当前场景 / Current Scenarios|Repo Orientation|Task Handoff|后续场景 / Planned Scenarios|如何使用一个场景|相关文档 / Related Docs" zh/cookbook/README.md
```

Expected: matches for entrypoint purpose, current scenarios, planned scenarios, usage checklist, and related docs.

- [ ] **Step 4: Verify related links exist**

Run:

```bash
for f in zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md zh/playbooks/start-a-new-project.md zh/loops/README.md zh/skill-workflow/README.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 5: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/cookbook/README.md
```

Expected: no output.

- [ ] **Step 6: Commit the entrypoint**

Run:

```bash
git add zh/cookbook/README.md
git commit -m "docs: add cookbook module entrypoint"
```

Expected: commit succeeds.

## Task 2: Repository Orientation Scenario

**Files:**
- Create: `zh/cookbook/repo-orientation.md`

- [ ] **Step 1: Create repository orientation scenario**

Create `zh/cookbook/repo-orientation.md` with this content:

````markdown
# Repo Orientation

这个场景用于让 Claude Code 或 Codex CLI 在修改文件前读懂一个已有仓库。

目标不是让 agent 立即开始工作，而是先形成一份可验证的 repo orientation：项目做什么、当前处于什么状态、有哪些关键文件、可以运行哪些验证命令、哪些风险或未知信息需要人类确认。

## 适用场景 / When to Use

适合：

- 第一次进入一个陌生 repo。
- 隔了一段时间后重新接手项目。
- 给一个新的 agent 建立上下文。
- 在 task handoff 前先让 agent 做项目理解。

## 不适用场景 / When Not to Use

不适合：

- 你已经明确知道要修改什么文件，并且上下文足够。
- 你希望 agent 直接执行一个修改任务。
- 你准备设计自动 loop。
- 项目可能涉及 Level 4 风险，但还没有人类确认风险边界。

这个场景不给 agent 修改文件的权限。

## 输入材料 / Inputs

尽量提供：

- 项目目标或一句话背景。
- 当前想了解的问题。
- 已知项目级别；不确定时要求 agent 判断并说明依据。
- 需要重点阅读的文件或目录。
- 禁止操作，例如不要修改文件、不要安装依赖、不要运行部署命令。

## Claude Code Prompt

```text
请先只做 repo orientation，不要修改文件。

请阅读当前 repo 中存在的 README、STATE、SPEC、AGENTS、package/config 文件、脚本和主要目录结构。

要求：
- 只基于真实存在的文件和命令。
- 判断项目级别 Level 1-4；如果证据不足，请说明“不确定”和需要人类确认的问题。
- 总结项目目标、目录结构、当前状态、关键约定、可用验证命令、风险点和未知信息。
- 如果发现 README、STATE、SPEC、AGENTS 之间有矛盾，请列出来。
- 不要编造不存在的命令、测试、部署流程、团队规则或风险假设。
- 不要修改文件，不要安装依赖，不要运行部署、migrate、权限、认证、支付、生产或敏感数据相关命令。

请输出：
- 项目一句话总结
- 项目级别判断和依据
- 关键文件和目录
- 当前状态
- 可用验证命令以及证据来源
- 风险和未知信息
- 建议下一步
```

## Codex CLI Prompt

```text
请在当前 repo 内做一次只读 orientation。

请读取 README.md、STATE.md，以及如果存在的 SPEC.md、AGENTS.md、package/config 文件、脚本和主要目录结构。

要求：
- 只使用 repo 中真实存在的信息。
- 不要修改文件。
- 不要编造命令、测试、部署流程、团队规则或风险假设。
- 如果找不到验证命令，请明确说明。
- 如果不确定项目级别，请说明需要人类确认哪些信息。
- 不要运行部署、migrate、权限、认证、支付、生产或敏感数据相关命令。

请输出：
- repo 概览
- 项目级别判断
- 关键文件和目录
- 当前状态
- 真实可用的验证命令
- 文档矛盾或缺口
- 风险和需要人类确认的问题
- 建议下一步
```

## 期望输出 / Expected Output

一份 orientation 应该包含：

- 项目一句话总结。
- 项目级别判断和依据。
- 关键文件、目录和脚本。
- 当前状态和明显缺口。
- 真实存在的验证命令。
- 风险、未知信息和需要人类确认的问题。
- 下一步建议。

## 验证 / Verification

检查 agent 输出：

- 是否引用了真实文件。
- 推荐的命令是否真实存在。
- 找不到的信息是否明确标为未知。
- 是否没有把猜测写成事实。
- 是否没有修改文件。

可以运行：

```bash
git status --short
```

Expected: 如果只是 orientation，应无文件改动。

## 停止与升级 / Stop And Escalate

遇到以下情况，停止并交给人类判断：

- 项目级别可能是 Level 4。
- 涉及生产、权限、认证、支付、敏感数据、安全或合规。
- 文档之间互相矛盾。
- 找不到真实验证命令。
- agent 想要安装依赖、修改文件、部署、migrate、访问外部服务或改变权限。
- agent 无法说明信息来源。

## Skill Capture Signals

如果多次出现以下情况，可以考虑沉淀为项目级 skill：

- 每次进入 repo 都要重复同样的 orientation 步骤。
- agent 总是漏读同一批关键文件。
- orientation 输出需要固定格式才能被后续任务复用。
- 项目有稳定的验证命令、风险边界和状态记录规则。

如果这种 orientation 模式跨多个项目都有效，可以记录为系统级 skill 候选，但不能自动升级。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
````

- [ ] **Step 2: Verify repository orientation scenario**

Run:

```bash
rg -n "Repo Orientation|适用场景 / When to Use|不适用场景 / When Not to Use|Claude Code Prompt|Codex CLI Prompt|期望输出 / Expected Output|验证 / Verification|停止与升级 / Stop And Escalate|Skill Capture Signals" zh/cookbook/repo-orientation.md
```

Expected: matches for all required sections.

- [ ] **Step 3: Verify read-only and no-invention guidance**

Run:

```bash
rg -n "不要修改文件|不要编造|只基于真实存在|真实可用的验证命令|git status --short|不能自动升级" zh/cookbook/repo-orientation.md
```

Expected: matches for read-only orientation, no invention, verification, and no automatic skill upgrade.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/cookbook/repo-orientation.md
```

Expected: no output.

- [ ] **Step 5: Commit repository orientation scenario**

Run:

```bash
git add zh/cookbook/repo-orientation.md
git commit -m "docs: add repo orientation cookbook"
```

Expected: commit succeeds.

## Task 3: Task Handoff Scenario

**Files:**
- Create: `zh/cookbook/task-handoff.md`

- [ ] **Step 1: Create task handoff scenario**

Create `zh/cookbook/task-handoff.md` with this content:

````markdown
# Task Handoff

这个场景用于把一个边界清楚的任务交给 Claude Code 或 Codex CLI。

好的 handoff 不是只写“帮我做这个”，而是写清目标、项目级别、上下文、允许动作、禁止动作、验证方式、完成标准和什么时候必须交回人类。

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
````

- [ ] **Step 2: Verify task handoff scenario**

Run:

```bash
rg -n "Task Handoff|适用场景 / When to Use|不适用场景 / When Not to Use|Handoff Template|Claude Code Prompt|Codex CLI Prompt|期望输出 / Expected Output|验证 / Verification|停止与升级 / Stop And Escalate|Skill Capture Signals" zh/cookbook/task-handoff.md
```

Expected: matches for all required sections.

- [ ] **Step 3: Verify handoff fields and boundaries**

Run:

```bash
rg -n "Task goal|Project level|Allowed actions|Forbidden actions|Verification command or signal|Completion criteria|State update location|Human confirmation gates|不要编造|不要运行部署|不能自动升级" zh/cookbook/task-handoff.md
```

Expected: matches for template fields and safety boundaries.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/cookbook/task-handoff.md
```

Expected: no output.

- [ ] **Step 5: Commit task handoff scenario**

Run:

```bash
git add zh/cookbook/task-handoff.md
git commit -m "docs: add task handoff cookbook"
```

Expected: commit succeeds.

## Task 4: README Link And Cross-Page Verification

**Files:**
- Modify: `README.zh.md`
- Verify: `zh/cookbook/README.md`
- Verify: `zh/cookbook/repo-orientation.md`
- Verify: `zh/cookbook/task-handoff.md`

- [ ] **Step 1: Link cookbook from `README.zh.md`**

Find this line:

```markdown
- `zh/cookbook/`：按场景使用 Claude Code 和 Codex CLI。
```

Replace it with:

```markdown
- `zh/cookbook/`：按场景使用 Claude Code 和 Codex CLI。场景化 prompt 和交接模板见 [Cookbook](zh/cookbook/README.md)。
```

- [ ] **Step 2: Verify README link**

Run:

```bash
rg -n "Cookbook|zh/cookbook/README.md" README.zh.md
```

Expected: README.zh.md links to the cookbook README.

- [ ] **Step 3: Verify cookbook pages link to each other**

Run:

```bash
rg -n "repo-orientation.md|task-handoff.md|Repo Orientation|Task Handoff" zh/cookbook/README.md
```

Expected: README links both scenario pages.

- [ ] **Step 4: Verify all related links point to files**

Run:

```bash
for f in zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md zh/playbooks/start-a-new-project.md zh/loops/README.md zh/skill-workflow/README.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 5: Verify no unresolved drafting markers**

Run:

```bash
rg -n "TBD|TODO|待定|正文不完整|草稿未清理|临时文本" zh/cookbook README.zh.md || true
```

Expected: no output.

- [ ] **Step 6: Verify English stub files unchanged**

Run:

```bash
git status --short en
```

Expected: no output.

- [ ] **Step 7: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/cookbook README.zh.md
```

Expected: no output.

- [ ] **Step 8: Commit README link**

Run:

```bash
git add README.zh.md
git commit -m "docs: link cookbook module"
```

Expected: commit succeeds.

## Task 5: Final Acceptance Verification

**Files:**
- Verify: `zh/cookbook/README.md`
- Verify: `zh/cookbook/repo-orientation.md`
- Verify: `zh/cookbook/task-handoff.md`
- Verify: `README.zh.md`

- [ ] **Step 1: Verify required files exist**

Run:

```bash
for f in zh/cookbook/README.md zh/cookbook/repo-orientation.md zh/cookbook/task-handoff.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 2: Verify cookbook entrypoint positioning**

Run:

```bash
rg -n "playbooks|cookbook|loops|skill-workflow|不是 Claude Code 或 Codex CLI 的命令大全|真实项目上下文|验证" zh/cookbook/README.md
```

Expected: matches for cookbook positioning and scenario-driven stance.

- [ ] **Step 3: Verify both scenarios include prompts**

Run:

```bash
rg -n "Claude Code Prompt|Codex CLI Prompt" zh/cookbook/repo-orientation.md zh/cookbook/task-handoff.md
```

Expected: both scenario files contain Claude Code and Codex CLI prompt sections.

- [ ] **Step 4: Verify prompt safety language**

Run:

```bash
rg -n "不要编造|真实存在|真实验证|不要修改文件|不要运行部署|不要做未请求的大范围重构|不要运行部署、migrate、权限、认证、支付、生产或敏感数据相关命令" zh/cookbook/repo-orientation.md zh/cookbook/task-handoff.md
```

Expected: matches for no-invention, real evidence, and safety boundaries.

- [ ] **Step 5: Verify scenario operational sections**

Run:

```bash
rg -n "验证 / Verification|停止与升级 / Stop And Escalate|Skill Capture Signals|相关文档 / Related Docs" zh/cookbook/repo-orientation.md zh/cookbook/task-handoff.md
```

Expected: both scenario files include verification, stop/escalate, skill capture, and related docs.

- [ ] **Step 6: Verify no automatic loop instruction**

Run:

```bash
rg -n "自动 loop|自动循环|不要.*loop|不适合.*loop|需要自动 loop" zh/cookbook/README.md zh/cookbook/repo-orientation.md zh/cookbook/task-handoff.md
```

Expected: matches only cautionary or not-applicable loop language, not instructions to run loops automatically.

- [ ] **Step 7: Verify README.zh link**

Run:

```bash
rg -n "zh/cookbook/README.md|场景化 prompt 和交接模板" README.zh.md
```

Expected: README.zh.md links to the cookbook entrypoint.

- [ ] **Step 8: Verify no English files changed**

Run:

```bash
git status --short en
```

Expected: no output.

- [ ] **Step 9: Run final Markdown whitespace check**

Run:

```bash
git diff --check
```

Expected: no output.

- [ ] **Step 10: Review recent commits**

Run:

```bash
git log --oneline -8
```

Expected: cookbook module commits use Conventional Commits messages beginning with `docs:`.
