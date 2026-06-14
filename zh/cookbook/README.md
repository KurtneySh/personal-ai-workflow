# Cookbook

Cookbook 用于把 AI 开发工作流落到具体任务场景里。

它不是 Claude Code 或 Codex CLI 的命令大全，而是一组场景化做法：什么时候使用、给 agent 什么上下文、怎么写 prompt、怎么验证、什么时候停止，以及什么信号说明这个流程值得沉淀为 skill。

## 定位 / Positioning

- `zh/playbooks/`：端到端工作流，例如从空仓库启动项目。
- `zh/cookbook/`：具体任务场景，例如让 agent 读懂 repo 或接手一个小任务。
- `zh/loops/`：判断一个任务是否适合自动循环。
- `zh/skill-workflow/`：判断重复场景是否值得沉淀为 skill。

Cookbook 里的 prompt 不是魔法指令。它们仍然依赖真实项目上下文、明确边界和可验证结果。

## 当前场景 / Current Scenarios

- [Repo Orientation](repo-orientation.md)：让 agent 在修改文件前读懂一个已有仓库。
- [Task Handoff](task-handoff.md)：把一个边界清楚的任务交给 Claude Code 或 Codex CLI。
- [Review And Fix](review-and-fix.md)：处理具体 test、lint、review 或 CI 反馈。
- [Context Refresh](context-refresh.md)：在长会话、中断、agent 切换或上下文压缩之后恢复当前项目状态。
- [PR CI Workflow](pr-ci-workflow.md)：在 PR 准备、CI 读取、review 处理和合并前检查中组织 agent 协作。

## 后续场景 / Planned Scenarios

后续可以继续补充：

- multi-agent delegation。

这些场景先不创建文件，等真实使用后再补。

## 如何使用一个场景 / How To Use A Scenario

1. 判断项目级别。
2. 阅读当前项目状态。
3. 选择匹配的 cookbook 场景。
4. 填入输入、约束和禁止事项。
5. 运行对应 Claude Code 或 Codex CLI prompt。
6. 用真实信号验证结果。
7. 记录状态、停止或升级给人类，必要时考虑是否沉淀为 skill。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
