# 典型开发流程

```
需求 → 头脑风暴 → 写计划 → 实现 → 验证 → 审查 → 完成
```

## 流程

| 阶段 | 技能 | 核心规则 |
|------|------|----------|
| 1. 头脑风暴 | `brainstorming` | 设计未批准前禁止写代码 |
| 2. 写计划 | `writing-plans` | 精确到文件路径、完整代码、无占位符 |
| 3. 实现 | `subagent-driven-development`（推荐）或 `executing-plans` | 一个任务 = 一个子代理 + 两级审查 |
| ↳ 并行任务 | `dispatching-parallel-agents` | 2+ 独立问题域时并行派发 |
| ↳ TDD | `test-driven-development` | 无失败测试 → 无生产代码（RED → GREEN → REFACTOR） |
| ↳ 调试 | `systematic-debugging` | 找到根因之前禁止修复；3 次修复失败 → 质疑架构 |
| 4. 验证 | `verification-before-completion` | 无验证证据，不声称完成 |
| 5. 审查 | `requesting-code-review` + `receiving-code-review` | 任务完成后审查；反馈需验证而非盲从 |
| 6. 完成 | `finishing-a-development-branch` | 验证测试 → 选择合并/PR/保留/丢弃 |

## 技能自动触发

SessionStart hook 注入 `using-superpowers` 后，模型自动判断并调用技能：1% 可能适用就调用，流程类技能优先。

> 详见[详细版流程](development-workflow-detailed.md)。
