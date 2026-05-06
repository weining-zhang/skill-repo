# Change Report 模板

## 目录

- [Change Report](#change-report)
- [Commit Record](#commit-record)
- [更新记录](#更新记录)

## Change Report

```md
# Change Report: <变更名称>

## 状态

- 状态：Draft / Ready for Review / Updated After Review
- 生成时间：
- 作者 / 维护者：

## 范围与版本

- Base Commit：
- Head Commit：
- Change Range：`<base>..<head>`
- 分支 / PR：

## 关联上下文

- 关联 REQ：
- 关联 PLAN：
- 关联 API Contract：
- 未确认上下文：

## 改动摘要

- 

## 实际改动 CHG

| ID | 模块 / 文件 | 改动 | 证据 | 关联 REQ / PLAN |
| -- | ----------- | ---- | ---- | --------------- |
| CHG-001 | | | | |

## 涉及文件 / 模块

| 路径 | 类型 | 说明 |
| ---- | ---- | ---- |

## 与 Plan 的偏差

| 偏差 | 原 Plan | 实际改动 | 原因 | 是否需要负责人确认 |
| ---- | ------- | -------- | ---- | ------------------ |

## 新增风险或边界影响

| 风险 | 来源 CHG | 影响 | 建议 Verification 覆盖 |
| ---- | -------- | ---- | --------------------- |

## Verification 关注点

- 

## 未确认事项

| 事项 | 不确定原因 | 需要确认的信息 |
| ---- | ---------- | -------------- |

## 备注

- 本报告只记录实际改动事实，不代表验证通过。
- Verification 结论应由 Verification Report 承载。
```

## Commit Record

Commit Record 是可选增强，只在大 PR、多人并行、审计级追踪、AI 高频提交或需要 commit 级绑定时使用。

```md
# Commit Record: <commit-sha>

## Commit 信息

- Commit：
- 作者：
- 时间：
- Message：

## 关联范围

- 所属 Change Report：
- 关联 REQ：
- 关联 PLAN：
- 关联 TEST：

## 改动内容

| 文件 / 模块 | 改动 | 证据 |
| ----------- | ---- | ---- |

## 风险与后续验证

- 新增风险：
- 需要 Verification 覆盖：

## 备注

- Commit Record 不替代 Change Report。
- Change Report 应聚合 Commit Record。
```

## 更新记录

更新已有 Change Report 时，可追加：

```md
## 更新记录

| 时间 | Head Commit | Change Range | 更新原因 | 说明 |
| ---- | ----------- | ------------ | -------- | ---- |
```
