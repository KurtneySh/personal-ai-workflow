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
- Verification: 清单覆盖对应级别最低要求；Level 1 至少包含 `README.md`、`STATE.md` 和一个验证命令；Level 2 至少再包含 `SPEC.md`、`AGENTS.md`、`docs/decisions/` 和测试或明确验证脚本；Level 3 必须包含协作、CI、任务、交接和 multi-agent delegation contract 材料；Level 4 必须包含风险、安全、隐私、rollback、human approval、部署、审计、loop stop 和权限边界材料。
- AI Prompt:

```text
请阅读 `zh/guides/infra-by-level.md`，根据项目级别列出当前最小需要创建的 workspace 文件或目录。

项目级别：<Level 1/2/3/4>

请按项目级别判断：
- Level 1：`README.md`、`STATE.md`、至少一个验证命令。
- Level 2：Level 1 全部，加 `SPEC.md`、`AGENTS.md`、`docs/decisions/`、tests 或明确验证脚本。
- Level 3：Level 2 全部，加 `CONTRIBUTING.md`、`CODEOWNERS` 或 ownership 文档、PR checklist、CI checks、issue / task template、handoff notes、multi-agent delegation contract。
- Level 4：Level 3 全部，加 risk register、security / privacy checklist、rollback plan、human approval gates、deployment checklist、incident / audit notes、loop stop conditions、permission boundaries for agents。

只列出已选级别的最小 infra；项目特定可选补充另行处理，不要添加不必要的可选文件。
请输出文件清单、用途，以及哪些项目需要人类确认。
```

### A4. 复制项目模板

- Input: 需要创建的 workspace 文件清单。
- Action: 项目模板覆盖四个核心 workspace 文件：`README.md`、`STATE.md`、Level 2+ `SPEC.md`、Level 2+ `AGENTS.md`；A3 选出的其他最小 infra 项目应创建为最小 stub、连接到项目已有工具，或在缺少上下文时标记为 `需要确认`。
- Output: 已创建或标记为需要确认的最小 infra 文件和目录。
- Verification: A3 选出的完整最小 infra 清单被保留；每个已创建文件都有真实项目上下文；没有保留空泛待填文本。
- AI Prompt:

```text
请根据 `zh/templates/project/` 中的对应模板，为这个项目生成 workspace 文件初稿，并保留 A3 选出的完整最小 infra 清单。

需要创建的文件：
<文件清单>

项目上下文：
<项目目标、项目级别、已知范围、已知约束>

要求：
- 对四个核心 workspace 文件使用项目模板：`README.md`、`STATE.md`、Level 2+ `SPEC.md`、Level 2+ `AGENTS.md`。
- 对 A3 清单中没有项目模板的其他最小 infra 项目：只有在上下文足够时才创建最小 stub 或连接到项目已有工具；上下文不足时标记为“需要确认”。
- 保留 A3 选出的完整最小 infra，不要只输出核心 workspace 文件。
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
- Action: 检查真实项目文件、脚本和工具链；如果存在真实命令，只自动运行 read-only 或低风险本地最小必要验证命令；如果命令可能部署、migrate 数据、写文件、安装依赖、联系外部服务、改变认证/权限，或触碰生产/敏感数据，先停下来请求人类确认；在得到人类确认后，把命令和结果写入 README/STATE/AGENTS。
- Output: 一个真实可运行的验证命令及其运行结果，或明确说明找不到真实可运行的验证命令。
- Verification: 如果存在真实命令，只有 read-only 或低风险本地命令会自动运行并记录结果；高风险命令已在运行前等待人类确认；命令失败没有被当作成功；如果不存在，已明确记录找不到真实可运行的验证命令。
- AI Prompt:

```text
请检查当前项目文件、可用脚本和工具链，寻找一个真实可运行的验证命令。

如果存在，请输出：
- 命令
- 为什么这是最小必要验证命令
- 实际运行结果
- 建议写入 README/STATE/AGENTS 的内容

找到真实命令后，只能先自动运行 read-only 或低风险本地最小必要验证命令并报告结果。
如果命令可能部署、migrate 数据、写文件、安装依赖、联系外部服务、改变认证/权限，或触碰生产/敏感数据，请先停下来请求人类确认，不要擅自运行。
如果命令失败，不要声称验证成功；请如实报告失败结果和可能需要人类判断的下一步。
只有在人类确认后，才把命令和结果写入 README/STATE/AGENTS；确认前请明确说明“等待人类确认后再编辑文件”。

如果找不到真实可运行的验证命令，请明确说明“找不到真实可运行的验证命令”。
不要编造不存在的命令、脚本、测试或工具链。
```

### A7. 更新 STATE 并做 smoke test

- Input: 已确认的 workspace 文件；验证命令结果。
- Action: 先阅读已创建的 workspace 文件，复述项目状态，提出 read-only 或低风险 smoke test，并等待人类确认；确认后只执行并报告 smoke test 结果，不更新 `STATE.md`；再次确认后，才更新 `STATE.md` 的当前阶段、下一步、阻塞问题、最近验证结果和 smoke test 结果。
- Output: 第一阶段输出当前状态、下一步、阻塞问题和 smoke test 建议；第二阶段输出 smoke test 实际结果并等待再次确认；第三阶段输出可交接的 `STATE.md`。
- Verification: AI 助手能复述项目状态和下一步，不需要再次询问基础上下文；smoke test 已在更新 `STATE.md` 前单独报告；`STATE.md` 只在第二次人类确认后记录当前阶段、下一步、阻塞问题、最近验证结果和 smoke test 结果。
- AI Prompt:

```text
请阅读已创建的 workspace 文件，并基于这些文件复述当前项目状态。
Level 1 通常包括 README/STATE；Level 2+ 通常还包括 SPEC/AGENTS。

请输出：
- 当前阶段
- 下一步
- 阻塞问题
- 最近一次验证结果
- 一个 read-only 或低风险 smoke test 小任务建议

第一阶段不要修改文件。请明确说明“等待人类确认后再执行 smoke test”。
除非人类明确确认，否则 smoke test 必须是 read-only 或低风险操作。

人类第一次确认后，执行 read-only 或低风险 smoke test，并只报告结果；此时不要更新 STATE.md。
报告后请明确说明“等待再次确认后再更新 STATE.md”。

人类第二次确认后，才更新 STATE.md：
- 当前阶段
- 下一步
- 阻塞问题
- 最近一次验证结果
- smoke test 结果
```

## 路径 B：改造已有项目

### B1. 扫描现有 repo

- Input: 一个已有仓库。
- Action: 检查 README、脚本、测试、构建配置、文档和当前 git 状态。
- Output: 项目现状摘要；已存在 infra 清单；风险或不确定项。
- Verification: 摘要引用真实文件或命令；不根据猜测补全项目结构。
- AI Prompt:

```text
请扫描当前 repo，列出现有 README、脚本、测试、构建配置、文档和 git 状态。

要求：
- 不要修改文件。
- 不要编造命令、目录或项目结构。
- 只根据真实存在的文件和实际命令输出总结。
- 如果某类信息不存在或无法确认，请明确写“缺失”或“需要确认”。

请输出：
- 项目现状摘要
- 已存在 infra 清单
- 风险或不确定项
```

### B2. 判断项目级别

- Input: repo 扫描结果；已知协作、生产、权限和数据风险。
- Action: 阅读 `zh/guides/project-levels.md`；判断 Level 1-4。
- Output: 项目级别；判断理由；需要人类确认的问题。
- Verification: 如果存在生产、权限、认证、支付、敏感数据、安全或合规风险，必须停下来让人类确认。
- AI Prompt:

```text
请阅读 `zh/guides/project-levels.md`，基于 repo 扫描结果判断这个项目属于 Level 1-4。

输入：
- repo 扫描结果：<粘贴 B1 输出>
- 已知协作、生产、权限和数据风险：<粘贴已知信息>

要求：
- 明确说明判断理由。
- 如果可能涉及生产、权限、认证、支付、敏感数据、安全或合规风险，请停止并要求人类确认。
- 如果可能是 Level 4，请停止并要求人类确认。
- 不要自行降低风险级别。
- 不要用缺失信息替项目做低风险假设。

请输出：
- 项目级别
- 判断理由
- 需要人类确认的问题
```

### B3. 盘点已有 infra 和缺失项

- Input: 项目级别；repo 扫描结果。
- Action: 对照 `zh/guides/infra-by-level.md` 和 `zh/guides/infra-checklists.md`；列出已有和缺失项。
- Output: 已有 infra；缺失 infra；最小补齐方案。
- Verification: Level 1 至少包含 README、STATE 和验证命令；Level 2+ 至少包含 SPEC、AGENTS、docs/decisions/、tests 或明确验证脚本；Level 3/4 选择后必须满足 guide 中的最低 infra。
- AI Prompt:

```text
请阅读 `zh/guides/infra-by-level.md` 和 `zh/guides/infra-checklists.md`，基于当前项目级别和 repo 扫描结果盘点 AI workflow infra。

输入：
- 项目级别：<Level 1/2/3/4>
- repo 扫描结果：<粘贴 B1 输出>

要求：
- 列出现有 infra。
- 列出缺失 infra。
- 只提出当前级别的最小补齐方案。
- 不要建议无关文件、无关流程或超出当前级别的可选项。
- Level 1 至少需要 README、STATE 和一个验证命令。
- Level 2+ 至少需要 SPEC、AGENTS、docs/decisions/、tests 或明确验证脚本。
- Level 3/4 如果被选中，必须对照 guide 覆盖最低 infra。
- 如果某项无法从 repo 证据确认，请标记为“需要确认”。

请输出：
- 已有 infra
- 缺失 infra
- 最小补齐方案
```

### B4. 基于现有证据补齐 workspace 文件

- Input: 现有 repo 文档和代码；缺失 infra 清单。
- Action: 基于现有证据，按项目级别生成或更新 B3 最小补齐方案中的 workspace 文件；Level 1 通常只需要 `README.md`、`STATE.md` 和验证命令记录，不要无条件创建 `SPEC.md` 或 `AGENTS.md`；不确定信息写为 `需要确认`。
- Output: 补齐后的 workspace 文件和/或标记为 `需要确认` 的项目。
- Verification: 新内容可追溯到现有文件、命令或人类确认；修改任何已有文件前已经列出精确文件和证据来源，并得到人类明确确认；不编造命令、测试、部署流程或团队规则。
- AI Prompt:

```text
请基于真实 repo 文件、真实命令和已有人类确认信息，补齐缺失的 workspace 文件。

输入：
- 现有 repo 文档和代码摘要：<粘贴证据>
- 缺失 infra 清单：<粘贴 B3 输出>

要求：
- 按项目级别和 B3 的最小补齐方案生成或更新 workspace 文件；不要无条件创建或更新 `SPEC.md`、`AGENTS.md`。
- Level 1 不要假设一定存在 `SPEC.md` 或 `AGENTS.md`；除非人类明确要求或 B3 证据支持，否则只补齐 Level 1 所需的 `README.md`、`STATE.md` 和验证命令记录。
- 所有新增内容必须能追溯到现有文件、实际命令或人类确认。
- 不确定信息统一写为“需要确认”。
- 不要编造命令、测试、部署流程、团队规则、权限模型或完成标准。
- 修改前先列出将创建或更新的精确文件清单，并为每个文件列出证据来源。
- 如果将修改任何已有文件，尤其是 `README.md`、`STATE.md`、`SPEC.md`、`AGENTS.md` 或其他已有 infra 文件，必须先等待人类明确确认；等待人类确认前不要修改文件。

请输出：
- 将创建或更新的精确文件清单
- 每个文件的证据来源
- 明确说明“等待人类确认后再修改已有文件”
- 已补齐的 workspace 文件
- 仍需人类确认的项目
- 每项关键信息的证据来源
```

### B5. 校验验证命令

- Input: README、STATE、AGENTS 中记录的验证命令。
- Action: 只自动运行 read-only 或低风险本地验证命令；如果记录的命令可能部署、migrate 数据、写文件、安装依赖、联系外部服务、改变认证或权限，或触碰生产 / 敏感数据，必须先停止并请求人类确认；记录命令和结果。
- Output: 验证命令结果；如果失败，记录关键错误和判断。
- Verification: 自动运行的命令必须是 read-only 或低风险本地命令；高风险命令已在运行前等待人类确认；命令失败不能当作成功。
- AI Prompt:

```text
请检查 README、STATE、AGENTS 中记录的验证命令，并运行最小必要验证。

要求：
- Level 1 可能没有 AGENTS；不要假设文件一定存在。
- 只自动运行 read-only 或低风险本地验证命令。
- 如果记录的命令可能部署、migrate 数据、写文件、安装依赖、联系外部服务、改变认证或权限，或触碰生产 / 敏感数据，请停止并请求人类确认；确认前不要运行该命令。
- 如果存在多个命令，优先选择最小、read-only 或低风险的本地验证命令。
- 至少实际运行一个可用的验证命令；如果唯一可用命令需要人类确认，请说明风险并等待确认。
- 报告命令、结果和关键错误。
- 如果没有记录验证命令，请明确说明“未找到验证命令”。
- 如果命令失败，请明确说明失败，不要声称成功。
- 不要编造不存在的脚本、测试或工具链。

请输出：
- 检查过的文件
- 实际运行的命令
- 命令结果
- 失败时的关键错误和判断
```

### B6. 人类 review 并更新 STATE

- Input: 补齐后的 workspace 文件；验证命令结果。
- Action: 人类 review 目标、边界、禁止操作、完成标准和验证结果；确认后更新 `STATE.md` 的当前状态、下一步和阻塞问题。
- Output: 经确认的 workspace 文件；可交接的 `STATE.md`。
- Verification: `STATE.md` 能让下一个 agent 接手；Level 1 的禁止操作和完成标准可以记录在 `README.md` 或 `STATE.md`；Level 2+ 应在 `AGENTS.md` 中包含禁止操作和完成标准。
- AI Prompt:

```text
请基于 README、SPEC、STATE、AGENTS 和验证结果，列出需要人类确认的关键字段。

要求：
- 重点检查目标、边界、禁止操作、完成标准和验证结果。
- Level 1 可能没有 SPEC 或 AGENTS；不要假设文件一定存在。
- Level 1 的禁止操作和完成标准可以记录在 README 或 STATE；如果 AGENTS 不存在，请检查 README/STATE 是否已覆盖这些内容。
- Level 2+ 应在 AGENTS 中包含禁止操作和完成标准；如果 AGENTS 不存在或缺失这些内容，请标记为需要补齐。
- 如果 AGENTS 存在，确认其中包含禁止操作和完成标准。
- 确认前不要修改文件。
- 不要编造团队规则、验收标准或验证结论。

请输出：
- 需要人类确认的字段
- 每个字段的当前内容或缺失状态
- 建议确认问题
- 明确说明“等待人类确认后再更新 STATE.md”
```

### B7. 让 agent 做低风险接手测试

- Input: 经确认的 workspace 文件。
- Action: agent 阅读文件，复述项目状态，提出低风险交接任务；执行前等待确认；如果执行，先报告结果再做任何文档更新。
- Output: agent 对项目的理解摘要；低风险任务建议或执行结果。
- Verification: agent 能说出项目级别、当前状态、下一步和验证方法；不会再次询问基础上下文。
- AI Prompt:

```text
请阅读已创建或更新的 workspace 文件，并复述你对项目的理解。

要求：
- Level 1 可能没有 SPEC 或 AGENTS；不要假设文件一定存在。
- 说明项目级别、当前状态、下一步、阻塞问题和验证方法。
- 提出一个 read-only 或低风险交接任务。
- 不要执行高风险操作。
- 不要在确认前编辑文件。
- 如果人类确认执行低风险任务，请先执行并报告结果，再等待进一步确认后才更新任何文档。
- 不要再次询问已经写在 workspace 文件中的基础上下文。

请输出：
- 项目理解摘要
- 项目级别
- 当前状态
- 下一步
- 阻塞问题
- 验证方法
- 低风险交接任务建议
- 明确说明“等待人类确认后再执行”
```

## 关键步骤的 Claude Code / Codex CLI 提示

这些提示适合在关键节点单独复制给工具使用。默认先读文件、先报告计划、先等待确认；不要让工具假设 Level 1 一定有 `SPEC.md` 或 `AGENTS.md`。

### Repo 扫描

Claude Code prompt:

```text
请先阅读当前 repo 的 README、脚本、测试、构建配置和文档。
不要修改文件。
不要编造不存在的命令、目录或项目结构。
请输出项目现状、可用验证命令和缺失的 AI workflow infra。
如果某类信息不存在或无法确认，请明确写“缺失”或“需要确认”。
```

Codex CLI prompt:

```text
请在当前 repo 内检查 README.md、STATE.md、可选的 SPEC.md / AGENTS.md，以及可用脚本。
判断当前项目级别，列出缺失 infra，并给出最小补齐方案。
Level 1 不要假设一定存在 SPEC.md 或 AGENTS.md。
不要修改文件。
不要编造不存在的命令；如果找不到验证命令，请明确说明。
```

### 模板生成

Claude Code prompt:

```text
请基于 zh/templates/project/ 下的模板，为当前项目生成该级别必需文件的初稿。
Level 1 通常只需要 README.md、STATE.md 和验证命令记录；Level 2+ 才需要 SPEC.md 和 AGENTS.md。
请先列出将创建或更新的文件、证据来源、假设和需要人类确认的问题。
修改任何已有文件前，请等待人类确认。
不要编造不存在的命令、测试、部署流程或团队规则。
```

Codex CLI prompt:

```text
请在当前 repo 中读取已有文件和脚本，
基于 zh/templates/project/ 下的模板补齐最小 workspace 文件。
只使用 repo 中真实存在的信息。
Level 1 不要无条件创建 SPEC.md 或 AGENTS.md；Level 2+ 才补齐这些文件。
修改任何已有文件前，请先说明计划、涉及文件和证据来源，并等待确认。
不确定的内容请写“需要确认”。
```

### 最终 smoke test

Claude Code prompt:

```text
请阅读 README.md、STATE.md，以及如果存在的 SPEC.md、AGENTS.md。
复述项目级别、当前状态、下一步、阻塞和验证方式。
然后提出一个 read-only 或低风险接手任务。
不要修改文件，先等待确认。
```

Codex CLI prompt:

```text
请基于当前 workspace 文件复述项目状态，并提出一个可以安全执行的小任务。
Level 1 可能只有 README.md 和 STATE.md；不要假设 SPEC.md 或 AGENTS.md 一定存在。
提出的交接任务必须是 read-only 或低风险操作。
不要修改文件；等待确认前不要执行任务。
请明确说明“等待确认后再执行”。
不要执行高风险操作。
```

## 最终检查清单 / Final Checklist

- [ ] 项目级别已判断
- [ ] `README.md` 已创建或更新
- [ ] `STATE.md` 已创建或更新
- [ ] Level 2+ 项目有 `SPEC.md`
- [ ] Level 2+ 项目有 `AGENTS.md`
- [ ] Level 3/4 项目覆盖对应 guide 中的最低协作、风险和交接 infra
- [ ] 验证命令真实可运行，或已明确记录找不到真实验证命令
- [ ] `STATE.md` 写清下一步和阻塞
- [ ] Level 1 的禁止操作和完成标准已写入 `README.md` 或 `STATE.md`
- [ ] Level 2+ 的 `AGENTS.md` 写清禁止操作和完成标准
- [ ] 人类 review 过 AI 生成的命令和规则
- [ ] agent 能复述项目状态并接手一个 read-only 或低风险小任务

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [项目工作台模板](../templates/project/README.md)
