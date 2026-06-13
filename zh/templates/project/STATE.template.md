# STATE Template

## 用途 / Purpose

记录项目短期状态、下一步、阻塞和交接上下文。这个文件应该频繁更新，优先保证接力清晰。

## 推荐级别 / Recommended Levels

- Level 1-4：必需。

## 必填部分 / Required Sections

- 当前阶段
- 当前目标
- 下一步
- 阻塞
- 最近完成
- 给下一个 agent 的上下文

## 可选部分 / Optional Sections

- 最近决策
- 已知问题
- 当前分支 / PR
- 最近验证结果

## AI 生成提示 / AI Generation Prompt

```text
请阅读当前仓库、README.md、SPEC.md 和最近的 git diff，为项目生成 STATE.md 初稿。
请只记录当前状态、下一步、阻塞和交接上下文。
请标出你做出的假设。
不要把长期产品愿景塞进 STATE.md；长期目标应该放在 SPEC.md。
```

## 常见错误 / Common Mistakes

- 创建后不更新。
- 把 STATE 写成第二份 README。
- 没有记录阻塞和下一步。
- 给下一个 agent 的上下文太含糊。

## 可复制模板 / Copyable Template

```markdown
# STATE

## 当前阶段

- （填写内容）

## 当前目标

- （填写内容）

## 下一步

1. （填写内容）
2. （填写内容）
3. （填写内容）

## 阻塞

- 无

## 最近完成

- （填写内容）

## 最近决策

| 日期 | 决策 | 原因 |
| --- | --- | --- |
|  |  |  |

## 已知问题

- （填写内容）

## 给下一个 agent 的上下文

- 当前应该优先处理：
- 不要重复做：
- 需要先确认：
- 最近验证结果：
```
