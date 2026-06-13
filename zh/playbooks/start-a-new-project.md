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
