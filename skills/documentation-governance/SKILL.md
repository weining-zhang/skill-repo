---
name: documentation-governance
description: 用于评估项目文档体系是否完整、结构是否合理、事实源是否清晰、阶段是否匹配，以及是否足以支撑 AI Coding、人工协作、review、验证和交付决策。适用于文档治理、文档目录盘点、Pre-Code/Post-Code 就绪度、文档架构、事实源冲突、追踪链断裂、Change Report、Verification Report、Known Issues 治理和交付就绪度检查。不用于文案润色、内容级文档 review、代码 review，或判断技术方案是否优雅 / 正确。
---

# 文档治理 / Documentation Governance

## 目标

作为“文档治理分析器”工作。只分析文档体系，不评审文档文案、代码正确性或技术方案优劣。

判断当前项目或模块的文档体系是否：

- 覆盖当前阶段必需的事实
- 采用合适的 Light Mode 或 Standard Mode
- 每类事实有清晰且唯一的事实源
- 派生文档没有重新定义事实
- 能支撑 AI Coding、人工协作、review、验证和交付决策
- 能避免多文档演进造成事实漂移、冲突或无限 review

## 资源读取

默认只使用本文件做快速诊断。

当需要精确检查必须文档、事实源映射、可选文档、断链、Verification 有效性或交付阻塞规则时，读取 [governance-rules.md](references/governance-rules.md)。

只有当用户明确要求“完整治理报告”“完整报告”“表格化报告”或“目标目录结构”时，读取 [full-report-template.md](references/full-report-template.md)。

## 职责边界

需要做：

- 识别当前项目模式：Light Mode / Standard Mode
- 识别当前阶段：Pre-Code / Post-Code / Mixed
- 检查必须文档和必须事实区块是否存在
- 检查目录结构和 `index.md` 职责是否清晰
- 检查每类事实是否只有一个事实源
- 检查派生文档是否定义了新事实
- 检查关键追踪链路是否断裂
- 检查 Verification 和 Known Issues 是否足以支撑交付判断
- 给出结构性治理建议

不需要做：

- 不评审代码正确性
- 不判断技术方案是否优雅或最优
- 不重写文档，不做文案润色
- 不生成完整业务文档
- 不把任务变成内容级文档 review
- 不输出“完善文档”“补充细节”等泛泛建议
- 不把 Light Mode 强行升级为 Standard Mode
- 不把可选文档说成必须文档

如果发现内容质量问题，只能在它影响结构、事实源、追踪链、验证或交付决策时，以治理问题形式指出。

## 核心模型

### 项目模式

Light Mode 适用于单人、小项目、短生命周期、低复杂度或小范围变更。有效结构可以是：

```text
docs/
  spec.md
  plan.md
  change.md
  verification.md
```

Standard Mode 适用于多人协作、长期维护、PR 流程、跨角色协作或复杂模块。有效结构可以是：

```text
docs/
  spec/
    index.md
    requirements.md
    acceptance.md
  api/
    index.md
    *.md
  plan/
    index.md
    design.md
    risk.md
    test-plan.md
  changes/
    index.md
    pr-*.md
  verification/
    index.md
    reports/
      v*.md
```

Light Mode 足够时，不要求升级为 Standard Mode。

### 当前阶段

Pre-Code：文档用于生码前锁定方向，尚无实际改动记录或验证记录。

Post-Code：代码已经实现，文档需要证明改了什么、如何验证、是否可交付。

Mixed：生码前和生码后文档同时存在，或实现已经开始但生码前事实仍不完整。

### 单一事实源

每类事实只能有一个主事实源：

| 事实类型 | 主事实源 |
| -------- | -------- |
| REQ | Spec |
| API | API Contract |
| PLAN | Implementation Plan |
| RISK / EDGE | Implementation Plan |
| TEST | Implementation Plan |
| CHG | Change Report |
| VERIFY | Verification Report |
| KI | Verification Report |

`index.md`、summary、release note、review record 都是派生文档。它们可以聚合和引用事实，但不得定义新的 REQ、API、PLAN、RISK、TEST、CHG、VERIFY 或 KI。

### 目录容器

把目录视为文档容器，把具体文件视为文档节点。

目录可以聚合、导航、展示当前状态、指向当前版本。事实必须落在具体文件或明确区块中，不接受“整个目录”作为模糊事实源。

Standard Mode 下，每个文档目录都建议有 `index.md`。`index.md` 只能做导航、聚合和当前状态说明。

## 必须锚点

Pre-Code 必须有：

- Spec，作为唯一需求事实源
- API Contract，当涉及接口、SDK、服务调用、请求 / 响应结构或错误码变化时
- Implementation Plan
- Plan 中独立的 Risk & Edge Cases 区块
- Plan 中独立的 Test Plan 区块

Post-Code 必须有：

- Change Report，包含 Base Commit、Head Commit、Change Range、关联 REQ、关联 PLAN、CHG、涉及文件 / 模块、与 Plan 的偏差和新增风险
- Verification Report，包含 Base Commit、Head Commit、Change Range、CHG 覆盖、TEST 覆盖、自动化 / 手动验证结果、失败或跳过项原因、Known Issues 和交付结论
- Verification Report 内部的 Known Issues 区块

Known Issues 必须包含 ID、等级、来源、描述、状态、Owner、处理计划或接受理由。未接受的 P0 / P1 阻塞交付。P2 需要修复、接受、进入 backlog 或由负责人明确取舍。P3 默认不阻塞交付。

## 治理判断点与人工决策

先基于文档和目录证据做判断；只有证据不足、判断会改变必须项，或需要负责人取舍时，才放入“需人工决策”。不得把所有不确定性都上交给人工。

必须识别这些判断点：

| 判断点 | 可直接判断的证据 | 需要人工决策的情况 |
| ------ | ---------------- | ------------------ |
| Light Mode / Standard Mode | 目录结构、PR / 子任务文档、多人协作痕迹、长期维护痕迹 | 结构与规模证据冲突；是否多人协作 / 长期维护无法从材料判断；建议升级 Standard Mode 会显著增加维护成本 |
| Pre-Code / Post-Code / Mixed | 是否存在 Spec / Plan / Change Report / Verification Report / 代码改动记录 / 验证结果 | 文档阶段互相矛盾；无法确认是否已经实现；阶段判断会决定是否要求 Change / Verification |
| 是否涉及 API Contract | 文档或变更明确出现接口、SDK、服务调用、请求 / 响应、错误码变化 | 材料无法确认是否涉及接口；要求 API Contract 会改变交付门槛 |
| 是否启用 Review Record | 存在多模型 review、争议建议、拒绝项或停止 review 需求 | 只是可能存在审查争议，但材料没有证据；是否保留审查决策记录取决于团队流程 |
| 是否启用 Commit Record | 大 PR、多人并行、审计要求、AI 高频自动提交、需要 commit 级追踪 | 是否需要审计级追踪属于治理成本取舍 |
| 是否从 Light Mode 升级 Standard Mode | 文档已自然拆分为多目录、多角色、多 PR / 子任务并行 | 当前 Light Mode 可工作，但未来是否长期维护或多人协作需要负责人判断 |
| Verification 是否过期 | Head Commit、Change Range、CHG 或 TEST 明确变化且报告未更新 | 无法获知当前代码 Head；只能提示“可能过期”，由人工确认当前被验证版本 |
| P2 / P3 如何处理 | Known Issues 已有状态、Owner、接受理由或 backlog | P2 是否本次修复、接受或延期，需要负责人取舍 |
| 输出快速诊断还是完整报告 | 用户明确要求完整报告时输出完整报告，否则快速诊断 | 不需要额外人工决策，按用户请求执行 |

如果存在需要人工决策的判断点，在默认输出中增加“需人工决策”小节，列出：

- 决策点：
- 不确定原因：
- 需要负责人确认的信息：
- 对交付判断的影响：

## 检查流程

1. 盘点相关文档文件和目录。
2. 判断项目模式，并说明证据。
3. 判断当前阶段，并说明证据。
4. 检查阶段错位，例如有 Change 但没有 Verification，或有 Verification 但没有 Change。
5. 按当前模式检查目录结构。
6. 按当前阶段检查必须文档和必须区块。
7. 检查事实源冲突，以及派生文档是否定义新事实。
8. 检查断链：REQ 到 PLAN、REQ 到 TEST、API 到 PLAN、RISK 到 TEST、PLAN 到 CHG、CHG 到 Verification、TEST 到结果、失败 / 跳过 TEST 到 Known Issues 或原因、review issue 到决策、Known Issue 到 Owner / 状态。
9. 当存在 Post-Code 产物时，检查 Verification 有效性。
10. 识别是否有需要人工决策的治理判断点。
11. 从固定状态列表中选择一个交付状态。

## Verification 有效性

Post-Code 阶段必须采用 commit 绑定验证：

- Verification 必须绑定 Base Commit 和 Head Commit。
- Verification 必须记录 Change Range，通常是 `Base..Head`。
- Verification 必须覆盖所有 CHG。
- Verification 必须覆盖关键 TEST。
- 失败或跳过的测试必须有原因或关联 Known Issues。
- Head Commit 变化时，Verification 可能过期。
- 新增 CHG 未被覆盖时，Verification 不完整。
- 合并前不要求 merged commit；合并后可以补充 merged commit。

## 默认输出

默认使用快速诊断模式。严格输出以下章节：

### 一、模式与阶段

- 模式：Light Mode / Standard Mode
- 阶段：Pre-Code / Post-Code / Mixed
- 判断依据：

### 二、必须文档检查

- 缺失：
- 已满足：

如果缺少任何必须文档或必须区块，输出：

```text
当前文档体系不足以支撑交付。
```

### 三、关键结构问题

最多输出 3 条。每条包含：

- 问题：
- 影响：
- 建议：

优先级顺序：

1. 缺少必须文档或必须区块
2. 事实源冲突
3. 阶段错位
4. 断链
5. 派生文档定义新事实
6. 目录结构不清晰

### 四、需人工决策

仅当存在需要负责人确认的治理判断点时输出。没有时写“无”。

每条包含：

- 决策点：
- 不确定原因：
- 需要确认的信息：
- 对交付判断的影响：

### 五、核心治理建议

只给一条结构性建议，例如：

- 补齐缺失事实源
- 将某类事实收敛到指定文件
- 将 `index.md` 改为聚合入口
- 为 Verification 增加 Known Issues 区块
- 为 Change Report 补充 Base / Head / Change Range
- 评估是否将 Light Mode 升级为 Standard Mode

禁止输出泛泛建议。

### 六、交付状态判断

只能选择一个：

- 文档体系足以支撑 Pre-Code 生码
- 文档体系不足以支撑 Pre-Code 生码
- 文档体系足以支撑 Post-Code review / 验证
- 文档体系不足以支撑 Post-Code review / 验证
- 文档体系足以进入人工最终决策
- 文档体系不可交付

并给出一句原因。

## 固定判断措辞

适用时使用这些措辞：

```text
当前文档体系不足以支撑生码前决策。
当前文档体系不足以支撑代码生成。
当前文档体系不足以支撑生码后 review / 验证 / 交付。
当前文档体系不足以证明实现正确性。
当前文档体系不足以支撑交付。
文档体系可支撑当前阶段，但建议按治理建议收敛结构。
文档体系足以支撑当前阶段。
```

## 禁止行为

禁止：

- 做代码 review
- 做内容级文档 review
- 判断实现质量
- 输出主观风格建议
- 为了完整性强行找问题
- 把可选文档说成必须文档
- 把 Light Mode 强行升级为 Standard Mode
- 在不知道是否涉及接口时强行要求 `api/`
- 要求未合并前必须知道 merged commit
- 要求每个 commit 都必须有 Commit Record
- 允许 `index.md` 定义新事实
- 允许派生文档成为事实源
