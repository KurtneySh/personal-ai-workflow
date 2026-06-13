# AI 开发工作流手册

这个仓库用于沉淀 AI 开发工作流的知识库和使用说明书。

它不把 AI 当成聊天框，而是把项目建设成 AI 可读、可执行、可验证、可复用的工作台。

## 3 分钟使用路径

1. 阅读 [项目级别判断](zh/guides/project-levels.md)，判断当前项目属于 Level 1-4 的哪一类。
2. 阅读 [按级别推荐 Infra](zh/guides/infra-by-level.md)，选择最小可用的 AI 工作流基础设施。
3. 阅读 [Infra Checklist](zh/guides/infra-checklists.md)，按能力补充上下文、规则、验证、协作、风险和 loop 约束。
4. 遇到具体任务时，查 `zh/playbooks/` 和 `zh/cookbook/`。
5. 遇到重复流程时，查 `zh/skill-workflow/` 判断是否值得沉淀为 skill。
6. 想让 agent 自动循环时，先查 `zh/loops/` 判断是否适合 loop 化。

## 内容地图

- `zh/guides/`：项目级别、infra 推荐和 checklist。
- `zh/playbooks/`：从目标到完成的端到端任务流程。新项目启动见 [新项目启动 Playbook](zh/playbooks/start-a-new-project.md)。
- `zh/cookbook/`：按场景使用 Claude Code 和 Codex CLI。
- `zh/loops/`：loop 是否该做、怎么设计、怎么停止。loop 判断与设计见 [Loops](zh/loops/README.md)。
- `zh/skill-workflow/`：什么时候把流程沉淀成 skill。skill 判断、项目级 skill 和系统级 skill 候选见 [Skill Workflow](zh/skill-workflow/README.md)。
- `zh/cases/`：实战案例，记录项目背景、infra 选择、agent 行动和人的判断。
- `zh/templates/`：纯文档模板示例。项目工作台模板见 [项目工作台模板](zh/templates/project/README.md)。
- `zh/reference/`：概念、哲学、术语和附录材料。

## 当前状态

- 中文内容是 v1 的 source of truth。
- `README.md` 是面向未来公开分享的英文默认入口。
- 英文目录先保持同构占位，等中文内容稳定后再翻译。
- 模板只是示例，不是强制标准。
