# Skill Workflow Module Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the first Chinese `zh/skill-workflow/README.md` entrypoint for deciding when AI development workflows should become reusable project skills or system skill candidates.

**Architecture:** Create one focused Markdown document that starts with a conservative default stance, defines project skills and system skills, then gives capture criteria, human/agent/loop boundaries, two copyable templates, prompt guidance, maintenance rules, and related links. Keep English content as existing stub files only; Chinese remains the source of truth.

**Tech Stack:** Markdown documentation, Git, ripgrep validation, Conventional Commits.

---

## File Structure

Create or modify these files:

```text
zh/skill-workflow/README.md
README.zh.md
```

Do not modify English stub files, project templates, or repository maintenance records in this plan.

## Task 1: Skill Workflow README Shell And Default Stance

**Files:**
- Create: `zh/skill-workflow/README.md`

- [ ] **Step 1: Create the skill workflow directory**

Run:

```bash
mkdir -p zh/skill-workflow
```

Expected: directory exists.

- [ ] **Step 2: Create the document shell**

Create `zh/skill-workflow/README.md` with this content:

```markdown
# Skill Workflow

Skill 是把反复出现的 AI 开发流程沉淀成可复用能力的方式。

这个目录的默认立场是：不要急着把所有重复动作都做成 skill。先把稳定、可验证、边界清楚的流程沉淀为项目级 skill；只有当它跨任务或跨项目反复有效时，才考虑升级为系统级 skill 候选。

## 立场 / Default Stance

- 默认不急着创建 skill。
- 先判断重复流程是否稳定、可教、可验证。
- 还依赖当前 repo、团队规则、命令或风险边界时，优先沉淀为项目级 skill。
- 只有经过多次真实使用，并且可以脱离单个项目上下文时，才考虑系统级 skill 候选。
- agent 可以提出 skill 候选，但不能自动创建、安装或升级系统级 skill。
- 系统级 skill 必须经过人类 review，因为它会影响未来多个项目里的 agent 行为。
```

- [ ] **Step 3: Verify shell sections**

Run:

```bash
rg -n "# Skill Workflow|立场 / Default Stance|默认不急着创建 skill|项目级 skill|系统级 skill" zh/skill-workflow/README.md
```

Expected: matches for the title, default stance, and both skill levels.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/skill-workflow/README.md
```

Expected: no output.

- [ ] **Step 5: Commit the shell**

Run:

```bash
git add zh/skill-workflow/README.md
git commit -m "docs: add skill workflow module shell"
```

Expected: commit succeeds.

## Task 2: Skill Definitions, Candidate Sources, And Capture Criteria

**Files:**
- Modify: `zh/skill-workflow/README.md`

- [ ] **Step 1: Append definitions and candidate sources**

Append this content to `zh/skill-workflow/README.md`:

```markdown

## 什么是 skill / What Is A Skill

这里的 skill 不是简单的一段 prompt，而是一种可复用的 AI 工作能力：

- 识别什么时候该使用这个流程。
- 知道需要哪些输入和上下文。
- 按固定步骤执行。
- 遵守动作边界和禁止事项。
- 用真实信号验证结果。
- 知道什么时候停止或交回人类。

### 项目级 skill

项目级 skill 是绑定在一个项目里的复用流程或规则。

它可以写在：

- `AGENTS.md`
- `STATE.md`
- 项目 runbook
- checklist
- 任务模板
- 未来的项目本地 skill 文件

适合项目级 skill 的场景：

- 流程依赖当前 repo 的命令、目录、架构或约定。
- 流程依赖当前团队的 review、交付或协作规则。
- 流程涉及当前项目特有的风险边界。
- 流程还没有证明可以迁移到其他项目。

项目级 skill 不是低级版本。很多流程停留在项目级就是正确选择。

### 系统级 skill

系统级 skill 是可以脱离单个项目存在的跨项目能力。

它类似正式的 `SKILL.md` 能力包，应该有：

- 稳定触发条件。
- 清晰适用范围。
- 明确不适用场景。
- 可靠验证方式。
- 示例和反例。
- 维护责任人。

系统级 skill 影响未来多个项目里的 agent 行为，所以只能作为候选提出，不能由 agent 自动升级。

## skill 候选从哪里来 / Candidate Sources

skill 候选通常来自四类信号：

- 人类反复协调同一类任务、反复写相似 prompt、反复纠正同类 agent 错误。
- loop 结束后，agent 发现重复步骤、重复验证规则或可复用恢复方式。
- 项目 infra 中已经稳定下来的规则，例如 `README.md`、`STATE.md`、`SPEC.md`、`AGENTS.md`、CI、review 约定或 runbook。
- 多个实战案例显示出同一种问题结构和处理方式。

## 什么时候值得沉淀 / When To Capture

一个流程适合成为 skill 候选，通常需要同时满足：

- 重复出现，不是一次性任务。
- 稳定到可以被描述。
- 输入、输出和边界清楚。
- 能教给 agent，而不是只靠人的临场判断。
- 有真实验证信号。
- 比每次临时写 prompt 更省协调成本。
- 对未来任务有复用价值。
- 能说清楚它不适用于哪些场景。

如果这些条件答不清楚，先不要做系统级 skill。可以继续保留在任务记录、项目 runbook、checklist 或项目级 skill 草稿里。
```

- [ ] **Step 2: Verify definitions and criteria**

Run:

```bash
rg -n "什么是 skill|项目级 skill|系统级 skill|skill 候选从哪里来|什么时候值得沉淀|不是一次性任务|真实验证信号" zh/skill-workflow/README.md
```

Expected: matches for definitions, sources, and capture criteria.

- [ ] **Step 3: Verify system skill caution**

Run:

```bash
rg -n "不能由 agent 自动升级|只能作为候选提出|不能自动创建、安装或升级系统级 skill" zh/skill-workflow/README.md
```

Expected: matches that prevent automatic system skill promotion.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/skill-workflow/README.md
```

Expected: no output.

- [ ] **Step 5: Commit definitions and criteria**

Run:

```bash
git add zh/skill-workflow/README.md
git commit -m "docs: define skill workflow criteria"
```

Expected: commit succeeds.

## Task 3: Project Skill vs System Skill And Human-Agent Boundaries

**Files:**
- Modify: `zh/skill-workflow/README.md`

- [ ] **Step 1: Append comparison and boundary guidance**

Append this content to `zh/skill-workflow/README.md`:

```markdown

## 项目级还是系统级 / Project Skill vs System Skill

| 判断项 | 项目级 skill | 系统级 skill 候选 |
| --- | --- | --- |
| 适用范围 | 一个 repo、一个产品、一个团队或一个任务域 | 多个任务或多个项目 |
| 证据要求 | 当前项目内重复有效 | 跨任务或跨项目重复有效 |
| 存放位置 | `AGENTS.md`、`STATE.md`、runbook、checklist、任务模板 | 正式 skill 包或系统级 skill 候选文档 |
| owner | 项目 owner、reviewer 或维护者 | 明确的维护责任人 |
| 验证方式 | 当前项目的真实命令、review、CI 或 checklist | 不依赖单个项目的验证规则和示例 |
| 升级条件 | 已经能减少项目内重复协调 | 已经证明可脱离当前项目复用 |
| 退役条件 | 项目规则变化、触发条件消失、反复导致错误 | 不再跨项目有效、维护者缺失、触发条件不清 |

选择原则：

- 依赖当前 repo 命令、目录、架构、团队规则或风险边界时，选择项目级 skill。
- 已经在多个任务或项目中被验证，并且不依赖隐藏项目上下文时，才提出系统级 skill 候选。
- 不要因为一次有用，就把项目级 skill 升级为系统级 skill。

## 人、agent 与 loop 的边界 / Human, Agent, And Loop Boundaries

skill 判断取决于谁在观察重复。

### 人类主导

当人类在协调 agent 工作时，人类负责判断哪些流程反复出现、哪些 prompt 反复被写、哪些错误反复被纠正。

agent 可以帮助输出 skill 候选报告，但不替人类做最终判断。

候选报告应该回答：

- 重复模式是什么？
- 出现过几次？
- 当前证据来自哪里？
- 更适合项目级 skill 还是系统级 skill 候选？
- 需要什么验证？
- 不适用于哪些场景？

### loop 主导

如果任务已经被设计成 loop，并且 loop 有明确目标、动作边界、验证信号、停止条件和升级条件，agent 可以在 loop 结束时提出项目级 skill 草稿。

但是 agent 只能提出草稿，不能直接把草稿视为已采用的项目规则。人类 review 之后，才能把它写入 `AGENTS.md`、runbook、checklist 或任务模板。

### 系统级 skill 提升

系统级 skill 的提升必须由人类确认。

agent 可以做：

- 汇总证据。
- 起草系统级 skill 候选。
- 标出适用范围、非目标、失败模式和验证方式。

agent 不可以做：

- 自动创建系统级 skill。
- 自动安装 skill。
- 自动把项目级 skill 提升为系统级 skill。
- 在没有人类 review 的情况下改变未来项目的默认工作方式。
```

- [ ] **Step 2: Verify comparison and boundaries**

Run:

```bash
rg -n "Project Skill vs System Skill|人、agent 与 loop 的边界|人类主导|loop 主导|系统级 skill 提升|自动安装 skill" zh/skill-workflow/README.md
```

Expected: matches for comparison table, human-led flow, loop-led flow, and system-skill limits.

- [ ] **Step 3: Verify loop wording does not duplicate loop design rules**

Run:

```bash
rg -n "明确目标、动作边界、验证信号、停止条件和升级条件|zh/loops|loop 结束" zh/skill-workflow/README.md
```

Expected: matches only concise references to loop prerequisites and loop completion.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/skill-workflow/README.md
```

Expected: no output.

- [ ] **Step 5: Commit boundary guidance**

Run:

```bash
git add zh/skill-workflow/README.md
git commit -m "docs: clarify skill workflow boundaries"
```

Expected: commit succeeds.

## Task 4: Skill Templates

**Files:**
- Modify: `zh/skill-workflow/README.md`

- [ ] **Step 1: Append project and system skill templates**

Append this content to `zh/skill-workflow/README.md`:

````markdown

## 模板 / Templates

这些模板是纯文档模板，可以复制到项目的 `AGENTS.md`、`STATE.md`、runbook、checklist 或任务说明里。

### Project Skill Template

```markdown
## Project Skill: <name>

- Skill name:
- Trigger:
- Project context:
- Allowed actions:
- Forbidden actions:
- Inputs:
- Steps:
- Verification:
- State updates:
- Owner or reviewer:
- Examples:
- Not applicable when:
- Last reviewed:
```

填写要求：

- `Trigger` 写清什么时候应该使用这个项目级 skill。
- `Project context` 写清它依赖哪些 repo 约定、命令、目录、风险边界或团队规则。
- `Allowed actions` 和 `Forbidden actions` 必须足够具体，不能让 agent 自行扩大范围。
- `Verification` 必须指向真实命令、review、CI、人工 checklist 或明确的验证信号。
- `Not applicable when` 写清反例，避免 skill 被错误泛化。

### System Skill Candidate Template

```markdown
## System Skill Candidate: <name>

- Candidate name:
- Problem it solves:
- Evidence from repeated use:
- Scope:
- Non-goals:
- Required inputs:
- Workflow steps:
- Tool or context requirements:
- Verification:
- Failure modes:
- Examples:
- Non-examples:
- Maintenance owner:
- Promotion decision:
```

填写要求：

- `Evidence from repeated use` 必须说明这个能力在哪些任务或项目中反复有效。
- `Scope` 和 `Non-goals` 必须能防止过度泛化。
- `Verification` 不能只写“人工感觉正确”，要说明可观察的判断信号。
- `Failure modes` 写清什么情况下应该停止使用或交回人类。
- `Promotion decision` 只能记录候选状态、批准状态或拒绝原因，不能让 agent 自动批准。
````

- [ ] **Step 2: Verify templates**

Run:

```bash
rg -n "Project Skill Template|System Skill Candidate Template|Not applicable when|Promotion decision|Evidence from repeated use|不能让 agent 自动批准" zh/skill-workflow/README.md
```

Expected: matches for both templates and anti-overpromotion fields.

- [ ] **Step 3: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/skill-workflow/README.md
```

Expected: no output.

- [ ] **Step 4: Commit templates**

Run:

```bash
git add zh/skill-workflow/README.md
git commit -m "docs: add skill workflow templates"
```

Expected: commit succeeds.

## Task 5: Prompt Guidance, Maintenance, Related Links, And README Link

**Files:**
- Modify: `zh/skill-workflow/README.md`
- Modify: `README.zh.md`

- [ ] **Step 1: Append prompts, maintenance, and related docs**

Append this content to `zh/skill-workflow/README.md`:

````markdown

## 判断 prompt / Prompt Guidance

这些 prompt 用于判断和起草，不用于自动创建或安装系统级 skill。

### 人类主导：skill 候选报告

#### Claude Code

```text
请阅读当前 repo 的 README、STATE、可选的 SPEC/AGENTS、相关 runbook 和最近任务记录。

请判断当前反复出现的流程是否值得沉淀为 skill。

要求：
- 先说明重复模式是什么。
- 区分项目级 skill 和系统级 skill 候选。
- 列出证据来自哪些文件、任务或 review 反馈。
- 写清适用范围、边界、不适用场景和验证方式。
- 如果证据不足，请建议继续记录，不要创建 skill。
- 不要自动创建、安装或升级系统级 skill。

请输出：
- 是否建议沉淀
- 建议类型：项目级 skill / 系统级 skill 候选 / 暂不沉淀
- 判断理由
- 证据
- 风险和不适用场景
- 下一步建议
```

#### Codex CLI

```text
请基于当前 repo 文件和真实任务记录，判断一个重复流程是否值得沉淀为 skill。

请先读取 README.md、STATE.md，以及如果存在的 SPEC.md、AGENTS.md、runbook、checklist。

要求：
- 只使用 repo 中真实存在的信息。
- 判断它更适合项目级 skill、系统级 skill 候选，还是暂不沉淀。
- 如果找不到重复证据、验证信号或清晰边界，请明确说明。
- 不要自动创建、安装或升级系统级 skill。

请输出：
- 重复模式
- 推荐类型
- 证据
- 项目依赖
- 验证方式
- 不适用场景
- 需要人类确认的问题
```

### loop 主导：项目级 skill 草稿

#### Claude Code

```text
请基于刚完成的 loop 记录，判断是否出现了值得沉淀的项目级 skill。

要求：
- 只在 loop 已有明确目标、动作边界、验证信号、停止条件和升级条件时判断。
- 只起草项目级 skill，不要创建系统级 skill。
- 写清 trigger、project context、allowed actions、forbidden actions、verification 和 not applicable when。
- 如果重复证据不足，请输出“暂不沉淀”。
- 草稿必须等待人类 review 后才能写入项目规则。
```

#### Codex CLI

```text
请读取本次任务或 loop 的状态记录，判断是否应该起草一个项目级 skill。

要求：
- 只使用已有记录，不要编造发生过的步骤。
- 如果发现可复用模式，请按 Project Skill Template 输出草稿。
- 如果缺少重复证据、验证信号或边界，请说明暂不沉淀。
- 不要修改 AGENTS.md、STATE.md 或系统级 skill 目录，除非人类明确确认。
```

### 系统级 skill 候选评估

#### Claude Code

```text
请评估一个项目级 skill 是否已经具备升级为系统级 skill 候选的条件。

要求：
- 检查是否有跨任务或跨项目的重复证据。
- 检查它是否能脱离当前 repo 的隐藏上下文。
- 写清 scope、non-goals、required inputs、verification、failure modes、examples 和 non-examples。
- 如果证据不足，请建议继续作为项目级 skill 维护。
- 不要创建、安装或升级系统级 skill，只输出候选评估。
```

#### Codex CLI

```text
请根据当前 repo 中的项目级 skill、任务记录和案例记录，评估它是否适合作为系统级 skill 候选。

要求：
- 只输出评估和候选草稿。
- 明确说明跨任务或跨项目证据。
- 明确说明哪些内容仍然依赖当前项目。
- 如果无法证明可迁移性，请建议不要升级。
- 不要写入系统级 skill 目录，不要安装 skill。
```

## 维护 / Maintenance

- 每次真实使用后，检查 skill 是否减少了重复协调。
- 当命令、项目规则、验证信号、权限边界或风险边界变化时，更新 skill。
- 当触发条件不再出现，或者 skill 反复导致错误行为时，退役它。
- 项目级 skill 的 owner 可以是项目维护者、reviewer 或负责该流程的人。
- 系统级 skill 候选必须记录维护责任人和升级决策。
- 保留足够上下文，让下一个人或 agent 能理解为什么这个 skill 存在。

## 相关文档 / Related Docs

- [Loops](../loops/README.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
````

- [ ] **Step 2: Link skill workflow module from `README.zh.md`**

Find this line:

```markdown
- `zh/skill-workflow/`：什么时候把流程沉淀成 skill。
```

Replace it with:

```markdown
- `zh/skill-workflow/`：什么时候把流程沉淀成 skill。skill 判断、项目级 skill 和系统级 skill 候选见 [Skill Workflow](zh/skill-workflow/README.md)。
```

- [ ] **Step 3: Verify prompt guidance**

Run:

```bash
rg -n "判断 prompt|人类主导：skill 候选报告|loop 主导：项目级 skill 草稿|系统级 skill 候选评估|不要创建、安装或升级系统级 skill|不要写入系统级 skill 目录" zh/skill-workflow/README.md
```

Expected: matches for all prompt scenarios and no-auto-upgrade guidance.

- [ ] **Step 4: Verify maintenance and related docs**

Run:

```bash
rg -n "维护 / Maintenance|真实使用后|退役|相关文档 / Related Docs|../loops/README.md|../playbooks/start-a-new-project.md|../guides/project-levels.md" zh/skill-workflow/README.md
```

Expected: matches for maintenance rules and related links.

- [ ] **Step 5: Verify README link**

Run:

```bash
rg -n "Skill Workflow|zh/skill-workflow/README.md" README.zh.md
```

Expected: README.zh.md links to the skill workflow README.

- [ ] **Step 6: Verify related links point to files**

Run:

```bash
for f in zh/loops/README.md zh/playbooks/start-a-new-project.md zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 7: Verify no unresolved drafting markers**

Run:

```bash
rg -n "TBD|TODO|待定|正文不完整|草稿未清理|临时文本" zh/skill-workflow/README.md README.zh.md || true
```

Expected: no output.

- [ ] **Step 8: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/skill-workflow/README.md README.zh.md
```

Expected: no output.

- [ ] **Step 9: Commit prompts, maintenance, and README link**

Run:

```bash
git add zh/skill-workflow/README.md README.zh.md
git commit -m "docs: link skill workflow module"
```

Expected: commit succeeds.

## Task 6: Final Acceptance Verification

**Files:**
- Verify: `zh/skill-workflow/README.md`
- Verify: `README.zh.md`

- [ ] **Step 1: Verify all required top-level sections**

Run:

```bash
rg -n "立场 / Default Stance|什么是 skill / What Is A Skill|skill 候选从哪里来 / Candidate Sources|什么时候值得沉淀 / When To Capture|项目级还是系统级 / Project Skill vs System Skill|人、agent 与 loop 的边界|模板 / Templates|判断 prompt / Prompt Guidance|维护 / Maintenance|相关文档 / Related Docs" zh/skill-workflow/README.md
```

Expected: matches for all required sections.

- [ ] **Step 2: Verify acceptance criteria language**

Run:

```bash
rg -n "项目级 skill|系统级 skill|系统级 skill 候选|默认不急着创建 skill|人类主导|loop 主导|Project Skill Template|System Skill Candidate Template|Not applicable when|Non-examples|不要自动创建、安装或升级系统级 skill" zh/skill-workflow/README.md
```

Expected: matches for definitions, default stance, boundaries, templates, and no-auto-upgrade rules.

- [ ] **Step 3: Verify negative cases are embedded in criteria and templates**

Run:

```bash
rg -n "不是一次性任务|不适用场景|Not applicable when|Non-goals|Failure modes|Non-examples|暂不沉淀" zh/skill-workflow/README.md
```

Expected: matches for negative-case fields and guidance.

- [ ] **Step 4: Verify no English files changed**

Run:

```bash
git status --short en
```

Expected: no output.

- [ ] **Step 5: Run final Markdown whitespace check**

Run:

```bash
git diff --check
```

Expected: no output.

- [ ] **Step 6: Review recent commits**

Run:

```bash
git log --oneline -8
```

Expected: skill workflow module commits use Conventional Commits messages beginning with `docs:`.
