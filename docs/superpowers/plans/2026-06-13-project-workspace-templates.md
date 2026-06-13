# Project Workspace Templates Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add the Chinese project workspace template set for the AI workflow handbook.

**Architecture:** The project template section uses an "index teaches, templates copy" structure. `zh/templates/project/README.md` explains selection by project level; the four `.template.md` files provide concise guidance and copyable document bodies for `README.md`, `SPEC.md`, `STATE.md`, and `AGENTS.md`.

**Tech Stack:** Markdown documentation, Git, Conventional Commits.

---

## File Structure

Create or modify these files:

```text
README.zh.md

zh/templates/project/README.md
zh/templates/project/README.template.md
zh/templates/project/SPEC.template.md
zh/templates/project/STATE.template.md
zh/templates/project/AGENTS.template.md
```

## Task 1: Project Templates Index

**Files:**
- Create: `zh/templates/project/README.md`

- [ ] **Step 1: Create the project templates directory**

Run:

```bash
mkdir -p zh/templates/project
```

Expected: directory exists.

- [ ] **Step 2: Write the project templates index**

Create `zh/templates/project/README.md` with this content:

```markdown
# Project Workspace Templates

这些模板用于给项目搭建最小可用的 AI-readable workspace。

它们不是强制标准，而是一组示例：帮助你判断哪些信息必须写进文件，哪些内容可以随着项目复杂度增加再补。

## 用途 / Purpose

Project workspace templates 解决三个问题：

- 让人和 agent 快速理解项目是什么。
- 让 agent 知道如何运行、修改和验证项目。
- 让项目状态、目标、规则和上下文沉淀在文件系统里，而不是散落在聊天记录中。

## 适用方式 / How to Use

1. 先阅读 `zh/guides/project-levels.md` 判断项目级别。
2. 再阅读 `zh/guides/infra-by-level.md` 确认当前项目最小需要哪些文件。
3. 从本目录复制对应模板到你的项目根目录。
4. 让 AI 阅读当前仓库后生成初稿，并要求它标出假设。
5. 人类 review 关键字段，尤其是验证命令、禁止操作和人类审批点。

## Level 1-4 推荐表

| 模板 | Level 1 | Level 2 | Level 3 | Level 4 |
| --- | --- | --- | --- | --- |
| `README.template.md` | 必需 | 必需 | 必需 | 必需 |
| `STATE.template.md` | 必需 | 必需 | 必需 | 必需 |
| `SPEC.template.md` | 可选 | 必需 | 必需 | 必需，需写清高风险约束 |
| `AGENTS.template.md` | 可选 | 必需 | 必需，需写清协作规则 | 必需，需写清权限边界和审批点 |

## 模板选择建议

- Level 1 项目先写 `README.md` 和 `STATE.md`；`SPEC.md` 和 `AGENTS.md` 保持可选，除非项目会长期维护或需要 agent 反复参与。
- Level 2 项目建议一次性补齐 `README.md`、`SPEC.md`、`STATE.md` 和 `AGENTS.md`。
- Level 3 项目的 `AGENTS.md` 必须说明 review、ownership、handoff 和协作边界。
- Level 4 项目的 `SPEC.md` 和 `AGENTS.md` 必须说明风险、权限、人类审批点和回滚要求。

## 让 AI 生成初稿

可以使用这段提示：

```text
请阅读当前仓库，判断这个项目大致属于 Level 1-4 的哪一级。
基于 zh/templates/project/ 下的模板，为当前项目生成该级别必需模板的初稿。
Level 1 通常先生成 README.md 和 STATE.md；只有在项目需要长期维护或 agent 反复参与时，才生成 SPEC.md 和 AGENTS.md。
请标出你做出的假设。
不要编造不存在的命令、测试、部署流程或团队规则。
如果缺少信息，请用“需要确认”标出，而不是自行补全。
```

## 常见错误 / Common Mistakes

- 把模板当成强制规范，而不是根据项目级别裁剪。
- 让 AI 编造验证命令，而不是先检查项目实际脚本。
- `STATE.md` 创建后长期不更新。
- `AGENTS.md` 写成通用 prompt，没有项目里的禁止操作和完成标准。
- Level 4 项目没有写人类审批点、权限边界和回滚要求。
```

- [ ] **Step 3: Verify the index headings**

Run:

```bash
rg -n "用途 / Purpose|Level 1-4 推荐表|让 AI 生成初稿|Common Mistakes" zh/templates/project/README.md
```

Expected: matches for all four phrases.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/templates/project/README.md
```

Expected: no output.

- [ ] **Step 5: Commit the project template index**

Run:

```bash
git add zh/templates/project/README.md
git commit -m "docs: add project templates index"
```

Expected: commit succeeds.

## Task 2: README And SPEC Templates

**Files:**
- Create: `zh/templates/project/README.template.md`
- Create: `zh/templates/project/SPEC.template.md`

- [ ] **Step 1: Write `README.template.md`**

Create `zh/templates/project/README.template.md` with this content:

`````markdown
# README Template

## 用途 / Purpose

作为项目入口，帮助人和 agent 快速理解项目用途、运行方式、验证方式和关键上下文入口。

## 推荐级别 / Recommended Levels

- Level 1-4：必需。

## 必填部分 / Required Sections

- 项目是什么
- 快速开始
- 常用命令
- 验证方式
- 相关文档

## 可选部分 / Optional Sections

- 适合谁使用
- 目录结构
- 当前状态
- 部署方式
- 贡献方式

## AI 生成提示 / AI Generation Prompt

```text
请阅读当前仓库，为这个项目生成 README.md 初稿。
必须基于实际文件和脚本填写快速开始、常用命令和验证方式。
请标出你做出的假设。
不要编造不存在的命令、服务、部署流程或文档链接。
```

## 常见错误 / Common Mistakes

- 只写安装命令，不写项目目标。
- 验证方式缺失，agent 无法判断修改是否正确。
- 链接到不存在的文档。
- 当前状态写得太细，导致 README 很快过时。

## 可复制模板 / Copyable Template

````markdown
# 项目名称

## 项目是什么

用 2-4 句话说明这个项目解决什么问题、主要产物是什么。

## 适合谁使用

- 使用者：
- 维护者：
- agent 参与方式：

## 快速开始

```bash
# 安装依赖

# 启动项目
```

## 常用命令

```bash
# 运行

# 测试

# lint

# build
```

## 验证方式

完成修改前至少运行：

```bash
# 写入当前项目真实存在的验证命令
```

预期结果：

- 所有检查通过，或记录失败命令和关键错误。

## 目录结构

```text
.
├── README.md
└── ...
```

## 当前状态

当前状态记录在 `STATE.md`。

## 相关文档

- `SPEC.md`：项目目标、范围和关键决策。
- `STATE.md`：当前状态、下一步和阻塞。
- `AGENTS.md`：agent 工作规则。
````
`````

- [ ] **Step 2: Write `SPEC.template.md`**

Create `zh/templates/project/SPEC.template.md` with this content:

````markdown
# SPEC Template

## 用途 / Purpose

记录项目目标、边界、约束和关键决策，减少反复解释产品方向和实现判断。

## 推荐级别 / Recommended Levels

- Level 1：可选。
- Level 2-3：必需。
- Level 4：必需，并需要明确安全、隐私、合规、生产或其他高风险约束。

## 必填部分 / Required Sections

- 背景
- 目标
- 非目标
- 核心功能
- 约束
- 成功标准
- 关键决策

## 可选部分 / Optional Sections

- 用户 / 使用场景
- 风险与开放问题
- 数据模型
- 权限边界
- 发布策略

## AI 生成提示 / AI Generation Prompt

```text
请阅读当前仓库和 README.md，为这个项目生成 SPEC.md 初稿。
请明确区分目标和非目标。
请标出你做出的假设。
如果仓库中没有证据支持某个功能、约束或成功标准，请写“需要确认”，不要编造。
不要编造不存在的命令、验证方式、功能、约束或成功标准。
```

## 常见错误 / Common Mistakes

- 只写愿景，不写非目标。
- 成功标准无法验证。
- 把实现细节当成产品目标。
- 高风险项目没有写权限、安全、隐私或回滚约束。

## 可复制模板 / Copyable Template

```markdown
# SPEC

## 背景

这个项目为什么存在？当前要解决什么问题？

## 目标

- 目标 1：

## 非目标

- 非目标 1：

## 用户 / 使用场景

- 用户：
- 使用场景：

## 核心功能

- 功能 1：

## 约束

- 技术约束：
- 时间 / 资源约束：
- 安全 / 隐私 / 合规约束：

## 成功标准

- 行为结果：
- 验证方式：
- 完成判定：
- 不满足时的处理：

## 关键决策

| 日期 | 决策 | 原因 |
| --- | --- | --- |
|  |  |  |

## 风险与开放问题

- 风险：
- 开放问题：
```
````

- [ ] **Step 3: Verify README and SPEC template sections**

Run:

```bash
rg -n "可复制模板 / Copyable Template|验证方式|非目标|成功标准" zh/templates/project/README.template.md zh/templates/project/SPEC.template.md
```

Expected: matches in both templates.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/templates/project/README.template.md zh/templates/project/SPEC.template.md
```

Expected: no output.

- [ ] **Step 5: Commit README and SPEC templates**

Run:

```bash
git add zh/templates/project/README.template.md zh/templates/project/SPEC.template.md
git commit -m "docs: add project readme and spec templates"
```

Expected: commit succeeds.

## Task 3: STATE And AGENTS Templates

**Files:**
- Create: `zh/templates/project/STATE.template.md`
- Create: `zh/templates/project/AGENTS.template.md`

- [ ] **Step 1: Write `STATE.template.md`**

Create `zh/templates/project/STATE.template.md` with this content:

````markdown
# STATE Template

## 用途 / Purpose

记录项目短期状态、下一步、阻塞和交接上下文。这个文件应该频繁更新，优先保证接力清晰。

## 推荐级别 / Recommended Levels

- Level 1-4：必需。

## 必填部分 / Required Sections

- 当前阶段
- 当前目标
- 下一步
- 阻塞
- 最近完成
- 给下一个 agent 的上下文

## 可选部分 / Optional Sections

- 最近决策
- 已知问题
- 当前分支 / PR
- 最近验证结果

## AI 生成提示 / AI Generation Prompt

```text
请阅读当前仓库、README.md、SPEC.md 和最近的 git diff，为项目生成 STATE.md 初稿。
请只记录当前状态、下一步、阻塞和交接上下文。
请标出你做出的假设。
不要把长期产品愿景塞进 STATE.md；长期目标应该放在 SPEC.md。
不要编造不存在的命令、验证结果、阻塞或决策。
```

## 常见错误 / Common Mistakes

- 创建后不更新。
- 把 STATE 写成第二份 README。
- 没有记录阻塞和下一步。
- 给下一个 agent 的上下文太含糊。

## 可复制模板 / Copyable Template

```markdown
# STATE

## 当前阶段

- 阶段名称：

## 当前目标

- 当前目标：

## 下一步

1. 下一步行动：
2. 负责人或前置条件：
3. 验证或交接要求：

## 阻塞

- 无

## 最近完成

- 已完成事项：

## 最近决策

| 日期 | 决策 | 原因 |
| --- | --- | --- |
|  |  |  |

## 已知问题

- 已知问题：

## 给下一个 agent 的上下文

- 当前应该优先处理：
- 不要重复做：
- 需要先确认：
- 最近验证结果：
```
````

- [ ] **Step 2: Write `AGENTS.template.md`**

Create `zh/templates/project/AGENTS.template.md` with this content:

`````markdown
# AGENTS Template

## 用途 / Purpose

定义 agent 在当前项目里的行为契约，包括工作原则、验证要求、禁止操作、完成标准和人类审批点。

## 推荐级别 / Recommended Levels

- Level 1：可选，可先写在 README 中。
- Level 2：必需。
- Level 3：必需，并需要写清协作、review、ownership 和 handoff 规则。
- Level 4：必需，并需要写清权限边界、人类审批点、风险操作和回滚要求。

## 必填部分 / Required Sections

- 项目上下文
- 工作原则
- 常用命令
- 验证要求
- 禁止操作
- 需要人类确认的操作
- 完成标准

## 可选部分 / Optional Sections

- 代码与文档约定
- Claude Code Notes
- Codex CLI Notes
- 多 agent 协作规则
- 发布 / 回滚规则

## AI 生成提示 / AI Generation Prompt

```text
请阅读当前仓库、README.md、SPEC.md、STATE.md 和可用脚本，为项目生成 AGENTS.md 初稿。
必须基于真实存在的命令填写常用命令和验证要求。
请标出你做出的假设。
不要编造不存在的测试、lint、部署流程或团队规则。
请明确写出禁止操作、需要人类确认的操作和完成标准。
```

## 常见错误 / Common Mistakes

- 写成通用 prompt，而不是项目规则。
- 没有真实验证命令。
- 没有禁止操作。
- 没有说明什么时候必须请求人类确认。
- Claude Code / Codex CLI 差异替代了主规则。

## 可复制模板 / Copyable Template

````markdown
# AGENTS

## 项目上下文

- 项目类型：
- 项目级别：
- 主要语言 / 框架：
- 关键目录：

## 工作原则

- 先读现有文档：`README.md`、`SPEC.md`、`STATE.md`。
- 修改前先理解当前任务的成功标准。
- 优先做小范围、可验证的改动。
- 不做与任务无关的重构。
- 结束前更新必要的文档或状态。

## 常用命令

```bash
# 安装依赖

# 运行

# 测试

# lint

# build
```

## 验证要求

完成任务前必须运行：

```bash
# 写入当前项目真实存在的验证命令
```

如果验证失败：

- 记录失败命令和关键错误。
- 判断是否由本次修改引入。
- 无法修复时停止并请求人类判断。

## 代码与文档约定

- 命名、格式或文档约定：

## 禁止操作

- 不要删除或覆盖用户未提交的改动。
- 不要编造不存在的命令、API、配置或文档。
- 不要在没有确认的情况下引入新依赖。
- 不要在没有确认的情况下执行破坏性 git 操作。

## 需要人类确认的操作

- 架构方向变化。
- 数据模型变化。
- 权限、安全、隐私或合规相关修改。
- 新依赖引入。
- 大范围重构。
- 发布、回滚或生产操作。

## 完成标准

- 目标行为已实现或文档已更新。
- 必要验证已运行，并记录结果。
- 关键 diff 已自查。
- 如有剩余风险，已明确写出。

## Claude Code Notes

- 可使用项目内的 CLAUDE.md 或相关 skill 作为补充规则。
- 对长流程任务，优先让 Claude Code 明确计划、验证命令和停止条件。

## Codex CLI Notes

- 使用 Codex CLI 时，优先让 agent 在 repo 内读文件、改文件、运行验证命令。
- 对多步骤任务，先生成 spec / plan，再执行。
- 结束前报告修改文件、验证结果和剩余风险。
````
`````

- [ ] **Step 3: Verify STATE and AGENTS template sections**

Run:

```bash
rg -n "给下一个 agent 的上下文|需要人类确认的操作|Claude Code Notes|Codex CLI Notes|禁止操作" zh/templates/project/STATE.template.md zh/templates/project/AGENTS.template.md
```

Expected: matches for the key sections.

- [ ] **Step 4: Run Markdown whitespace check**

Run:

```bash
git diff --check -- zh/templates/project/STATE.template.md zh/templates/project/AGENTS.template.md docs/superpowers/plans/2026-06-13-project-workspace-templates.md
```

Expected: no output.

- [ ] **Step 5: Commit STATE and AGENTS templates**

Run:

```bash
git add zh/templates/project/STATE.template.md zh/templates/project/AGENTS.template.md docs/superpowers/plans/2026-06-13-project-workspace-templates.md
git commit -m "docs: add project state and agents templates"
```

Expected: commit succeeds.

## Task 4: Link Templates From Chinese Entrypoint

**Files:**
- Modify: `README.zh.md`

- [ ] **Step 1: Update the templates entry in `README.zh.md`**

Find this line:

```markdown
- `zh/templates/`：纯文档模板示例。
```

Replace it with:

```markdown
- `zh/templates/`：纯文档模板示例。项目工作台模板见 [项目工作台模板](zh/templates/project/README.md)。
```

- [ ] **Step 2: Verify the link exists**

Run:

```bash
rg -n "项目工作台模板|zh/templates/project/README.md" README.zh.md
```

Expected: one match in the content map.

- [ ] **Step 3: Verify the linked file exists**

Run:

```bash
test -f zh/templates/project/README.md
```

Expected: command exits with status 0.

- [ ] **Step 4: Commit the entrypoint link**

Run:

```bash
git add README.zh.md
git commit -m "docs: link project templates from chinese entrypoint"
```

Expected: commit succeeds.

## Task 5: Final Verification

**Files:**
- Verify all files created or modified by Tasks 1-4.

- [ ] **Step 1: Verify all project template files exist**

Run:

```bash
for f in zh/templates/project/README.md zh/templates/project/README.template.md zh/templates/project/SPEC.template.md zh/templates/project/STATE.template.md zh/templates/project/AGENTS.template.md; do test -f "$f" || exit 1; done
```

Expected: command exits with status 0.

- [ ] **Step 2: Verify required sections exist**

Run:

```bash
rg -n "用途 / Purpose|推荐级别 / Recommended Levels|AI 生成提示 / AI Generation Prompt|可复制模板 / Copyable Template" zh/templates/project/*.template.md
```

Expected: matches in all four `.template.md` files.

- [ ] **Step 3: Verify no unfinished-marker text exists**

Run:

```bash
rg -n "占位内容|缺少正文|临时填充" zh/templates/project README.zh.md || true
```

Expected: no output.

- [ ] **Step 4: Verify README link**

Run:

```bash
test -f zh/templates/project/README.md && rg -n "zh/templates/project/README.md" README.zh.md
```

Expected: command exits with status 0 and prints the README link.

- [ ] **Step 5: Run Markdown whitespace check across the template implementation**

Run:

```bash
git diff --check HEAD~4..HEAD
```

Expected: no output.

- [ ] **Step 6: Review recent commit messages**

Run:

```bash
git log --oneline -6
```

Expected: project template implementation commits use Conventional Commits messages beginning with `docs:`.
