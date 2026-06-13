# Repo Orientation

这个场景用于让 Claude Code 或 Codex CLI 在修改文件前读懂一个已有仓库。

目标不是让 agent 立即开始工作，而是先形成一份可验证的 repo orientation：项目做什么、当前处于什么状态、有哪些关键文件、可以运行哪些验证命令、哪些风险或未知信息需要人类确认。

## 适用场景 / When to Use

适合：

- 第一次进入一个陌生 repo。
- 隔了一段时间后重新接手项目。
- 给一个新的 agent 建立上下文。
- 在 task handoff 前先让 agent 做项目理解。

## 不适用场景 / When Not to Use

不适合：

- 你已经明确知道要修改什么文件，并且上下文足够。
- 你希望 agent 直接执行一个修改任务。
- 你准备设计自动 loop。
- 项目可能涉及 Level 4 风险，但还没有人类确认风险边界。

这个场景不给 agent 修改文件的权限。

## 输入材料 / Inputs

尽量提供：

- 项目目标或一句话背景。
- 当前想了解的问题。
- 已知项目级别；不确定时要求 agent 判断并说明依据。
- 需要重点阅读的文件或目录。
- 禁止操作，例如不要修改文件、不要安装依赖、不要运行部署命令。

## Claude Code Prompt

```text
请先只做 repo orientation，不要修改文件。

请阅读当前 repo 中存在的 README、STATE、SPEC、AGENTS、package/config 文件、脚本和主要目录结构。

要求：
- 只基于真实存在的文件和命令。
- 判断项目级别 Level 1-4；如果证据不足，请说明“不确定”和需要人类确认的问题。
- 总结项目目标、目录结构、当前状态、关键约定、可用验证命令、风险点和未知信息。
- 如果发现 README、STATE、SPEC、AGENTS 之间有矛盾，请列出来。
- 不要编造不存在的命令、测试、部署流程、团队规则或风险假设。
- 不要修改文件，不要安装依赖，不要运行部署、migrate、权限、认证、支付、生产或敏感数据相关命令。

请输出：
- 项目一句话总结
- 项目级别判断和依据
- 关键文件和目录
- 当前状态
- 可用验证命令以及证据来源
- 风险和未知信息
- 建议下一步
```

## Codex CLI Prompt

```text
请在当前 repo 内做一次只读 orientation。

请读取 README.md、STATE.md，以及如果存在的 SPEC.md、AGENTS.md、package/config 文件、脚本和主要目录结构。

要求：
- 只使用 repo 中真实存在的信息。
- 不要修改文件。
- 不要编造命令、测试、部署流程、团队规则或风险假设。
- 如果找不到验证命令，请明确说明。
- 如果不确定项目级别，请说明需要人类确认哪些信息。
- 不要运行部署、migrate、权限、认证、支付、生产或敏感数据相关命令。

请输出：
- repo 概览
- 项目级别判断
- 关键文件和目录
- 当前状态
- 真实可用的验证命令
- 文档矛盾或缺口
- 风险和需要人类确认的问题
- 建议下一步
```

## 期望输出 / Expected Output

一份 orientation 应该包含：

- 项目一句话总结。
- 项目级别判断和依据。
- 关键文件、目录和脚本。
- 当前状态和明显缺口。
- 真实存在的验证命令。
- 风险、未知信息和需要人类确认的问题。
- 下一步建议。

## 验证 / Verification

检查 agent 输出：

- 是否引用了真实文件。
- 推荐的命令是否真实存在。
- 找不到的信息是否明确标为未知。
- 是否没有把猜测写成事实。
- 是否没有修改文件。

可以运行：

```bash
git status --short
```

Expected: 如果只是 orientation，应无文件改动。

## 停止与升级 / Stop And Escalate

遇到以下情况，停止并交给人类判断：

- 项目级别可能是 Level 4。
- 涉及生产、权限、认证、支付、敏感数据、安全或合规。
- 文档之间互相矛盾。
- 找不到真实验证命令。
- agent 想要安装依赖、修改文件、部署、migrate、访问外部服务或改变权限。
- agent 无法说明信息来源。

## Skill Capture Signals

如果多次出现以下情况，可以考虑沉淀为项目级 skill：

- 每次进入 repo 都要重复同样的 orientation 步骤。
- agent 总是漏读同一批关键文件。
- orientation 输出需要固定格式才能被后续任务复用。
- 项目有稳定的验证命令、风险边界和状态记录规则。

如果这种 orientation 模式跨多个项目都有效，可以记录为系统级 skill 候选，但不能自动升级。

## 相关文档 / Related Docs

- [项目级别判断](../guides/project-levels.md)
- [按项目级别推荐 Infra](../guides/infra-by-level.md)
- [Infra Checklist](../guides/infra-checklists.md)
- [新项目启动 Playbook](../playbooks/start-a-new-project.md)
- [Loops](../loops/README.md)
- [Skill Workflow](../skill-workflow/README.md)
