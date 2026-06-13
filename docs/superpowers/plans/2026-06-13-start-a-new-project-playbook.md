# Start a New Project Playbook Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the first executable Chinese playbook for setting up an AI-readable project workspace from an empty or existing repository.

**Architecture:** Create one focused playbook at `zh/playbooks/start-a-new-project.md`. The document uses a single-file, two-path structure: shared purpose/success/stop guidance first, then Path A for empty repositories, Path B for existing repository retrofit, followed by tool prompts, checklist, and related links.

**Tech Stack:** Markdown documentation, Git, Conventional Commits.

---

## File Structure

Create or modify these files:

```text
README.zh.md
zh/playbooks/start-a-new-project.md
```

## Task 1: Playbook Shell And Shared Guidance

**Files:**
- Create: `zh/playbooks/start-a-new-project.md`

- [ ] **Step 1: Create the playbooks directory**

Run:

```bash
mkdir -p zh/playbooks
```

Expected: directory exists.

- [ ] **Step 2: Write the playbook shell and shared guidance**

Create `zh/playbooks/start-a-new-project.md` with this content:

```markdown
# Start a New Project Playbook

这个 playbook 用于把一个项目变成 AI 可读、可执行、可验证、可接手的工作区。

它覆盖两条路径：

- 路径 A：从空仓库开始。
- 路径 B：改造已有项目。

## 用途 / Purpose

这个 playbook 帮你完成从“项目想法或现有仓库”到“agent 可以稳定参与开发”的第一轮搭建。

它连接这些材料：

- `zh/guides/project-levels.md`
- `zh/guides/infra-by-level.md`
- `zh/guides/infra-checklists.md`
- `zh/templates/project/README.md`

## 适用场景 / When to Use

适合使用：

- 新建一个个人项目或团队项目。
- 给已有项目补 AI workflow infra。
- 想让 Claude Code、Codex CLI 或其他 agentic 工具进入 repo 工作。
- 想减少每次任务前反复解释上下文的成本。

不适合直接使用：

- 已经进入 PR / CI 修复阶段的任务。
- 需要设计多 agent 分工的复杂任务。
- 主要目标是设计自动 loop 的任务。

这些场景应该使用后续 playbook 或 cookbook。

## 成功标准 / Success Criteria

完成后应该满足：

- `README.md` 已创建或更新。
- `STATE.md` 已创建或更新。
- Level 2+ 项目有 `SPEC.md`。
- Level 2+ 项目有 `AGENTS.md`。
- 至少一个真实验证命令可以运行。
- `STATE.md` 写清当前阶段、下一步和阻塞。
- `AGENTS.md` 写清验证要求、禁止操作和完成标准。
- 人类 review 过 AI 生成的命令、测试、部署步骤和团队规则。
- agent 能基于这些文件理解项目，并接手一个低风险小任务。

## 停止与升级条件 / Stop and Escalate

遇到以下情况，先停下来请求人类判断：

- 项目级别无法判断，尤其是不确定是否属于 Level 4。
- 项目涉及生产、权限、认证、支付、敏感数据、安全或合规。
- 找不到真实可运行的验证命令。
- AI 生成了不存在的命令、测试、部署流程或团队规则。
- `README.md`、`SPEC.md`、`STATE.md`、`AGENTS.md` 之间互相矛盾。
- 当前 repo 有大量未提交改动，而且本次改造会触碰同一批文件。
- 需要引入新依赖、改数据模型、改权限边界或做大范围重构。
- agent 无法解释项目当前状态或下一步。
```

- [ ] **Step 3: Verify shell sections**

Run:

```bash
rg -n "用途 / Purpose|适用场景 / When to Use|成功标准 / Success Criteria|停止与升级条件 / Stop and Escalate" zh/playbooks/start-a-new-project.md
```

Expected: matches for all four sections.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/playbooks/start-a-new-project.md
```

Expected: no output.

- [ ] **Step 5: Commit the shell**

Run:

```bash
git add zh/playbooks/start-a-new-project.md
git commit -m "docs: add start project playbook shell"
```

Expected: commit succeeds.

## Task 2: Path A Empty Repository Flow

**Files:**
- Modify: `zh/playbooks/start-a-new-project.md`

- [ ] **Step 1: Append Path A to the playbook**

Append this content to `zh/playbooks/start-a-new-project.md`:

````markdown

## 路径 A：从空仓库开始

### A1. 写一句项目目标

Input:

- 一个项目想法。

Action:

- 用一句话写清这个项目要解决的问题。

Output:

- 一句项目目标。

Verification:

- 这句话能回答“这个项目为什么存在”。

AI Prompt:

```text
我准备创建一个新项目。请根据下面的想法，帮我压缩成一句项目目标。
不要扩展功能范围，不要补充我没有提到的需求。

项目想法：
<写下你的想法>
```

### A2. 判断项目级别

Input:

- 一句项目目标。
- 是否长期维护、是否多人协作、是否涉及生产或敏感风险。

Action:

- 阅读 `zh/guides/project-levels.md`。
- 判断项目属于 Level 1-4。

Output:

- 项目级别。
- 判断理由。
- 需要人类确认的问题。

Verification:

- 如果可能涉及 Level 4，必须停下来让人类确认。

AI Prompt:

```text
请根据项目目标和以下信息判断项目级别 Level 1-4。
如果无法判断，或者可能涉及生产、权限、认证、支付、敏感数据、安全或合规，请明确要求人类确认。
不要自行假设风险不存在。

项目目标：
<项目目标>

已知信息：
<长期维护 / 多人协作 / 风险信息>
```

### A3. 选择最小 infra

Input:

- 项目级别。

Action:

- 阅读 `zh/guides/infra-by-level.md`。
- 列出当前级别最小需要的文件。

Output:

- 本项目需要创建的 workspace 文件清单。

Verification:

- Level 1 至少包含 `README.md` 和 `STATE.md`。
- Level 2+ 还需要 `SPEC.md` 和 `AGENTS.md`。

AI Prompt:

```text
请根据项目级别选择最小 AI workflow infra。
只列出该级别必需的文件和原因。
不要为了形式添加不需要的文件。

项目级别：
<Level>
```

### A4. 复制 project templates

Input:

- 需要创建的 workspace 文件清单。

Action:

- 从 `zh/templates/project/` 复制对应模板内容。
- 根据项目级别裁剪可选部分。

Output:

- `README.md`
- `STATE.md`
- Level 2+ 的 `SPEC.md`
- Level 2+ 的 `AGENTS.md`

Verification:

- 每个文件都有真实项目上下文。
- 没有保留空泛待填文本。

AI Prompt:

```text
请基于 zh/templates/project/ 下的模板，为当前新项目生成该级别必需文件的初稿。
请标出你做出的假设。
不要编造不存在的命令、测试、部署流程或团队规则。
如果缺少信息，请写“需要确认”。
```

### A5. 人类 review 关键字段

Input:

- AI 生成的 workspace 文件初稿。

Action:

- 人类 review 项目目标、范围、验证命令、禁止操作和完成标准。

Output:

- 被人类确认过的 workspace 文件。

Verification:

- 命令和规则来自真实项目或明确的人类判断。
- AI 没有编造测试、部署流程或团队规则。

AI Prompt:

```text
请列出这些文件中最需要人类确认的字段。
重点检查命令、测试、部署流程、团队规则、禁止操作和完成标准。
不要修改文件，只输出 review checklist。
```

### A6. 建立至少一个验证命令

Input:

- 当前项目文件。
- 可用脚本或工具链。

Action:

- 找到或定义一个可以运行的验证命令。
- 写入 `README.md`、`STATE.md` 或 `AGENTS.md`。

Output:

- 一个真实可运行的验证命令。

Verification:

- 命令实际运行过。
- 结果被记录。

AI Prompt:

```text
请检查当前项目是否有真实可运行的验证命令。
如果存在，请说明命令和预期结果。
如果不存在，请明确说找不到，不要编造。
```

### A7. 更新 STATE 并做 smoke test

Input:

- 已确认的 workspace 文件。
- 验证命令结果。

Action:

- 更新 `STATE.md`，写清当前阶段、下一步、阻塞和最近验证结果。
- 让 agent 基于新上下文执行一个低风险小任务。

Output:

- 可交接的 `STATE.md`。
- 一次低风险 smoke test 结果。

Verification:

- agent 能复述项目状态。
- agent 能说明下一步。
- agent 不需要重新询问基础上下文。

AI Prompt:

```text
请阅读 README.md、STATE.md、SPEC.md、AGENTS.md。
先复述当前项目状态、下一步和阻塞。
然后提出一个低风险 smoke test 任务。
不要修改文件，先等待确认。
```
````

- [ ] **Step 2: Verify Path A step structure**

Run:

```bash
rg -n "### A[1-7]|Input:|Action:|Output:|Verification:|AI Prompt:" zh/playbooks/start-a-new-project.md
```

Expected: matches for A1-A7 and the step fields.

- [ ] **Step 3: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/playbooks/start-a-new-project.md
```

Expected: no output.

- [ ] **Step 4: Commit Path A**

Run:

```bash
git add zh/playbooks/start-a-new-project.md
git commit -m "docs: add empty repo project start flow"
```

Expected: commit succeeds.

## Task 3: Path B Existing Repository Retrofit Flow

**Files:**
- Modify: `zh/playbooks/start-a-new-project.md`

- [ ] **Step 1: Append Path B to the playbook**

Append this content to `zh/playbooks/start-a-new-project.md`:

````markdown

## 路径 B：改造已有项目

### B1. 扫描现有 repo

Input:

- 一个已有仓库。

Action:

- 检查 README、脚本、测试、构建配置、文档和当前 git 状态。

Output:

- 项目现状摘要。
- 已存在 infra 清单。
- 风险或不确定项。

Verification:

- 摘要引用真实文件或命令。
- 不根据猜测补全项目结构。

AI Prompt:

```text
请扫描当前 repo，列出现有 README、脚本、测试、构建配置、文档和 git 状态。
不要修改文件。
不要编造不存在的命令或目录。
如果某类信息不存在，请明确说明。
```

### B2. 判断项目级别

Input:

- repo 扫描结果。
- 已知协作、生产、权限和数据风险。

Action:

- 阅读 `zh/guides/project-levels.md`。
- 判断项目属于 Level 1-4。

Output:

- 项目级别。
- 判断理由。
- 需要人类确认的问题。

Verification:

- 如果涉及生产、权限、认证、支付、敏感数据、安全或合规，必须停下来确认。

AI Prompt:

```text
请根据 repo 扫描结果判断项目级别 Level 1-4。
如果可能是 Level 4，请停止并列出需要人类确认的问题。
不要自行降低风险级别。
```

### B3. 盘点已有 infra 和缺失项

Input:

- 项目级别。
- repo 扫描结果。

Action:

- 对照 `zh/guides/infra-by-level.md` 和 `zh/guides/infra-checklists.md`。
- 列出已有项和缺失项。

Output:

- 已有 infra。
- 缺失 infra。
- 最小补齐方案。

Verification:

- Level 1 至少应该有 `README.md` 和 `STATE.md`。
- Level 2+ 应该有 `SPEC.md` 和 `AGENTS.md`。

AI Prompt:

```text
请根据当前项目级别，盘点已有 AI workflow infra 和缺失项。
只提出最小补齐方案。
不要建议与当前级别无关的流程或文件。
```

### B4. 基于现有证据补齐 workspace 文件

Input:

- 现有 repo 文档和代码。
- 缺失 infra 清单。

Action:

- 基于现有证据生成或更新 `README.md`、`STATE.md`、`SPEC.md`、`AGENTS.md`。
- 不确定的信息标记为“需要确认”。

Output:

- 补齐后的 workspace 文件。

Verification:

- 新内容能追溯到现有文件、命令或人类确认。
- 没有编造命令、测试、部署流程或团队规则。

AI Prompt:

```text
请基于当前 repo 的真实文件和命令，补齐缺失的 workspace 文件。
不确定的信息请写“需要确认”。
不要编造不存在的命令、测试、部署流程或团队规则。
```

### B5. 校验验证命令

Input:

- README、STATE、AGENTS 中记录的验证命令。

Action:

- 实际运行至少一个验证命令。
- 记录命令和结果。

Output:

- 验证命令结果。
- 如果失败，记录关键错误和判断。

Verification:

- 命令真实运行过。
- 失败时没有继续假装成功。

AI Prompt:

```text
请检查 README.md、STATE.md、AGENTS.md 中记录的验证命令。
运行最小必要验证，并报告命令、结果和关键错误。
如果命令不存在或失败，请明确说明，不要继续生成成功结论。
```

### B6. 人类 review 并更新 STATE

Input:

- 补齐后的 workspace 文件。
- 验证命令结果。

Action:

- 人类 review 目标、边界、禁止操作、完成标准和验证结果。
- 更新 `STATE.md` 的当前状态、下一步和阻塞。

Output:

- 经确认的 workspace 文件。
- 可交接的 `STATE.md`。

Verification:

- `STATE.md` 能让下一个 agent 接手。
- `AGENTS.md` 中有禁止操作和完成标准。

AI Prompt:

```text
请基于当前 README.md、SPEC.md、STATE.md、AGENTS.md 和验证结果，
列出需要人类确认的关键字段。
重点检查目标、边界、禁止操作、完成标准和验证命令。
不要修改文件，先输出 review checklist。
```

### B7. 让 agent 做低风险接手测试

Input:

- 经确认的 workspace 文件。

Action:

- 让 agent 复述项目状态。
- 让 agent 提出或执行一个低风险任务。

Output:

- agent 的项目理解摘要。
- 低风险任务结果。

Verification:

- agent 能说明项目级别、当前状态、下一步和验证方式。
- agent 没有重新询问基础上下文。

AI Prompt:

```text
请阅读 README.md、SPEC.md、STATE.md、AGENTS.md。
复述项目级别、当前状态、下一步、阻塞和验证方式。
然后提出一个低风险接手任务。
不要执行高风险操作。
```
````

- [ ] **Step 2: Verify Path B step structure**

Run:

```bash
rg -n "### B[1-7]|Input:|Action:|Output:|Verification:|AI Prompt:" zh/playbooks/start-a-new-project.md
```

Expected: matches for B1-B7 and the step fields.

- [ ] **Step 3: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/playbooks/start-a-new-project.md
```

Expected: no output.

- [ ] **Step 4: Commit Path B**

Run:

```bash
git add zh/playbooks/start-a-new-project.md
git commit -m "docs: add existing repo retrofit flow"
```

Expected: commit succeeds.

## Task 4: Tool Prompts, Checklist, Links, And Entrypoint

**Files:**
- Modify: `zh/playbooks/start-a-new-project.md`
- Modify: `README.zh.md`

- [ ] **Step 1: Append tool prompts, final checklist, and related docs**

Append this content to `zh/playbooks/start-a-new-project.md`:

```markdown

## 关键步骤的 Claude Code / Codex CLI 提示

### Repo 扫描

Claude Code:

```text
请先阅读当前 repo 的 README、脚本、测试、构建配置和文档。
不要修改文件。
请输出项目现状、可用验证命令和缺失的 AI workflow infra。
```

Codex CLI:

```text
请在当前 repo 内检查 README.md、SPEC.md、STATE.md、AGENTS.md 和可用脚本。
判断当前项目级别，列出缺失 infra，并给出最小补齐方案。
不要编造不存在的命令；如果找不到验证命令，请明确说明。
```

### 模板生成

Claude Code:

```text
请基于 zh/templates/project/ 下的模板，为当前项目生成该级别必需文件的初稿。
请标出假设和需要人类确认的问题。
不要编造不存在的命令、测试、部署流程或团队规则。
```

Codex CLI:

```text
请在当前 repo 中读取已有文件和脚本，
基于 zh/templates/project/ 下的模板补齐最小 workspace 文件。
只使用 repo 中真实存在的信息。
不确定的内容请写“需要确认”。
```

### 最终 smoke test

Claude Code:

```text
请阅读 README.md、SPEC.md、STATE.md、AGENTS.md。
复述项目级别、当前状态、下一步、阻塞和验证方式。
然后提出一个低风险接手任务。
不要修改文件，先等待确认。
```

Codex CLI:

```text
请基于当前 workspace 文件复述项目状态，并提出一个可以安全执行的小任务。
如果任务需要修改文件，请先说明计划、涉及文件和验证命令。
不要执行高风险操作。
```

## 最终检查清单 / Final Checklist

- [ ] 项目级别已判断
- [ ] `README.md` 已创建或更新
- [ ] `STATE.md` 已创建或更新
- [ ] Level 2+ 项目有 `SPEC.md`
- [ ] Level 2+ 项目有 `AGENTS.md`
- [ ] 验证命令真实可运行
- [ ] `STATE.md` 写清下一步和阻塞
- [ ] `AGENTS.md` 写清禁止操作和完成标准
- [ ] 人类 review 过 AI 生成的命令和规则
- [ ] agent 能复述项目状态并接手一个小任务

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [项目工作台模板](../templates/project/README.md)
```

- [ ] **Step 2: Link the playbook from `README.zh.md`**

Find this line:

```markdown
- `zh/playbooks/`：从目标到完成的端到端任务流程。
```

Replace it with:

```markdown
- `zh/playbooks/`：从目标到完成的端到端任务流程。新项目启动见 [Start a New Project Playbook](zh/playbooks/start-a-new-project.md)。
```

- [ ] **Step 3: Verify tool prompts and links**

Run:

```bash
rg -n "Claude Code|Codex CLI|Final Checklist|Related Docs|Start a New Project Playbook|zh/playbooks/start-a-new-project.md" zh/playbooks/start-a-new-project.md README.zh.md
```

Expected: matches for tool prompts, final checklist, related docs, and README link.

- [ ] **Step 4: Verify related docs exist**

Run:

```bash
for f in zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md zh/templates/project/README.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 5: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/playbooks/start-a-new-project.md README.zh.md
```

Expected: no output.

- [ ] **Step 6: Commit tool prompts and entrypoint link**

Run:

```bash
git add zh/playbooks/start-a-new-project.md README.zh.md
git commit -m "docs: link start project playbook"
```

Expected: commit succeeds.

## Task 5: Final Verification

**Files:**
- Verify: `zh/playbooks/start-a-new-project.md`
- Verify: `README.zh.md`

- [ ] **Step 1: Verify playbook file exists**

Run:

```bash
test -f zh/playbooks/start-a-new-project.md
```

Expected: command exits with status 0.

- [ ] **Step 2: Verify required top-level sections**

Run:

```bash
rg -n "用途 / Purpose|适用场景 / When to Use|成功标准 / Success Criteria|停止与升级条件 / Stop and Escalate|路径 A：从空仓库开始|路径 B：改造已有项目|Claude Code / Codex CLI|最终检查清单 / Final Checklist|相关文档 / Related Docs" zh/playbooks/start-a-new-project.md
```

Expected: matches for all required sections.

- [ ] **Step 3: Verify both paths have executable fields**

Run:

```bash
rg -n "### A[1-7]|### B[1-7]|Input:|Action:|Output:|Verification:|AI Prompt:" zh/playbooks/start-a-new-project.md
```

Expected: matches for both paths and all step fields.

- [ ] **Step 4: Verify stop conditions and anti-invention guidance**

Run:

```bash
rg -n "停止|升级|不要编造不存在的命令|找不到验证命令|Level 4|人类确认" zh/playbooks/start-a-new-project.md
```

Expected: matches for stop/escalate and anti-invention guidance.

- [ ] **Step 5: Verify related links point to files**

Run:

```bash
for f in zh/guides/project-levels.md zh/guides/infra-by-level.md zh/guides/infra-checklists.md zh/templates/project/README.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 6: Verify README entrypoint link**

Run:

```bash
test -f zh/playbooks/start-a-new-project.md && rg -n "zh/playbooks/start-a-new-project.md" README.zh.md
```

Expected: command exits with status 0 and prints the README link.

- [ ] **Step 7: Verify no unresolved drafting markers**

Run:

```bash
rg -n "空泛待填|正文不完整|草稿未清理|临时文本" zh/playbooks/start-a-new-project.md README.zh.md || true
```

Expected: no output.

- [ ] **Step 8: Run Markdown whitespace check**

Run:

```bash
git diff --check HEAD~4..HEAD
```

Expected: no output.

- [ ] **Step 9: Review recent commit messages**

Run:

```bash
git log --oneline -8
```

Expected: playbook implementation commits use Conventional Commits messages beginning with `docs:`.
