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
