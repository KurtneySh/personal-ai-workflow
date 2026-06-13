# Project Workspace Templates

这些模板用于给项目搭建最小可用的 AI-readable workspace。

它们不是强制标准，而是一组示例：帮助你判断哪些信息必须写进文件，哪些内容可以随着项目复杂度增加再补。

## 用途 / Purpose

Project workspace templates 解决三个问题：

- 让人和 agent 快速理解项目是什么。
- 让 agent 知道如何运行、修改和验证项目。
- 让项目状态、目标、规则和上下文沉淀在文件系统里，而不是散落在聊天记录中。

## 适用方式 / How to Use

1. 先阅读 `zh/guides/project-levels.md` 判断项目级别。
2. 再阅读 `zh/guides/infra-by-level.md` 确认当前项目最小需要哪些文件。
3. 从本目录复制对应模板到你的项目根目录。
4. 让 AI 阅读当前仓库后生成初稿，并要求它标出假设。
5. 人类 review 关键字段，尤其是验证命令、禁止操作和人类审批点。

## Level 1-4 推荐表

| 模板 | Level 1 | Level 2 | Level 3 | Level 4 |
| --- | --- | --- | --- | --- |
| `README.template.md` | 必需 | 必需 | 必需 | 必需 |
| `STATE.template.md` | 必需 | 必需 | 必需 | 必需 |
| `SPEC.template.md` | 可选 | 必需 | 必需 | 必需，需写清高风险约束 |
| `AGENTS.template.md` | 可选 | 必需 | 必需，需写清协作规则 | 必需，需写清权限边界和审批点 |

## 模板选择建议

- Level 1 项目先写 `README.md` 和 `STATE.md`；`SPEC.md` 和 `AGENTS.md` 保持可选，除非项目会长期维护或需要 agent 反复参与。
- Level 2 项目建议一次性补齐 `README.md`、`SPEC.md`、`STATE.md` 和 `AGENTS.md`。
- Level 3 项目的 `AGENTS.md` 必须说明 review、ownership、handoff 和协作边界。
- Level 4 项目的 `SPEC.md` 和 `AGENTS.md` 必须说明风险、权限、人类审批点和回滚要求。

## 让 AI 生成初稿

可以使用这段提示：

```text
请阅读当前仓库，判断这个项目大致属于 Level 1-4 的哪一级。
基于 zh/templates/project/ 下的模板，为当前项目生成该级别必需模板的初稿。
Level 1 通常先生成 README.md 和 STATE.md；只有在项目需要长期维护或 agent 反复参与时，才生成 SPEC.md 和 AGENTS.md。
请标出你做出的假设。
不要编造不存在的命令、测试、部署流程或团队规则。
如果缺少信息，请用“需要确认”标出，而不是自行补全。
```

## 常见错误 / Common Mistakes

- 把模板当成强制规范，而不是根据项目级别裁剪。
- 让 AI 编造验证命令，而不是先检查项目实际脚本。
- `STATE.md` 创建后长期不更新。
- `AGENTS.md` 写成通用 prompt，没有项目里的禁止操作和完成标准。
- Level 4 项目没有写人类审批点、权限边界和回滚要求。
