# 文档治理 Skill 设计思想

## 目录

- [1. 项目或模块到底需要哪些必要文档](#1-项目或模块到底需要哪些必要文档)
- [2. 不同开发阶段需要的文档为什么不同](#2-不同开发阶段需要的文档为什么不同)
- [3. 哪些文档必须，哪些文档可选](#3-哪些文档必须哪些文档可选)
- [4. 多文档如何避免事实冲突](#4-多文档如何避免事实冲突)
- [5. 不同项目规模如何适配不同治理成本](#5-不同项目规模如何适配不同治理成本)
- [6. 文档是目录时如何定位事实源](#6-文档是目录时如何定位事实源)
- [7. 多人协作下 Change Report 如何落地](#7-多人协作下-change-report-如何落地)
- [8. Verification Report 如何判断是否仍然有效](#8-verification-report-如何判断是否仍然有效)
- [9. Known Issues 如何支撑交付决策](#9-known-issues-如何支撑交付决策)
- [10. 多文档之间如何形成交付闭环](#10-多文档之间如何形成交付闭环)
- [11. 如何避免 Governance Skill 变成另一个 Review Skill](#11-如何避免-governance-skill-变成另一个-review-skill)
- [12. 如何避免治理报告本身过重](#12-如何避免治理报告本身过重)
- [13. 如何处理与现有文档结构的冲突](#13-如何处理与现有文档结构的冲突)

## 1. 项目或模块到底需要哪些必要文档

AI Coding 容易出现两种极端：一种是文档不足，导致 AI 生码方向不清；另一种是文档过多，导致维护成本过高。

设计思想：以“是否支撑当前交付阶段”为标准，定义必要文档。

- Pre-Code：支撑生码前决策
- Post-Code：支撑生码后验证与交付

解决方式：

Pre-Code 必须关注：

- Spec
- API Contract，涉及接口时
- Implementation Plan
- Risk & Edge Cases
- Test Plan

Post-Code 必须关注：

- Change Report
- Verification Report
- Known Issues

## 2. 不同开发阶段需要的文档为什么不同

生码前和生码后的目标不同。如果混在一起检查，会出现阶段错位，例如还没写代码就要求 Change Report，或者代码已生成却没有 Verification。

设计思想：按交付阶段拆分治理目标。

解决方式：

Pre-Code 关注：

```text
方向是否正确，AI 是否能按正确目标生成代码。
```

Post-Code 关注：

```text
实现是否可验证，是否具备交付条件。
```

## 3. 哪些文档必须，哪些文档可选

如果所有文档都强制，会导致流程过重；如果都不强制，又无法保证交付质量。

设计思想：按“是否承载不可替代事实源”区分文档必要性。

解决方式：

- 必须文档：缺失后无法支撑当前阶段决策
- 条件必须文档：特定变更类型下必须存在
- 可选文档：增强协作、审计、发布表达，但不默认阻塞

示例：

- 必须：Spec、Plan、Change Report、Verification Report
- 条件必须：API Contract
- 可选：Review Record、Commit Record、Release Note

## 4. 多文档如何避免事实冲突

当需求、方案、改动、验证结果被多个文档重复描述时，后续更新很容易出现多个版本的“真相”。

设计思想：引入单一事实源原则。

解决方式：

每类事实只允许有一个主来源，其它文档只能引用，不能复制或重新定义。

事实映射：

- REQ -> Spec
- API -> API Contract
- PLAN -> Implementation Plan
- RISK / EDGE -> Plan 中的独立区块
- TEST -> Plan 中的 Test Plan 区块
- CHG -> Change Report
- VERIFY -> Verification Report
- KI -> Verification Report 中的 Known Issues 区块

## 5. 不同项目规模如何适配不同治理成本

单人小项目不适合重型目录化治理；多人中大型项目如果过于轻量，又容易协作混乱。

设计思想：定义 Light / Standard 两档模式。

解决方式：

Light Mode 适合单人、小项目，使用单文档闭环。

```text
docs/
  spec.md
  plan.md
  change.md
  verification.md
```

Standard Mode 适合多人、中大型项目，使用目录化治理。

```text
docs/
  spec/
  api/
  plan/
  changes/
  verification/
```

## 6. 文档是目录时如何定位事实源

真实项目中文档可能不是单个文件，而是一个目录加多个子文档。如果只按文件判断，会误判文档缺失或事实源混乱。

设计思想：目录是文档容器，文件或明确区块才是事实源。

解决方式：

- 目录负责聚合、导航、状态入口
- 文件负责定义具体事实
- `index.md` 只能做入口和聚合，不能定义新事实
- 事实源必须落在具体文件或明确区块中

## 7. 多人协作下 Change Report 如何落地

多人协作时，全局 Change Report 很难由一个人维护；但没有改动记录，后续 review 和验证会发散。

设计思想：Change Report 默认追踪到 PR / 子任务级，记录 commit range，而不是强制每个 commit 都写记录。

解决方式：

Change Report 必须包含：

- Base Commit
- Head Commit
- Change Range
- 关联 REQ / PLAN
- 实际改动 CHG

Commit Record 作为可选增强，仅在审计级追踪、AI 高频提交、大 PR 等场景启用。

## 8. Verification Report 如何判断是否仍然有效

验证报告不是永久有效的。代码、测试用例或改动范围变化后，原验证结论可能已经失效。

设计思想：Verification 需要绑定版本边界和覆盖范围。

解决方式：

Verification 必须绑定：

- Base Commit
- Head Commit
- Change Range
- Covered CHG
- Covered TEST

过期条件包括：

- Head Commit 变化
- 新增 CHG 未覆盖
- TEST 变更但 Verification 未更新
- Base Commit 发生重大变化

未合并前不要求知道最终主分支 commit，只绑定 `Base..Head`。

## 9. Known Issues 如何支撑交付决策

测试失败、风险接受、技术债如果散落在 review 记录或聊天上下文中，交付时无法判断是否还能发布。

设计思想：Known Issues 合并进 Verification Report，但必须独立区块、分级、状态化。

解决方式：

Known Issues 必须包含：

- 等级：P0 / P1 / P2 / P3
- 状态：To Fix / Accepted / Blocked / Deferred / Done
- Owner
- 来源：RISK / TEST / REVIEW / CHANGE
- 处理计划或接受理由

交付规则：

- 未接受的 P0 / P1：不可交付
- P2：需要修复、接受或记录取舍
- P3：不阻塞

## 10. 多文档之间如何形成交付闭环

即使文档都存在，也可能只是各写各的：需求没有方案，风险没有测试，改动没有验证，失败测试没有归因。

设计思想：通过断链检查验证文档体系是否闭环。

解决方式：

检查以下链路：

- REQ -> PLAN
- REQ -> TEST
- API -> PLAN
- RISK / EDGE -> TEST
- PLAN -> CHG
- CHG -> Verification
- TEST -> Verification Result
- Failed / Skipped TEST -> Known Issue / 原因
- Review Issue -> Accepted / Rejected / Fixed / Known Issue
- Known Issue -> Owner / Status / Plan

## 11. 如何避免 Governance Skill 变成另一个 Review Skill

如果不限制边界，Governance Skill 会开始评价方案优劣、润色文档、审查代码，和 Review Skill 重叠。

设计思想：明确 Governance Skill 只检查文档体系，不检查内容质量。

解决方式：

Governance Skill 只输出：

- 缺失文档
- 事实源冲突
- 阶段错位
- 断链
- 目录结构问题
- 交付状态判断

不输出：

- 代码问题
- 方案优劣
- 文案润色
- 泛泛优化建议

## 12. 如何避免治理报告本身过重

如果每次都输出完整治理报告，会增加新的文档负担。

设计思想：默认快速诊断，完整治理报告作为可选模式。

解决方式：

默认输出：

1. 模式与阶段
2. 必须文档检查
3. 关键结构问题，最多 3 条
4. 核心治理建议，只给 1 条
5. 交付状态判断

只有明确要求时，才输出完整治理报告。

## 13. 如何处理与现有文档结构的冲突

真实项目通常已经有自己的文档结构。治理规则如果直接要求重组目录、重命名文件或重写内容，容易制造额外维护成本，也可能破坏已有团队习惯。

设计思想：优先兼容现有结构，用最小搬迁解决事实源和追踪链问题。

解决方式：

- 先把现有文档映射到治理角色，而不是直接套推荐目录。
- 如果现有结构能支撑单一事实源和交付闭环，就保留现有结构。
- 如果同类事实分散在多个文档中，先确定主事实源。
- 解决冲突时，只搬迁原文档内已有内容，不修改原文内容。
- 搬迁必须以完整段落、表格、列表、代码块或明确区块为单位，不摘句拼接。
- 搬迁前必须给出计划，说明来源、目标、范围、原因和风险。
- 只有人工确认后，才执行实际搬迁。

这条原则让 Governance Skill 保持“治理建议者”的角色：先识别结构问题和搬迁方案，再由负责人决定是否执行。
