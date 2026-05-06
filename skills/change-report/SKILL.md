---
name: change-report
description: 用于在 Post-Code 阶段根据 git diff、commit range、PR 改动或已完成代码变更生成 / 更新 Change Report，并在必要时生成 Commit Record。适用于记录 Base Commit、Head Commit、Change Range、实际改动 CHG、关联 REQ / PLAN、涉及文件、与计划偏差、新增风险，以及为后续 review、Verification Report、Known Issues 和交付判断提供可追踪改动事实源。不用于代码 review、Verification、测试结论判断、执行 git commit / push，或替代文档治理。
---

# Change Report

## 目标

把已经发生的代码改动固化为可追踪的事实记录，作为 Post-Code 阶段的 CHG 事实源。

Change Report 需要回答：

- 基于哪个 Base Commit 开始改
- 当前 Head Commit 是什么
- Change Range 是什么
- 实际改了哪些文件、模块和行为
- 这些改动关联哪些 REQ / PLAN
- 与 Plan 是否有偏差
- 是否引入新风险或后续 Verification 关注点

## 资源读取

默认使用本文件即可完成普通 Change Report。

当用户要求输出完整模板、更新现有 Change Report、生成 Commit Record，或需要固定表格格式时，读取 [templates.md](references/templates.md)。

## 执行时机

Pre-Code：不生成 Change Report。此阶段应维护 Spec、API Contract、Implementation Plan、Risk & Edge Cases、Test Plan。

Coding 中：可以生成草稿，但必须标记为 Draft，不作为交付依据。

Post-Code 改动稳定后：生成或更新 Change Report。

进入 review / Verification 前：必须确认 Change Report 覆盖当前 `Base..Head`。

Verification 后：如果 Head Commit、CHG 或 TEST 发生变化，必须更新 Change Report，并提示 Verification 可能需要复核。

不要把本 skill 绑定到每一次 git commit。Commit Record 是可选增强，只在大 PR、多人并行、审计级追踪、AI 高频提交或需要 commit 级 REQ / TEST 绑定时启用。

## 职责边界

需要做：

- 读取 git 状态、commit range、diff、文件列表和必要上下文
- 生成或更新 Change Report
- 记录 Base Commit、Head Commit、Change Range
- 总结实际改动 CHG，但只基于 diff 和文件证据
- 映射关联 REQ / PLAN；无法确认时明确标记“未确认”
- 标出与 Plan 的偏差
- 标出新增风险或需要 Verification 覆盖的点
- 在需要时生成 Commit Record 草稿

不需要做：

- 不做代码 review
- 不判断实现是否正确
- 不判断测试是否通过，除非用户提供明确测试输出
- 不生成 Verification Report
- 不创建 Known Issues，最多提示“可能需要进入 Known Issues”
- 不执行 git commit、git push、git reset、git checkout 等版本控制操作，除非用户明确要求
- 不编造 REQ / PLAN / TEST 关联

## 输入优先级

优先使用用户明确提供的 Base Commit、Head Commit、Change Range、PR 号、需求文档、Plan 文档或目标输出路径。

如果用户没有提供：

- Base Commit：优先使用当前分支 merge-base 或用户指定主线；无法确认时标记“待确认”
- Head Commit：优先使用当前 `HEAD`
- Change Range：优先使用 `Base..Head`
- 输出路径：根据现有文档结构判断；无法确定时只输出报告内容，不直接写文件

不得假装知道缺失的业务背景。无法从材料确认的 REQ / PLAN / 风险，必须标记“未确认”。

## 推荐 git 取证

按需使用以下命令收集证据：

```bash
git status --short --branch
git branch --show-current
git merge-base HEAD origin/master
git rev-parse HEAD
git diff --stat <base>..<head>
git diff --name-status <base>..<head>
git log --oneline <base>..<head>
git diff <base>..<head> -- <path>
```

如果仓库主线不是 `origin/master`，不要强行使用；应根据当前分支追踪信息、用户说明或仓库约定判断。无法判断主线时，把 Base Commit 放入“需人工确认”。

## 输出位置

Light Mode：

- 默认写入或建议写入 `docs/change.md`

Standard Mode：

- 默认写入或建议写入 `docs/changes/pr-*.md`、`docs/changes/<task>.md` 或现有 changes 目录下的任务级 Change Report
- `docs/changes/index.md` 只能聚合，不应定义新的 CHG

如果现有文档结构不同，优先兼容现有结构。只有用户明确要求写文件，且目标路径清晰时，才修改文件；否则先输出 Change Report 草稿和建议路径。

## Change Report 必须包含

- 标题
- 状态：Draft / Ready for Review / Updated After Review
- Base Commit
- Head Commit
- Change Range
- 关联 REQ
- 关联 PLAN
- 改动摘要
- 实际改动 CHG
- 涉及文件 / 模块
- 与 Plan 的偏差
- 新增风险或边界影响
- Verification 关注点
- 未确认事项

## Commit Record 使用规则

默认不生成 Commit Record。

只有以下场景才建议生成：

- 单个 PR 很大
- 多人并行修改同一模块
- 需要审计级追踪
- AI 自动提交频繁
- 需要把每个 commit 与 REQ / TEST 精确绑定

Commit Record 是 Change Report 的细粒度补充，不替代 Change Report。Change Report 应聚合 Commit Record。

## 更新规则

更新现有 Change Report 时：

- 保留已有人工确认内容
- 只更新与当前 Change Range 相关的 CHG
- 如果 Head Commit 变化，更新 Head Commit 和 Change Range
- 如果新增 CHG，补充实际改动和 Verification 关注点
- 如果发现旧报告覆盖范围不再匹配，标记为“需复核”
- 不删除人工记录的风险接受、偏差说明或未确认事项，除非用户明确要求

## 默认输出

默认输出以下结构：

### 一、范围与版本

- Base Commit：
- Head Commit：
- Change Range：
- 状态：

### 二、关联上下文

- 关联 REQ：
- 关联 PLAN：
- 未确认上下文：

### 三、改动摘要

用 3-7 条说明实际改动，只基于 diff 和文件证据。

### 四、实际改动 CHG

| 模块 / 文件 | 改动 | 证据 | 关联 REQ / PLAN |
| ----------- | ---- | ---- | --------------- |

### 五、与 Plan 的偏差

没有证据时写“未发现可确认偏差”或“缺少 Plan，无法判断”。

### 六、新增风险与 Verification 关注点

只输出与改动直接相关的风险和需要验证的点。

### 七、未确认事项

列出无法从当前材料确认，但影响 Change Report 完整性的事项。

## 禁止行为

禁止：

- 编造需求、计划、测试或风险关联
- 把代码审查发现写成已确认 CHG，除非 diff 支持
- 把测试命令或测试结果写成已通过，除非用户提供输出或已实际运行
- 在没有 Base Commit 时假装 Change Range 完整
- 把 Commit Record 说成默认必需
- 把 Change Report 写成 Verification Report
- 未经用户明确要求执行 git commit、git push、git reset、git checkout
