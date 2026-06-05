# 典型开发流程（详细版）

从需求到交付的完整流程，由 superpowers 技能体系驱动。

## 流程总览

```
需求 → 头脑风暴 → 写计划 → 实现 → 验证 → 代码审查 → 完成
  │        │         │        │       │        │         │
  │   brainstorming  │  subagent-driven  │  requesting  finishing-a
  │                  │  executing-plans  │  -code-review  -development
  │             writing-plans       verification    receiving    -branch
  │                                 -before-        -code-review
  │            test-driven-development (贯穿全程)
  │            systematic-debugging (按需触发)
```

## 阶段一：头脑风暴（Brainstorming）

**触发技能：** `brainstorming`

在写任何代码之前，先探索需求、设计架构。

1. 探索项目上下文（文件、文档、最近提交）
2. 如有视觉需求，提供 visual companion
3. 逐个提问澄清需求（每次一个问题）
4. 提出 2-3 种方案及取舍，给出推荐
5. 逐节展示设计，每节获得用户确认
6. 将设计文档写入 `docs/superpowers/specs/YYYY-MM-DD-<topic>-design.md`
7. 设计自检（占位符、一致性、范围、歧义）
8. 用户审阅设计文档后批准

**硬性规则：** 设计未获批准前，禁止写任何代码。

---

## 阶段二：编写实现计划（Writing Plans）

**触发技能：** `writing-plans`

将设计文档转化为细粒度的实现计划。

- 每个步骤 2-5 分钟可完成
- 精确的文件路径
- 每步包含完整代码和验证命令
- 遵循 TDD（先写测试，再写实现）
- 无占位符（禁止 "TODO"、"implement later"）
- 计划存入 `docs/superpowers/plans/YYYY-MM-DD-<feature-name>.md`

---

## 阶段三：实现

### 3a. 子代理驱动开发（推荐）

**触发技能：** `subagent-driven-development`

每个任务使用独立子代理执行 + 两级审查：

```
每个任务:
  1. 派发实现子代理 → 实现 + 测试 + 提交 + 自审
  2. 派发 spec 审查子代理 → 确认代码符合设计
  3. 派发代码质量审查子代理 → 确认代码质量
  4. 通过后标记任务完成

所有任务完成后:
  → 最终代码审查 → finishing-a-development-branch
```

### 3b. 顺序执行计划

**触发技能：** `executing-plans`

在当前会话中按计划顺序执行所有任务，适合子代理不可用或任务紧密耦合的场景。

### 3c. 并行派发代理

**触发技能：** `dispatching-parallel-agents`

当有 2+ 个相互独立的问题域时，并行派发代理各自解决。

---

## 贯穿全程：测试驱动开发（TDD）

**触发技能：** `test-driven-development`

**铁律：** 没有失败的测试，就没有生产代码。

```
RED    → 写失败的测试
GREEN  → 写最小实现让测试通过
REFACTOR → 清理代码，保持测试绿色
```

- 新功能、Bug 修复、重构 — 全部遵循 TDD
- 如果先写了实现代码 → 删除，从头开始
- 测试必须亲眼看到它先失败再通过

---

## 按需触发：系统化调试（Systematic Debugging）

**触发技能：** `systematic-debugging`

**铁律：** 找到根因之前，禁止提出修复方案。

```
Phase 1: 根因调查 → 读错误、复现、检查变更、追踪数据流
Phase 2: 模式分析 → 找可工作的相似代码、对比差异
Phase 3: 假设验证 → 提出假设、最小化测试、一次一个变量
Phase 4: 实施修复 → 写失败测试 → TDD 修复 → 验证
```

如果 3 次修复都失败 → 质疑架构，不要继续尝试。

---

## 阶段四：完成前验证

**触发技能：** `verification-before-completion`

**铁律：** 无验证证据，不声称完成。

```
声称任何状态前:
  1. 确认：什么命令能证明这个声称？
  2. 运行：执行完整命令
  3. 阅读：完整输出、退出码、失败计数
  4. 验证：输出是否确实支持该声称？
  5. 然后：才能做出声称
```

禁止使用"should"、"probably"、"seems to"等模糊表述。

---

## 阶段五：代码审查

### 请求审查

**触发技能：** `requesting-code-review`

- 每个任务完成后请求审查（子代理驱动模式）
- 合并到主分支前请求审查
- Critical 问题立即修复，Important 问题通过前修复

### 接收审查反馈

**触发技能：** `receiving-code-review`

- 验证后再实施（不盲从）
- 禁止表演性认同（"Great point!"、"Absolutely right!"）
- 外部反馈 = 需要评估的建议，不是必须服从的命令
- 有疑问就推回，用技术论证

---

## 阶段六：完成开发分支

**触发技能：** `finishing-a-development-branch`

```
1. 验证测试全部通过
2. 检测工作区环境
3. 提供 4 个选项:
   ① 合并到主分支
   ② 推送并创建 PR
   ③ 保持分支原样
   ④ 丢弃工作
4. 执行选择
5. 清理工作区（仅选项 ① 和 ④）
```

---

## 技能调用规则

所有技能自动触发的前提是 `using-superpowers` 技能已加载（SessionStart hook 自动注入）。

模型根据以下规则自行判断是否调用技能：

1. **只要有 1% 的可能性某个技能适用，就必须调用它**
2. **流程类技能优先**（brainstorming、debugging），再调用执行类技能
3. **用户指令优先级最高**，可以覆盖技能规则
