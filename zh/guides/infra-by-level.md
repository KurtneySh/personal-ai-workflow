# 按项目级别推荐 Infra

AI 工作流 infra 的目标不是文档越多越好，而是让 agent 拥有稳定上下文、可执行反馈和明确边界。

## Level 1：个人小项目 / 实验项目

最小 infra：

- `README.md`：项目是什么、如何运行。
- `STATE.md`：当前状态、下一步、已知问题。
- 至少一个验证命令：`test`、`run`、`lint` 或等价命令。

可选：

- task brief
- 简短 prompt / command log

## Level 2：个人长期维护项目

最小 infra：

- Level 1 全部
- `SPEC.md`：目标、范围、非目标、关键决策。
- `AGENTS.md`：agent 工作规则。
- `docs/decisions/`：重要决策记录。
- tests 或明确验证脚本。

可选：

- skill index
- reusable task briefs
- release notes

## Level 3：多人协作项目

最小 infra：

- Level 2 全部
- `CONTRIBUTING.md`
- `CODEOWNERS` 或 ownership 文档
- PR checklist
- CI checks
- issue / task template
- handoff notes
- multi-agent delegation contract

可选：

- team skill registry
- architecture decision records
- review playbooks

## Level 4：生产 / 高风险项目

最小 infra：

- Level 3 全部
- risk register
- security / privacy checklist
- rollback plan
- human approval gates
- deployment checklist
- incident / audit notes
- loop stop conditions
- permission boundaries for agents

可选：

- threat model
- staging verification matrix
- change freeze rules
- postmortem template

## 使用方式

先选择项目级别，再用 `infra-checklists.md` 按能力补充。

不要为了满足某个级别而机械创建所有文件。真正需要的是上下文、规则、验证、协作和风险控制能力。
