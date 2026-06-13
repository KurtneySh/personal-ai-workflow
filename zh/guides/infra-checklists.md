# Infra Checklist

这份 checklist 按能力组织，而不是按文件名组织。

同一个能力可以通过不同文件实现。关键是让 agent 能稳定理解、执行、验证和停止。

## Checklist Item Schema

每个 infra 项都应回答：

- Item
- Purpose
- Recommended levels
- Required when
- Example file
- How AI uses it
- Common mistakes

## 1. Context / 上下文

目的：让 agent 知道项目是什么、当前状态是什么、边界在哪里。

基础项：

- `README.md`
- `STATE.md`

增强项：

- `SPEC.md`
- `docs/decisions/`
- architecture notes
- glossary

常见错误：

- README 只有安装命令，没有项目目标。
- STATE 长期不更新。
- 决策只留在聊天记录里。

## 2. Instructions / 行为规则

目的：让 agent 知道在这个项目里应该怎么工作。

基础项：

- Level 1 可写在 README 内。
- Level 2+ 推荐 `AGENTS.md`。

增强项：

- coding conventions
- dependency policy
- branch / commit policy
- forbidden actions

常见错误：

- 写成通用 prompt，而不是项目规则。
- 没有验证命令。
- 没有说明哪些操作必须请求人类确认。

## 3. Feedback / 验证闭环

目的：让 agent 能判断修改是否正确。

基础项：

- 至少一个明确验证命令。

增强项：

- test
- lint
- typecheck
- build
- browser verification
- CI
- smoke test checklist

常见错误：

- 只要求 agent “看起来没问题”。
- 没有记录验证命令的预期结果。
- 本地验证和 CI 验证不一致。

## 4. Task Flow / 任务流

目的：让人和 agent 对齐目标、验收标准和停止条件。

基础项：

- task brief
- acceptance criteria

增强项：

- issue template
- PR checklist
- review checklist
- handoff notes
- changelog

常见错误：

- 任务只写“优化一下”。
- 没有完成标准。
- 没有停止条件。

## 5. Reuse / 复利沉淀

目的：把重复成功流程变成可复用资产。

增强项：

- skill workflow notes
- skill index
- reusable prompts
- playbook links
- case notes

常见错误：

- 每次重新 prompt。
- 只保存结果，不保存流程和判断。
- 没有记录失败模式。

## 6. Automation / Loop

目的：判断是否值得自动循环，以及如何安全停止。

增强项：

- loop readiness checklist
- iteration budget
- verification command
- stop conditions
- escalation rules
- human approval gates

常见错误：

- 没有验证命令就 loop。
- 没有最大迭代次数。
- 让 agent 在高风险操作上自动继续。

## 7. Collaboration / 协作

目的：降低多人或多 agent 协作中的职责不清和合并风险。

基础项：

- ownership
- PR checklist
- handoff notes

增强项：

- delegation contract
- worktree policy
- reducer role
- conflict resolution notes

常见错误：

- 多个 agent 修改同一组文件但没有 ownership。
- 没有 reducer 负责最终合并。
- review 只看总结，不看关键 diff。

## 8. Risk / 风险

目的：控制权限、数据、部署和不可逆操作。

基础项：

- risk register
- rollback plan
- security / privacy checklist
- human approval gates

增强项：

- threat model
- incident notes
- audit trail

常见错误：

- 让 agent 同时接触私有数据、不可信输入和对外通信。
- 没有回滚计划。
- 没有明确哪些操作必须人类审批。
