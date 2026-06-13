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

## 路径 A：从空仓库开始

### A1. 写一句项目目标

- Input: 一个项目想法。
- Action: 用一句话写清这个项目要解决的问题。
- Output: 一句项目目标。
- Verification: 这句话能回答“这个项目为什么存在”。
- AI Prompt:

```text
我准备创建一个新项目。请根据下面的想法，帮我压缩成一句项目目标。
不要扩展功能范围，不要补充我没有提到的需求。

项目想法：
<写下你的想法>
```

### A2. 判断项目级别

- Input: 一句项目目标；是否长期维护、是否多人协作、是否涉及生产或敏感风险。
- Action: 阅读 `zh/guides/project-levels.md`；判断 Level 1-4。
- Output: 项目级别、判断理由、需要人类确认的问题。
- Verification: 如果可能涉及 Level 4，必须停下来让人类确认。
- AI Prompt:

```text
请阅读 `zh/guides/project-levels.md`，根据下面信息判断项目属于 Level 1-4。

输入：
- 项目目标：<一句项目目标>
- 是否长期维护：<是/否/不确定>
- 是否多人协作：<是/否/不确定>
- 是否涉及生产或敏感风险：<是/否/不确定>

请输出：
- 建议项目级别：Level 1/2/3/4
- 判断理由
- 需要人类确认的问题

如果项目可能涉及生产、权限、认证、支付、敏感数据、安全或合规，请停下来要求人类确认。
不要假设风险不存在；信息不足时写“需要确认”。
```

### A3. 选择最小 infra

- Input: 项目级别。
- Action: 阅读 `zh/guides/infra-by-level.md`；列出当前级别最小需要的文件或目录。
- Output: 本项目需要创建的 workspace 文件和目录清单。
- Verification: 清单覆盖对应级别最低要求；Level 1 至少包含 `README.md`、`STATE.md` 和一个验证命令；Level 2 至少再包含 `SPEC.md`、`AGENTS.md`、`docs/decisions/` 和测试或明确验证脚本；Level 3/4 按指南和项目风险补充协作、CI、交接、风险、部署、审计和权限边界材料。
- AI Prompt:

```text
请阅读 `zh/guides/infra-by-level.md`，根据项目级别列出当前最小需要创建的 workspace 文件或目录。

项目级别：<Level 1/2/3/4>

请按项目级别判断：
- Level 1：`README.md`、`STATE.md`、至少一个验证命令。
- Level 2：Level 1 全部，加 `SPEC.md`、`AGENTS.md`、`docs/decisions/`、tests 或明确验证脚本。
- Level 3：Level 2 全部，按需补充协作、ownership、review、CI、任务模板、交接和 multi-agent contract 相关材料。
- Level 4：Level 3 全部，按需补充风险、安全、隐私、rollback、human approval、部署、审计、loop stop 和权限边界相关材料。

只列出最小 infra，不要添加不必要的文件，也不要为了满足级别而机械创建所有可选文件。
请输出文件清单、用途，以及哪些项目需要人类确认。
```

### A4. 复制 project templates

- Input: 需要创建的 workspace 文件清单。
- Action: 从 `zh/templates/project/` 复制对应模板内容；根据项目级别裁剪可选部分。
- Output: `README.md`, `STATE.md`, Level 2+ `SPEC.md`, Level 2+ `AGENTS.md`.
- Verification: 每个文件都有真实项目上下文；没有保留空泛待填文本。
- AI Prompt:

```text
请根据 `zh/templates/project/` 中的对应模板，为这个项目生成 workspace 文件初稿。

需要创建的文件：
<文件清单>

项目上下文：
<项目目标、项目级别、已知范围、已知约束>

要求：
- 从模板内容出发，根据项目级别裁剪可选部分。
- 每个文件都必须包含真实项目上下文。
- 对缺失信息写“需要确认”。
- 明确标注你的假设。
- 不要编造命令、测试、部署流程或团队规则。
```

### A5. 人类 review 关键字段

- Input: AI 生成的 workspace 文件初稿。
- Action: 人类 review 项目目标、范围、验证命令、禁止操作和完成标准。
- Output: 被人类确认过的 workspace 文件。
- Verification: 命令和规则来自真实项目或明确的人类判断；AI 没有编造测试、部署流程或团队规则。
- AI Prompt:

```text
请只生成一份 review checklist，不要修改文件。

请重点检查：
- 项目目标是否准确
- 范围是否过大或过小
- 命令、测试和部署流程是否来自真实项目
- 团队规则是否来自明确的人类判断
- 禁止操作是否清楚
- 完成标准是否可验证

如果发现 AI 编造了命令、测试、部署流程或团队规则，请标记为需要人类确认。
```

### A6. 建立至少一个验证命令

- Input: 当前项目文件；可用脚本或工具链。
- Action: 检查真实项目文件、脚本和工具链；如果存在真实命令，运行最小必要验证命令；在得到人类确认后，把命令和结果写入 README/STATE/AGENTS。
- Output: 一个真实可运行的验证命令及其运行结果，或明确说明找不到真实可运行的验证命令。
- Verification: 如果存在真实命令，命令实际运行过且结果被记录；如果不存在，已明确记录找不到真实可运行的验证命令。
- AI Prompt:

```text
请检查当前项目文件、可用脚本和工具链，寻找一个真实可运行的验证命令。

如果存在，请输出：
- 命令
- 为什么这是最小必要验证命令
- 实际运行结果
- 建议写入 README/STATE/AGENTS 的内容

找到真实命令后，请先运行最小必要验证命令并报告结果。
只有在人类确认后，才把命令和结果写入 README/STATE/AGENTS；确认前请明确说明“等待人类确认后再编辑文件”。

如果找不到真实可运行的验证命令，请明确说明“找不到真实可运行的验证命令”。
不要编造不存在的命令、脚本、测试或工具链。
```

### A7. 更新 STATE 并做 smoke test

- Input: 已确认的 workspace 文件；验证命令结果。
- Action: 先阅读 workspace 文件，复述状态并提出低风险 smoke test，等待人类确认；确认后执行 smoke test，并更新 `STATE.md` 的当前阶段、下一步、阻塞问题和最近验证结果。
- Output: 准备阶段输出当前状态、下一步、阻塞问题和 smoke test 建议；确认执行后输出可交接的 `STATE.md` 和一次低风险 smoke test 结果。
- Verification: AI 助手能复述项目状态和下一步，不需要再次询问基础上下文；smoke test 结果已记录到 `STATE.md`。
- AI Prompt:

```text
请阅读 README/STATE/SPEC/AGENTS，并基于这些文件复述当前项目状态。

请输出：
- 当前阶段
- 下一步
- 阻塞问题
- 最近一次验证结果
- 一个低风险 smoke test 小任务建议

第一阶段不要修改文件。请明确说明“等待人类确认后再执行 smoke test”。

人类确认后，再执行低风险 smoke test，并更新 STATE.md：
- 当前阶段
- 下一步
- 阻塞问题
- 最近一次验证结果
- smoke test 结果
```
