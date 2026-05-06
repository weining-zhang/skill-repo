# 文档治理规则

## 目录

- [治理原则](#治理原则)
- [项目模式规则](#项目模式规则)
- [阶段规则](#阶段规则)
- [治理判断点与人工决策](#治理判断点与人工决策)
- [现有文档结构冲突处理](#现有文档结构冲突处理)
- [必须文档规则](#必须文档规则)
- [可选文档规则](#可选文档规则)
- [事实源规则](#事实源规则)
- [断链检查](#断链检查)
- [Verification 规则](#verification-规则)
- [交付状态规则](#交付状态规则)

## 治理原则

只分析文档治理，不分析文案是否优美、代码是否正确、技术方案是否优雅。

每类事实只能有一个主事实源。允许派生文档、summary、release note 和 index 聚合事实，但不允许它们定义新事实。

把目录视为文档容器，把文件视为文档节点。目录可以聚合、导航和展示状态。具体事实必须落在文件或明确区块中。

## 项目模式规则

### Light Mode

Light Mode 适用于单人、小项目、短生命周期、低风险或窄范围变更。不要求嵌套目录。

推荐结构：

```text
docs/
  spec.md
  plan.md
  change.md
  verification.md
```

Light Mode 允许文档合并：

- `spec.md`：REQ 事实源
- `plan.md`：PLAN、RISK / EDGE、TEST 事实源
- `change.md`：CHG 事实源，必须包含 Base Commit、Head Commit、Change Range
- `verification.md`：VERIFY 和 KI 事实源

即使文档合并，也必须有清晰事实区块。不允许同一事实在多个文件重复定义。

### Standard Mode

Standard Mode 适用于多人协作、PR 流程、长期维护、跨模块、跨角色或复杂项目。

推荐结构：

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

Standard Mode 预期：

- `spec/` 是 REQ 事实源目录。
- 涉及接口契约变化时，存在 `api/`。
- `plan/` 承载 design、risk、test-plan 事实。
- Post-Code 阶段存在 `changes/`。
- Post-Code 阶段存在 `verification/`。
- 每个文档目录都有 `index.md`。
- `changes/index.md` 只聚合 Change Report，不定义 CHG。
- `verification/index.md` 只指向当前有效报告，不定义 VERIFY 或 KI。

## 阶段规则

### Pre-Code

Pre-Code 目标是在实现前锁定方向。检查：

- 需求是否有事实源
- 涉及接口时是否有契约
- 实施路径是否清晰
- 风险和边界是否已识别
- 测试计划是否在代码生成前设计完成

### Post-Code

Post-Code 目标是证明实现正确并控制交付风险。检查：

- 实际改动是否有记录
- 改动是否能追踪到 commit range
- Verification 是否覆盖所有改动
- 测试结果是否可追踪
- Known Issues 是否分级且状态化
- 是否没有未接受的 P0 / P1

### Mixed

当计划文档和实现 / 验证文档同时存在，或实现已经开始但 Pre-Code 必须事实仍不完整时，判断为 Mixed。

阶段错位会影响生码、review、验证或交付决策时，应作为关键结构问题输出。

## 治理判断点与人工决策

不要把所有分支判断都抛给人工。优先从目录、文档标题、事实区块、commit 信息、验证记录和 Known Issues 状态中直接判断。

只有满足以下任一条件时，才进入“需人工决策”：

- 当前材料无法提供判断所需事实。
- 两种判断会触发不同的必须文档要求。
- 判断结果会显著改变维护成本，例如是否升级为 Standard Mode。
- 需要负责人接受风险、决定延期、决定是否启用额外追踪。

### 需要识别的判断点

| 判断点 | 默认处理 | 需要人工决策的触发条件 |
| ------ | -------- | ---------------------- |
| Light Mode / Standard Mode | 根据目录深度、PR / 子任务文档、协作痕迹、长期维护痕迹判断 | 结构与规模证据冲突；协作规模未知；升级 Standard Mode 会显著增加维护成本 |
| Pre-Code / Post-Code / Mixed | 根据 Spec、Plan、Change、Verification、代码改动记录和验证结果判断 | 无法确认是否已实现；阶段判断会决定是否要求 Change / Verification；文档阶段互相矛盾 |
| API Contract 是否必须 | 明确出现接口、SDK、服务调用、请求 / 响应、错误码变化时要求 | 材料无法确认是否涉及接口；要求 API Contract 会改变交付门槛 |
| Review Record 是否启用 | 有多模型 review、争议建议、拒绝项或停止 review 需求时建议 | 只是可能存在审查争议，但没有证据；是否记录取决于团队流程 |
| Commit Record 是否启用 | 大 PR、多人并行、审计要求、AI 高频自动提交时建议 | 是否需要审计级追踪属于治理成本取舍 |
| 是否升级 Standard Mode | 已自然出现多目录、多角色、多 PR / 子任务时建议 | Light Mode 当前可工作，但未来是否长期维护或多人协作未知 |
| Verification 是否过期 | Head Commit、Change Range、CHG 或 TEST 明确变化且报告未更新时判断 | 无法获知当前 Head；只能提示可能过期，需要确认被验证版本 |
| P2 / P3 如何处理 | 已有状态、Owner、接受理由或 backlog 时按记录判断 | P2 是否修复、接受或延期，需要负责人取舍 |
| 现有文档结构冲突如何处理 | 能明确定位冲突事实、来源文件和目标事实源时，给出搬迁计划 | 需要搬迁原文内容、重命名文件、调整目录或改变维护方式时，必须人工确认 |
| 输出快速诊断还是完整报告 | 默认快速诊断；用户明确要求时完整报告 | 不需要人工决策，按用户请求执行 |

### 输出规则

有人工决策点时，在默认输出中加入“需人工决策”小节。每条包含：

- 决策点
- 不确定原因
- 需要确认的信息
- 对交付判断的影响

如果不存在人工决策点，写“无”。

## 现有文档结构冲突处理

当现有文档结构与推荐治理结构冲突时，目标不是重写文档，而是用最小搬迁让事实源和追踪链重新清晰。

### 处理原则

- 优先兼容现有结构，不默认要求迁移目录或重命名文件。
- 只搬迁原文档内已有内容，不修改原文内容。
- 原文内容包括完整段落、标题下的完整区块、表格、列表、代码块或 frontmatter；不得摘句拼接。
- 不得改写、润色、总结、重述、合并解释或改变含义。
- 执行搬迁前必须先给计划，并等待人工明确确认。
- 未确认前，只输出诊断、目标结构建议和内容搬迁计划，不实际修改文档。

### 处理顺序

1. 盘点现有文档和目录。
2. 将现有文档映射到治理角色：Spec、API Contract、Plan、Change Report、Verification Report、Known Issues、Index、Derived、Unknown。
3. 标出冲突类型：事实源冲突、派生文档定义新事实、目录职责不清、阶段错位、断链。
4. 确定每类事实的目标主事实源。
5. 判断是否可以通过补充索引说明或调整引用解决；能不搬迁内容时，不建议搬迁。
6. 必须搬迁时，列出原文搬迁计划。
7. 将搬迁计划放入“需人工决策”，等待负责人确认。
8. 只有在用户明确确认后，才执行搬迁。

### 内容搬迁计划格式

每个搬迁项至少包含：

- 来源位置：文件路径、章节标题、表格或区块标识
- 目标位置：目标文件路径和章节
- 搬迁范围：要搬迁的完整原文区块
- 是否原文搬迁：必须为“是”
- 搬迁原因：要解决的治理问题
- 预期解决的冲突：例如 REQ 双事实源、`index.md` 定义新事实、TEST 缺少主事实源
- 搬迁后原位置处理：删除原区块、保留引用、或等待人工决定
- 需要人工确认的问题：例如是否接受目标事实源、是否创建目标文件、是否调整目录

### 允许与禁止

允许：

- 原样移动完整区块到目标事实源。
- 原样移动表格、列表或代码块。
- 将原文档中的事实区块移动到更合适的主事实源。
- 在人工确认后创建必要的目标容器文件或目录。

禁止：

- 在搬迁时改写原文。
- 将多个来源的句子拼接成新段落。
- 用总结替代原文。
- 未经确认直接修改文件。
- 为了贴合推荐结构而强制迁移可工作的现有结构。
- 把目录迁移、文件重命名或维护模式升级当作自动操作。

## 必须文档规则

### Pre-Code 必须项

#### Spec

必须有 Spec 作为唯一 REQ 事实源。

最低覆盖：

- 目标
- 范围与非范围
- 关键行为
- 输入与输出
- 验收标准
- 关键约束

缺失时输出：

```text
当前文档体系不足以支撑生码前决策。
```

#### API Contract

出现以下任一情况时，必须有 API Contract：

- 新增 endpoint 或接口
- 修改 endpoint 或接口
- 前后端协作
- 服务间调用
- SDK 或公共模块契约变化
- 请求、响应或错误码变化

可接受形式：

- Light Mode：`spec.md` 中独立 API 区块，或 `docs/api.md`
- Standard Mode：`docs/api/`

最低覆盖：

- endpoint 和 method
- request
- response
- error code
- 兼容性说明

缺失时输出：

```text
涉及接口变更但缺少 API Contract，接口事实源不完整。
```

如果无法确认是否涉及接口，不得直接要求 `api/`；应放入“需人工决策”。

#### Implementation Plan

必须有 Implementation Plan 作为 PLAN 事实源。

必须有独立区块：

- Design / PLAN
- Risk & Edge Cases / RISK & EDGE
- Test Plan / TEST

Risk 和 Test Plan 可以合并在 plan 文件中，但必须是明确区块，不能散落在自然语言描述中。

Plan 缺失时输出：

```text
当前文档体系不足以支撑代码生成。
```

Risk & Edge Cases 缺失时输出：

```text
缺少风险与边界事实源，无法评估实现风险。
```

Test Plan 缺失时输出：

```text
缺少测试设计，无法证明后续实现可验证。
```

### Post-Code 必须项

#### Change Report

必须有 Change Report 作为 CHG 事实源。

最低覆盖：

- Base Commit
- Head Commit
- Change Range
- 关联 REQ
- 关联 PLAN
- 实际改动 / CHG
- 涉及文件或模块
- 与 Plan 的偏差
- 是否引入新风险

Light Mode：`change.md`。

Standard Mode：`changes/pr-*.md` 或任务级 Change Report。`changes/index.md` 只聚合。

默认不要求 Commit Record。Change Report 至少必须能追踪到 commit range。

缺失时输出：

```text
当前文档体系不足以支撑生码后 review / 验证 / 交付。
```

#### Verification Report

必须有 Verification Report 作为 VERIFY 和 KI 事实源。

最低覆盖：

- Base Commit
- Head Commit
- Change Range
- 覆盖的 CHG
- 覆盖的 TEST
- 自动化测试结果
- 手动验证结果
- 失败或跳过测试及原因
- Known Issues
- 交付结论

必须采用 commit 绑定和 Change 覆盖：

- Verification 绑定 Base Commit 和 Head Commit。
- Verification 记录 Change Range。
- Verification 覆盖所有 CHG。
- Head Commit 变化时，Verification 可能过期。
- 存在新增 CHG 时，Verification 必须补充覆盖。

缺失时输出：

```text
当前文档体系不足以证明实现正确性。
```

#### Known Issues

Known Issues 合并进 Verification Report。

必须包含：

- ID
- 等级：P0 / P1 / P2 / P3
- 来源：RISK / TEST / REVIEW / CHANGE
- 描述
- 状态
- Owner
- 处理计划或接受理由

建议状态：

- To Fix
- Accepted
- Blocked
- Deferred
- Done

交付规则：

- 未接受的 P0 / P1 阻塞交付
- P2 需要修复、接受、进入 backlog 或负责人明确取舍
- P3 默认不阻塞交付

缺失时输出：

```text
缺少已知问题与风险接受记录，交付风险不可见。
```

## 可选文档规则

### Review Record

以下情况建议有 Review Record：

- 多模型 review
- 跨工具 review
- 有争议建议
- 有拒绝项
- 需要打断无限 review

Review Record 应记录问题来源、等级、接受 / 拒绝决策、拒绝原因、关闭状态和停止条件。

Review Record 不得重新定义 REQ、CHG、TEST 或 KI。若 review issue 成为 Known Issue，必须落到 Verification Report。

### Commit Record

默认不要求 Commit Record。

仅在以下情况建议启用：

- 大 PR
- 多人并行修改同一模块
- 审计级追踪
- AI 高频自动提交
- 需要 commit 与 REQ / TEST 精确绑定

若启用，Change Report 应聚合 Commit Record。

### Summary / Release Note

Summary 和 Release Note 只能作为派生文档，不得定义新事实。

## 事实源规则

期望映射：

| 事实类型 | 期望事实源 |
| -------- | ---------- |
| REQ | Spec |
| API | API Contract |
| PLAN | Implementation Plan |
| RISK / EDGE | Implementation Plan |
| TEST | Implementation Plan |
| CHG | Change Report |
| VERIFY | Verification Report |
| KI | Verification Report |

需要指出的冲突：

- Spec 和 README 同时定义需求。
- Plan 和 Change Report 同时定义实际改动。
- Verification 和 Change Report 同时定义 CHG。
- `index.md` 和子文档同时定义 REQ、CHG、TEST、VERIFY 或 KI。
- Summary 引入新需求。
- Release Note 记录了 Change Report 中不存在的改动。
- `changes/index.md` 定义新 CHG。
- `verification/index.md` 定义新测试结果。
- Plan 重新定义需求。
- Verification 重新定义需求或设计。
- Change Report 重新定义验收标准。
- Review Record 定义 Known Issues，但没有落到 Verification。

## 断链检查

检查这些链路：

- REQ -> PLAN
- REQ -> TEST
- API -> PLAN
- RISK / EDGE -> TEST
- PLAN -> CHG
- CHG -> Verification
- TEST -> Verification Result
- Failed / Skipped TEST -> Known Issues 或明确原因
- Review Issue -> Accepted / Rejected / Known Issue / Fixed
- Known Issue -> 状态 / Owner / 处理计划

需要指出：

- 需求没有实施方案
- 风险没有测试覆盖
- 改动没有验证
- 测试用例没有结果
- 失败测试没有原因
- Known Issue 没有状态或 Owner

## Verification 规则

检查 Verification Report 是否：

- 绑定 Base Commit
- 绑定 Head Commit
- 记录 Change Range
- 覆盖所有 CHG
- 覆盖关键 TEST
- 列出失败或跳过测试
- 将失败或跳过测试关联到 Known Issues 或原因
- 存在未接受 P0 / P1

过期判断：

- 如果 Head Commit 与被验证代码不同，Verification 可能过期。
- 如果新增 CHG 没有被覆盖，Verification 不完整。
- 如果 TEST 变化但 Verification 未更新，Verification 不完整。
- 如果 Base Commit 发生重大变化，提示可能需要重新验证。

未合并前不要求 merged commit。

合并前接受：

- Base Commit：当前分支基于的主线 commit
- Head Commit：当前分支或 PR 最新 commit
- Change Range：`Base..Head`

合并后可以补充：

- Merged Commit

## 交付状态规则

最终状态只能选择一个：

- 文档体系足以支撑 Pre-Code 生码
- 文档体系不足以支撑 Pre-Code 生码
- 文档体系足以支撑 Post-Code review / 验证
- 文档体系不足以支撑 Post-Code review / 验证
- 文档体系足以进入人工最终决策
- 文档体系不可交付

以下情况使用“文档体系不可交付”：

- 当前阶段必需文档缺失
- Post-Code 阶段缺少 Change Report
- Post-Code 阶段缺少 Verification Report
- Verification 未覆盖所有 CHG
- 存在未接受的 P0 / P1
- Known Issues 缺少状态
- 严重事实源冲突导致无法判断哪个文档为准

缺少必须项时，输出：

```text
当前文档体系不足以支撑交付。
```

结构完整但有少量非阻塞问题时，输出：

```text
文档体系可支撑当前阶段，但建议按治理建议收敛结构。
```

无关键缺口时，输出：

```text
文档体系足以支撑当前阶段。
```
