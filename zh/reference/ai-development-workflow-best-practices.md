---
title: AI 开发工作流最佳实践
created: 2026-06-12
updated: 2026-06-12
tags: [ai, workflow, agents, productivity]
---

# AI 开发工作流最佳实践

> 当前结论：不要把 AI 当聊天框，也不要一上来追求全自动。先把项目变成 AI 可读、可执行、可验证的环境，再把重复且可验证的部分沉淀成 Skill / Loop。

这份文档作为本仓库的工作哲学入口：它不追求记录所有资料来源，而是沉淀一套能指导实际开发的 AI 工作流原则。

## 1. 主入口：从聊天框迁移到本地 agentic 工具

开发主入口应从网页聊天框迁移到能读仓库、改文件、跑命令、看错误的本地 agentic 工具，例如 Claude Code、Codex CLI、Cursor。

聊天框的问题不是模型不够强，而是人需要在 AI 和执行环境之间搬运信息。更高效的方式是让 agent 直接进入工程环境，完成「读代码 -> 修改 -> 运行 -> 读错误 -> 再修改 -> 验证」的闭环。

落地原则：

- 默认在 repo 内启动 agent，而不是复制代码片段到聊天框。
- 让 agent 能访问代码、测试、日志、脚本、文档和错误输出。
- 人负责定义目标、约束、验收标准和关键判断，不充当复制粘贴报错的中间件。

## 2. 项目形态：为 AI 建一个上下文工作台

把项目资料、代码、设计、日志、任务、规则放进一个 AI 可读取、可 diff、可版本化的工作区。

推荐目录：

```text
project/
├── AGENTS.md           # agent 行为契约、项目规则
├── SPEC.md             # 高层目标、边界、产品判断
├── STATE.md            # 当前状态、下一步、阻塞、教训
├── skills/             # 可复用工作方法
├── docs/               # 设计、调研、决策记录
├── src/
├── tests/
├── scripts/            # agent 可调用的确定性工具
└── logs/               # 可被 agent 读取的反馈材料
```

核心不是目录形式本身，而是让上下文稳定地留在文件系统里。文件比聊天记忆更可控，也更适合审查、复用和版本管理。

## 3. 先建反馈闭环，再谈自动化

自动化之前，先让 agent 能验证自己的输出。测试、lint、构建、日志、trace、CLI、浏览器调试，比复杂 prompt 更基础。

落地原则：

- 每个项目准备一键验证命令：`test`、`lint`、`typecheck`、`build`。
- 任务描述必须包含 success criteria。
- 能做成 CLI 的能力优先做成 CLI；GUI 只作为人类界面，不作为 agent 唯一入口。
- 让 agent 在任务结束前主动运行验证命令，并报告结果。

## 4. Skill 是真正的复利资产

重复任务不要每次重新 prompt。把成功标准、真实踩过的坑、工具调用方式沉淀成 Skill。

一个好的 Skill 至少包含：

- 适用场景：什么时候应该使用它。
- 成功标准：什么结果算完成。
- 工作步骤：推荐的执行顺序。
- 常见坑：来自真实任务的失败模式。
- 可调用工具：命令、脚本、检查项、参考文件。
- 停止条件：什么时候应该请求人类判断。

落地原则：

- 高价值任务完成后，追加总结：「把本次流程沉淀成 Skill，方便下次复用。」
- Skill 不写成冗长 SOP，而是写清边界、验收和关键工具。
- 维护 `skills/index.md`，让 agent 能按任务发现该用哪个 Skill。

## 5. Loop 不是默认选项

不要把所有事情都做成后台 loop。只有任务重复、可自动验证、预算允许、agent 有工具时，才值得 loop 化。

适合作为早期 loop 的任务：

- CI 失败分诊；
- 依赖升级 PR；
- lint-and-fix；
- flaky test 复现；
- 强测试覆盖下的 issue 到 PR。

不适合作为早期 loop 的任务：

- 架构重写；
- 认证 / 支付；
- 生产部署；
- 含糊的产品探索；
- 完成与否主要依赖主观判断的任务。

Loop 必须有硬停止条件，例如最大迭代次数、最大耗时、最大 token、连续无进展次数、必须人工确认的风险点。

## 6. 多 agent 要先设计边界

多 agent 的关键不是数量，而是上下文隔离、权限边界、ownership、merge / reduce。没有收口机制，多 agent 只是多份意见。

落地原则：

- 单 agent 能做，先不拆。
- 要拆时写 delegation contract：Role / Goal / Context / Allowed actions / Ownership / Forbidden / Output format / Stop condition。
- 并行写代码时使用 worktree 或明确文件 ownership。
- 必须有 reducer 负责最终合并、冲突判断和责任收口。

## 7. 人的角色上移

AI 开发工作流不是人退出，而是人从打字者变成系统设计者、审查者、方向控制者。

适合交给 agent 的事情：

- 小范围实现；
- 测试补充；
- 错误定位；
- 文档整理；
- 重复性的代码迁移；
- 有明确验收标准的工具化任务。

人必须保留判断权的事情：

- 架构方向；
- 数据模型；
- 权限和安全；
- 依赖引入；
- 产品体验；
- 大范围重构；
- 是否继续、回滚或中止。

## 8. 管理理解债和安全债

AI 越能干，越容易积累理解债、安全风险和「再来一个 prompt」的惯性。

落地原则：

- 读关键 diff，不只看 agent 总结。
- 大改动隔离分支或 worktree。
- 不盲装第三方 Skill、MCP、plugin，先审查或重写干净版本。
- 给 loop 设置硬停止。
- 避免让 agent 同时拥有私有数据、不可信输入环境、对外通信三者。

## 推荐 v1 工作流

1. 每个项目建立 AI-readable repo：`AGENTS.md`、`SPEC.md`、`STATE.md`、`docs/`、`skills/`、`tests/`、`scripts/`。
2. 主力工具使用本地 agent：Codex CLI、Claude Code、Cursor，而不是网页聊天框。
3. 每个任务先写成功标准：完成什么、如何验证、何时停止。
4. 标准化验证命令：一键 test / lint / typecheck / build。
5. 重复任务沉淀成 Skill：先手动跑顺，再写 Skill，再考虑 loop。
6. 只有通过四条件测试的任务才 loop 化：重复、可验证、有预算、有工具。
7. 多 agent 只用于可隔离任务：每个 subagent 有 delegation contract、ownership、stop condition。
8. 人保留最终判断权：架构、权限、依赖、产品方向、大范围重构不自动放权。
