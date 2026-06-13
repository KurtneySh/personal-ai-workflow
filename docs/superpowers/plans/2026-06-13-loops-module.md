# Loops Module Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the first Chinese `zh/loops/README.md` entrypoint for deciding whether an AI workflow should use a loop and how to design it safely.

**Architecture:** Create one focused Markdown document that starts with a conservative default stance, defines loop types, then gives a decision checklist, Stop / Verify / Escalate model, project-level gates, a copyable design template, judgment prompts, and related links. Keep the document compact but sectioned so it can later split into deeper guides.

**Tech Stack:** Markdown documentation, Git, ripgrep validation, Conventional Commits.

---

## File Structure

Create or modify these files:

```text
zh/loops/README.md
```

Do not modify English placeholders, project templates, or existing guide files in this plan.

## Task 1: Loops README Shell And Default Stance

**Files:**
- Create: `zh/loops/README.md`

- [ ] **Step 1: Create the loops directory**

Run:

```bash
mkdir -p zh/loops
```

Expected: directory exists.

- [ ] **Step 2: Create the document shell**

Create `zh/loops/README.md` with this content:

```markdown
# Loops

Loop 是让 agent 围绕一个目标反复执行、验证、修正的工作方式。

这个目录的默认立场是：不要默认使用 loop。只有当目标清晰、动作边界明确、验证可靠、停止条件明确，并且知道什么时候交回人类时，才考虑 loop。

## 立场 / Default Stance

- 默认不 loop。
- 先判断项目级别，再判断 loop 风险。
- 没有真实验证信号，不设计 loop。
- 没有停止条件，不设计 loop。
- 没有升级给人类的条件，不设计 loop。
- Level 3/4 项目需要更强的人类确认门。
- Level 4 默认不自动 loop，除非人类明确批准风险边界、回滚或审计要求，以及 agent 权限限制。
```

- [ ] **Step 3: Verify shell sections**

Run:

```bash
rg -n "# Loops|立场 / Default Stance|默认不 loop|Level 4 默认不自动 loop" zh/loops/README.md
```

Expected: matches for the title, default stance, and Level 4 caution.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/loops/README.md
```

Expected: no output.

- [ ] **Step 5: Commit the shell**

Run:

```bash
git add zh/loops/README.md
git commit -m "docs: add loops module shell"
```

Expected: commit succeeds.

## Task 2: Loop Types And Loop Decision Checklist

**Files:**
- Modify: `zh/loops/README.md`

- [ ] **Step 1: Append loop type definitions**

Append this content to `zh/loops/README.md`:

```markdown

## Loop 类型 / Loop Types

### 任务执行 loop

模式：

```text
计划 -> 执行 -> 验证 -> 修正
```

适合：

- 目标明确。
- 操作边界小。
- 每一轮都有真实验证信号。

不适合：

- 目标还在探索。
- 需要频繁改变范围。
- 需要引入新权限、改数据模型、改部署或触碰生产风险。

### 开发反馈 loop

模式：

```text
测试 / lint / review / CI 反馈 -> 修复 -> 再验证
```

适合：

- 反馈具体。
- 失败可以复现。
- 验证命令真实可运行。

不适合：

- 反馈来自模糊偏好。
- 失败原因不清楚。
- 每轮修复都需要人类重新判断产品方向。

### 长期自动 loop

模式：

```text
监控 -> 生成 / 修复 / 维护 -> 验证 -> 重复
```

适合：

- 有明确 owner。
- 有审计记录。
- 有强停止条件。
- 有人工批准的权限边界。

不适合：

- 没有 owner。
- 没有审计或回滚要求。
- 涉及生产、权限、认证、支付、敏感数据、安全或合规，但没有人工批准。

这三类 loop 不是同一种风险。一次任务内的验证循环，不等于长期自动代理。

## 是否应该 loop / Should This Be a Loop

先回答这些问题：

- 目标结果是什么？
- agent 被允许做哪些动作？
- agent 被禁止做哪些动作？
- 什么真实验证信号判断成功或失败？
- 什么情况必须停止？
- 什么情况必须升级给人类？
- 最多允许几轮？
- 每轮之间要把状态记录在哪里？
- 当前项目级别是否要求额外审批？

如果任意问题答不清楚，不要设计 loop。把任务拆成带人类确认的顺序步骤。
```

- [ ] **Step 2: Verify loop types and decision checklist**

Run:

```bash
rg -n "任务执行 loop|开发反馈 loop|长期自动 loop|是否应该 loop|最多允许几轮|不要设计 loop" zh/loops/README.md
```

Expected: matches for all three loop types and the decision checklist.

- [ ] **Step 3: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/loops/README.md
```

Expected: no output.

- [ ] **Step 4: Commit loop types and checklist**

Run:

```bash
git add zh/loops/README.md
git commit -m "docs: define loop types and decision checklist"
```

Expected: commit succeeds.

## Task 3: Stop / Verify / Escalate And Project-Level Gates

**Files:**
- Modify: `zh/loops/README.md`

- [ ] **Step 1: Append Stop / Verify / Escalate guidance**

Append this content to `zh/loops/README.md`:

```markdown

## Stop / Verify / Escalate

每个 loop 都必须先写清 Stop / Verify / Escalate。

### Stop

遇到以下情况必须停止：

- 已达到成功条件。
- 已达到最大轮数。
- 同一类验证错误重复出现，agent 没有新的判断依据。
- 下一步需要改变范围、权限、依赖、数据模型、部署方式或风险边界。
- agent 无法解释当前状态、最近一轮做了什么、下一步要做什么。

### Verify

每个 loop 都必须有真实验证信号，例如：

- test / lint / typecheck / build。
- CI 或 review 结果。
- 人类确认过的 checklist。
- 明确记录“找不到真实验证命令”。

验证失败不能当作成功。没有验证信号时，不要 loop。

### Escalate

遇到以下情况必须升级给人类：

- 不确定是否属于 Level 4。
- 涉及生产、权限、认证、支付、敏感数据、安全或合规。
- 需要运行可能部署、migrate 数据、写文件、安装依赖、联系外部服务、改变认证或权限的命令。
- 文档之间互相矛盾。
- 缺少真实验证信号。
- 同一类失败重复出现。

## 按项目级别的 loop 门槛

### Level 1：个人小项目 / 实验项目

可以设计轻量 loop，但必须满足：

- 动作是 read-only 或低风险。
- 有明确停止条件。
- 有最大轮数。
- 状态写入 `README.md` 或 `STATE.md`。

### Level 2：个人长期维护项目

在 Level 1 基础上，还需要：

- 真实可运行的验证命令。
- `STATE.md` 记录最近一轮状态。
- Level 2+ 的 agent 规则写入 `AGENTS.md`。

### Level 3：多人协作项目

在 Level 2 基础上，还需要：

- 协作规则。
- owner 或 reviewer。
- review / CI 信号。
- 交接记录。
- 明确哪些动作需要人类确认。

### Level 4：生产 / 高风险项目

默认不自动 loop。除非人类明确批准，否则不要让 agent 自动循环。

如果确实要设计 loop，必须写清：

- 风险边界。
- agent 权限边界。
- 人工批准门。
- rollback 或审计要求。
- 部署、数据、权限和安全相关的停止条件。
```

- [ ] **Step 2: Verify gates**

Run:

```bash
rg -n "Stop / Verify / Escalate|必须停止|必须升级给人类|Level 1|Level 2|Level 3|Level 4|默认不自动 loop" zh/loops/README.md
```

Expected: matches for Stop / Verify / Escalate and all project levels.

- [ ] **Step 3: Verify Level 4 and risky command language**

Run:

```bash
rg -n "生产|权限|认证|支付|敏感数据|安全|合规|migrate|外部服务|rollback|审计" zh/loops/README.md
```

Expected: matches for high-risk conditions.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/loops/README.md
```

Expected: no output.

- [ ] **Step 5: Commit gates**

Run:

```bash
git add zh/loops/README.md
git commit -m "docs: add loop safety gates"
```

Expected: commit succeeds.

## Task 4: Loop Design Template, Judgment Prompts, And Related Links

**Files:**
- Modify: `zh/loops/README.md`

- [ ] **Step 1: Append loop design template**

Append this content to `zh/loops/README.md`:

````markdown

## Loop Design Template

复制下面的模板到项目的 `README.md`、`STATE.md`、`AGENTS.md` 或任务说明中。

```markdown
## Loop Design

- Loop goal:
- Loop type:
- Project level:
- Allowed actions:
- Forbidden actions:
- Verification signal:
- Stop conditions:
- Escalation conditions:
- Maximum iterations:
- Human confirmation gates:
- State record location:
- Owner or reviewer:
```

填写要求：

- `Loop goal` 必须是一个可验证的目标。
- `Allowed actions` 必须足够小，不能让 agent 自行扩大范围。
- `Forbidden actions` 必须写清生产、权限、认证、支付、敏感数据、安全和部署边界。
- `Verification signal` 必须来自真实命令、真实 review / CI 结果，或人类确认过的 checklist。
- `Maximum iterations` 必须是具体数字。
- `Human confirmation gates` 必须写清哪些动作执行前需要确认。

## 判断 prompt

这些 prompt 只用于判断和设计 loop，不用于执行 loop。

### Claude Code

```text
请阅读当前 repo 的 README、STATE、可选的 SPEC/AGENTS，以及相关脚本和验证命令。

请判断当前任务是否适合设计成 loop。

要求：
- 先判断项目级别 Level 1-4。
- 区分任务执行 loop、开发反馈 loop、长期自动 loop。
- 如果目标、允许动作、验证信号、停止条件或升级条件不清楚，请建议不要 loop。
- 如果涉及生产、权限、认证、支付、敏感数据、安全或合规，请要求人类确认。
- 不要编造不存在的命令、测试、部署流程、团队规则或风险假设。
- 不要执行 loop，只输出判断和设计草案。

请输出：
- 是否建议 loop
- loop 类型
- 判断理由
- Stop / Verify / Escalate
- 最大轮数建议
- 需要人类确认的问题
```

### Codex CLI

```text
请基于当前 repo 文件和真实命令，判断这个任务是否适合 loop。

请先读取 README.md、STATE.md，以及如果存在的 SPEC.md、AGENTS.md。

要求：
- 先判断项目级别 Level 1-4。
- 只使用 repo 中真实存在的信息。
- 如果找不到真实验证信号，请明确说明，不要编造。
- 如果 loop 需要运行可能部署、migrate 数据、写文件、安装依赖、联系外部服务、改变认证或权限、触碰生产或敏感数据的命令，请停止并要求人类确认。
- 不要执行 loop。

请输出：
- 是否适合 loop
- 不适合时的原因
- 适合时的 loop 类型
- Stop / Verify / Escalate
- 最大轮数
- 人类确认门
```

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
````

- [ ] **Step 2: Verify template and prompts**

Run:

```bash
rg -n "Loop Design Template|判断 prompt|Claude Code|Codex CLI|不要执行 loop|Stop / Verify / Escalate|Maximum iterations|Human confirmation gates" zh/loops/README.md
```

Expected: matches for template, prompts, and non-execution guidance.

- [ ] **Step 3: Verify related docs exist**

Run:

```bash
for f in zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md zh/playbooks/start-a-new-project.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/loops/README.md
```

Expected: no output.

- [ ] **Step 5: Commit template and prompts**

Run:

```bash
git add zh/loops/README.md
git commit -m "docs: add loop design template and prompts"
```

Expected: commit succeeds.

## Task 5: Link Loops Module And Final Verification

**Files:**
- Modify: `README.zh.md`
- Verify: `zh/loops/README.md`

- [ ] **Step 1: Link loops module from `README.zh.md`**

Find this line:

```markdown
- `zh/loops/`：loop 是否该做、怎么设计、怎么停止。
```

Replace it with:

```markdown
- `zh/loops/`：loop 是否该做、怎么设计、怎么停止。loop 判断与设计见 [Loops](zh/loops/README.md)。
```

- [ ] **Step 2: Verify README link**

Run:

```bash
rg -n "zh/loops/README.md|Loops" README.zh.md
```

Expected: README.zh.md links to the loops README.

- [ ] **Step 3: Verify required loop sections**

Run:

```bash
rg -n "立场 / Default Stance|Loop 类型 / Loop Types|是否应该 loop / Should This Be a Loop|Stop / Verify / Escalate|按项目级别的 loop 门槛|Loop Design Template|判断 prompt|相关文档 / Related Docs" zh/loops/README.md
```

Expected: matches for all required sections.

- [ ] **Step 4: Verify no execution prompts accidentally run loops**

Run:

```bash
rg -n "不要执行 loop|不用于执行 loop|只用于判断和设计" zh/loops/README.md
```

Expected: matches that say judgment prompts do not execute loops.

- [ ] **Step 5: Verify related links point to files**

Run:

```bash
for f in zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md zh/playbooks/start-a-new-project.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 6: Verify no unresolved drafting markers**

Run:

```bash
rg -n "TBD|TODO|待定|正文不完整|草稿未清理|临时文本" zh/loops/README.md README.zh.md || true
```

Expected: no output.

- [ ] **Step 7: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/loops/README.md README.zh.md
```

Expected: no output.

- [ ] **Step 8: Commit README link**

Run:

```bash
git add zh/loops/README.md README.zh.md
git commit -m "docs: link loops module"
```

Expected: commit succeeds.

- [ ] **Step 9: Review recent commits**

Run:

```bash
git log --oneline -8
```

Expected: loops module commits use Conventional Commits messages beginning with `docs:`.
