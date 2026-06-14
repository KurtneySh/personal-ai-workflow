# Cases Module Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add a lightweight Chinese `zh/cases/` module with an entrypoint and reusable case template.

**Architecture:** This is a pure documentation change. `zh/cases/README.md` explains the module purpose and boundaries; `zh/cases/CASE.template.md` provides a copyable structure for future real cases.

**Tech Stack:** Markdown documentation, existing `zh/` documentation structure, git verification commands.

---

## File Structure

- Create: `zh/cases/README.md`
  - Responsibility: explain what cases are for, what they should capture, what they are not, safety boundaries, suggested workflow, and related docs.
- Create: `zh/cases/CASE.template.md`
  - Responsibility: provide a structured template for recording real project cases.
- Do not modify: `README.zh.md`, English placeholder files, scripts, cookbook pages, or unrelated docs.

## Acceptance Checklist

- `zh/cases/README.md` exists.
- `zh/cases/CASE.template.md` exists.
- README explains cases as real project records, not tutorials or cookbook replacements.
- README explains that cases can be incomplete while work is ongoing.
- README includes safety guidance for secrets, private data, production details, and sensitive information.
- README links related existing modules.
- Template includes all required fields from the design spec.
- Template distinguishes agent actions from human decisions.
- Template includes verification, risks, failures/confusion, reusable patterns, non-reusable parts, and skill candidates.
- Template does not include a fictional example case.
- English stub files are unchanged.
- Related links point to existing files.
- `git diff --check` passes.

---

### Task 1: Add Cases Module README

**Files:**
- Create: `zh/cases/README.md`
- Reference: `docs/superpowers/specs/2026-06-13-cases-module-design.md`
- Reference: `README.zh.md`
- Reference: `zh/cookbook/README.md`
- Reference: `zh/skill-workflow/README.md`

- [ ] **Step 1: Read the design spec and related docs**

Run:

```bash
sed -n '1,240p' docs/superpowers/specs/2026-06-13-cases-module-design.md
sed -n '1,120p' README.zh.md
sed -n '1,160p' zh/cookbook/README.md
sed -n '1,220p' zh/skill-workflow/README.md
```

Expected:

```text
The design spec and related docs are readable. README.zh.md already lists zh/cases/ in the content map.
```

- [ ] **Step 2: Create `zh/cases/README.md`**

Write this complete file:

```markdown
# Cases

Cases 用于记录真实项目或真实任务中的 AI 开发工作流复盘。

它关注发生了什么、为什么这样判断、用了哪些 infra、agent 做了什么、人做了什么判断、验证信号是什么，以及哪些流程值得沉淀为 skill 候选。

## 用途 / Purpose

这个目录用于沉淀实战案例。

case 不是成功故事，也不是泛泛心得。一个好的 case 应该能帮助未来的自己或读者理解：当时的项目背景是什么、AI workflow 怎么被使用、哪些地方有效、哪些地方失败、哪些判断不能自动化。

case 可以在工作进行中保持不完整，但必须明确哪些信息已经验证，哪些仍然未知。

## 一个 Case 应该记录什么 / What A Case Should Capture

建议记录：

- 项目背景。
- 项目级别和判断依据。
- 初始目标和起点状态。
- 实际使用的 infra。
- 使用过的 playbooks、cookbook 场景、loops 或 skill workflow。
- agent 做了什么。
- 人做了什么判断。
- 关键决策和取舍。
- 验证信号，例如命令、commit、PR、CI、review 或人工 checklist。
- 风险、失败、阻塞和处理方式。
- 哪些模式可以复用。
- 哪些部分不应该复用。
- 哪些流程可能值得沉淀为 skill 候选。

## 一个 Case 不是什么 / What A Case Is Not

case 不是：

- 教程。
- cookbook 的重复改写。
- prompt 收藏。
- 只记录结果、不记录判断过程的总结。
- 把一次性经验包装成通用规则。
- 隐藏失败、人工介入或验证缺口的成功故事。
- 发布 secrets、credentials、private data、customer data、production details 或 internal credentials 的地方。

如果案例包含敏感信息，应该先脱敏或只记录抽象后的判断，不要写入可识别的私有细节。

## 建议流程 / Suggested Workflow

1. 先判断项目级别。
2. 记录初始目标和起点状态。
3. 记录实际使用的 infra、playbooks、cookbook 场景和 loop 设计。
4. 分开记录 agent actions 和 human decisions。
5. 记录真实验证信号；没有验证时明确写出来。
6. 记录风险、失败、stop/escalate 时刻和人的判断。
7. 复盘哪些模式可以复用，哪些不应该复用。
8. 如果出现重复流程，只提出 skill candidate，不自动创建或升级 skill。

## Case Template

复制 [CASE.template.md](CASE.template.md) 作为新案例的起点。

建议按真实项目或真实任务命名案例文件，例如：

```text
2026-06-13-my-project-bootstrap.md
2026-06-13-ci-feedback-loop.md
```

案例可以先是不完整草稿，但应该标明当前状态和缺失信息。

## Skill Capture Boundary

case 可以提出 skill 候选，但不能直接把候选视为已经采用的 skill。

当 case 里出现重复流程时，先回答：

- 这个模式出现过几次？
- 证据来自哪些任务、commit、PR、review 或验证结果？
- 它依赖当前项目，还是可以跨项目复用？
- 更适合项目级 skill，还是系统级 skill 候选？
- 有哪些反例或不适用场景？
- 需要谁 review？

具体判断见 [Skill Workflow](../skill-workflow/README.md)。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Cookbook](../cookbook/README.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
```

- [ ] **Step 3: Verify README required sections and safety guidance**

Run:

```bash
rg -n "^## (用途 / Purpose|一个 Case 应该记录什么 / What A Case Should Capture|一个 Case 不是什么 / What A Case Is Not|建议流程 / Suggested Workflow|Case Template|Skill Capture Boundary|相关文档 / Related Docs)" zh/cases/README.md
rg -n "真实项目|真实任务|不完整|secrets|credentials|private data|customer data|production details|internal credentials|Skill Workflow" zh/cases/README.md
```

Expected:

```text
All required headings are listed. Safety and real-case guidance terms are present.
```

- [ ] **Step 4: Verify related links exist**

Run:

```bash
test -f zh/guides/project-levels.md
test -f zh/guides/infra-by-level.md
test -f zh/guides/infra-checklists.md
test -f zh/playbooks/start-a-new-project.md
test -f zh/cookbook/README.md
test -f zh/loops/README.md
test -f zh/skill-workflow/README.md
```

Expected:

```text
All commands exit 0.
```

- [ ] **Step 5: Commit the README**

Run:

```bash
git diff --check
git add zh/cases/README.md
git commit -m "docs: add cases module"
```

Expected:

```text
git diff --check has no output.
Commit succeeds with message "docs: add cases module".
```

---

### Task 2: Add Case Template

**Files:**
- Create: `zh/cases/CASE.template.md`
- Reference: `docs/superpowers/specs/2026-06-13-cases-module-design.md`
- Reference: `zh/cases/README.md`

- [ ] **Step 1: Read the design spec and cases README**

Run:

```bash
sed -n '1,240p' docs/superpowers/specs/2026-06-13-cases-module-design.md
sed -n '1,220p' zh/cases/README.md
```

Expected:

```text
The spec and README are readable. The template should support real cases and avoid fictional example content.
```

- [ ] **Step 2: Create `zh/cases/CASE.template.md`**

Write this complete file:

```markdown
# Case: <case title>

> 这个模板用于记录真实项目或真实任务。不要把 secrets、credentials、private data、customer data、production details 或 internal credentials 写入案例。

## Metadata

- Date:
- Case owner:
- Current status:
- Related repo or project:
- Related task, issue, PR, or commit:

## Project Background

- Project background:
- Product or task context:
- Team or personal context:
- Constraints:

## Project Level

- Level:
- Why this level:
- Risk notes:
- Human confirmation gates:

## Initial Goal

- Initial goal:
- Success criteria:
- Non-goals:
- Deadline or timebox:

## Starting State

- Starting repo state:
- Existing docs or infra:
- Known blockers:
- Unknowns:

## Infra Used

- README / STATE / SPEC / AGENTS:
- Project templates:
- Verification commands:
- CI or review process:
- Worktree or branch strategy:
- Other infra:

## Playbooks Used

- Playbooks:
- Why these playbooks:
- What changed after using them:

## Cookbook Scenarios Used

- Cookbook scenarios:
- Why these scenarios:
- What worked:
- What did not work:

## Loop Usage

- Was a loop used:
- Loop type:
- Goal:
- Allowed actions:
- Forbidden actions:
- Verification signal:
- Stop conditions:
- Escalation conditions:
- Maximum rounds:
- Result:

## Skill Workflow Usage

- Was skill workflow considered:
- Repeated pattern observed:
- Project-level skill candidate:
- System-level skill candidate:
- Evidence:
- Human review needed:

## Agent Actions

Record what agents actually did.

- Agent or tool:
- Assigned task:
- Files or areas touched:
- Commands run:
- Output or result:
- Verification reported:
- Residual risks reported:

## Human Decisions

Record what humans decided.

- Decision:
- Context:
- Alternatives considered:
- Why this decision:
- Evidence used:
- Impact:

## Key Decisions

- Decision:
- Owner:
- Date:
- Evidence:
- Reversible:
- Follow-up:

## Verification Signals

- Command, CI, review, or checklist:
- Result:
- Evidence link or commit:
- What this verified:
- What this did not verify:

## Risks And Handling

- Risk:
- Severity:
- How it appeared:
- Handling:
- Stop or escalate decision:
- Remaining risk:

## What Worked

- Pattern:
- Evidence:
- Why it worked:
- Reuse conditions:

## What Failed Or Was Confusing

- Problem:
- Evidence:
- Cause or hypothesis:
- Human intervention:
- What to change next time:

## Skill Candidates

- Candidate name:
- Project-level or system-level candidate:
- Repeated pattern:
- Evidence:
- Required inputs:
- Verification:
- Not applicable when:
- Review owner:

## Reusable Patterns

- Pattern:
- Where it applies:
- Required context:
- Verification:
- Limits:

## Non-Reusable Parts

- Part:
- Why not reusable:
- Risk of overgeneralizing:
- What to do instead:

## Follow-Up Actions

- Action:
- Owner:
- Priority:
- Due date or review point:
- Status:
```

- [ ] **Step 3: Verify required template fields**

Run:

```bash
rg -n "^## (Metadata|Project Background|Project Level|Initial Goal|Starting State|Infra Used|Playbooks Used|Cookbook Scenarios Used|Loop Usage|Skill Workflow Usage|Agent Actions|Human Decisions|Key Decisions|Verification Signals|Risks And Handling|What Worked|What Failed Or Was Confusing|Skill Candidates|Reusable Patterns|Non-Reusable Parts|Follow-Up Actions)" zh/cases/CASE.template.md
rg -n "Date|Project background|Level|Initial goal|Starting repo state|Infra Used|Playbooks|Cookbook scenarios|Loop|Skill|Agent Actions|Human Decisions|Verification Signals|Risks|What Failed|Skill Candidates|Reusable Patterns|Non-Reusable Parts|Follow-Up Actions" zh/cases/CASE.template.md
```

Expected:

```text
All required template sections and key fields are listed.
```

- [ ] **Step 4: Verify no fictional example case**

Run:

```bash
rg -n "Example|示例案例|Acme|Foo|Bar|my-project|ci-feedback-loop|lorem|虚构" zh/cases/CASE.template.md zh/cases/README.md || true
```

Expected:

```text
No fictional case content. Filename examples in README are allowed only if they are generic naming guidance, not filled case content.
```

- [ ] **Step 5: Commit the template**

Run:

```bash
git diff --check
git add zh/cases/CASE.template.md
git commit -m "docs: add case template"
```

Expected:

```text
git diff --check has no output.
Commit succeeds with message "docs: add case template".
```

---

### Final Verification

- [ ] **Step 1: Verify no placeholders or plan leakage**

Run:

```bash
rg -n "TBD|TODO|待定|占位|implement later|fill in" zh/cases/README.md zh/cases/CASE.template.md
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

- [ ] **Step 3: Verify only expected files changed**

Run:

```bash
git diff --name-status HEAD~2..HEAD
```

Expected:

```text
A	zh/cases/CASE.template.md
A	zh/cases/README.md
```

- [ ] **Step 4: Verify git status**

Run:

```bash
git status --short --branch
```

Expected:

```text
Only expected branch-ahead status or clean status remains. There are no uncommitted changes.
```
