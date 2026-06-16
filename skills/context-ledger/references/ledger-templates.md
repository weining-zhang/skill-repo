# Context Ledger 模板

默认目录：

```text
docs/context-ledger/
  problem.md
  decision-log.md
  solution-draft.md
  checkpoint.md
```

如果项目已有文档约定，优先融入现有目录。模板中的 `YYYY-MM-DD` 使用当前日期。

## problem.md

````md
# Problem Map

Status: Draft
Last updated: YYYY-MM-DD

## 背景

- 当前系统 / 场景：
- 当前痛点：
- 为什么现在要解决：

## 目标

### 必须达成

- 

### 最好达成

- 

### 不做范围

- 

## 约束

- 技术约束：
- 业务约束：
- 时间约束：
- 风险约束：

## 主链路

```text
环节 A -> 环节 B -> 环节 C -> 环节 D
```

## 子问题地图

| 环节 | 子问题 | 状态 | 需要取证 | 备注 |
| ---- | ------ | ---- | -------- | ---- |
|      |        | Open |          |      |

## 已确认事实

- [代码事实] 
- [运行事实] 
- [用户确认] 

## 当前假设

- [讨论假设] 

## 未确认问题

- [ ] 

## 矛盾 / 冲突

- 
````

## decision-log.md

````md
# Decision Log

Status: Active
Last updated: YYYY-MM-DD

## 状态说明

- Proposed：已提出，尚未取舍
- Tentative：暂定采用，依赖未确认前提
- Accepted：已接受，除非前提变化否则按此推进
- Superseded：已被后续决策替代
- Rejected：已拒绝

## D001：决策标题

- Status: Tentative
- Date: YYYY-MM-DD
- Owner / Decider: 待确认

### 背景

- 

### 选项

- A：
- B：
- C：

### 结论

暂定 / 接受 / 拒绝：

### 原因

- 

### 影响

- 

### 重新评估条件

- 

### 关联

- Problem：
- Solution：
- Evidence：
````

## solution-draft.md

````md
# Solution Draft

Status: Draft / Partial / May Change
Last updated: YYYY-MM-DD

## 当前方案摘要

- 

## 适用前提

- [讨论假设] 
- [待确认] 

## 总体链路

```text
输入 / 触发 -> 环节 1 -> 环节 2 -> 环节 3 -> 输出 / 状态
```

## 模块 / 环节一

### 当前结论

- 

### 依赖事实

- [代码事实] 
- [用户确认] 

### 待确认

- [ ] 

### 风险

- 

### 替代方案

- 

## 已废弃方案

| 方案 | 废弃原因 | 替代决策 |
| ---- | -------- | -------- |
|      |          |          |

## 下一步

- 
````

## checkpoint.md

````md
# Checkpoints

## YYYY-MM-DD：Checkpoint 标题

### 已接受结论

- 

### 暂定结论

- 

### 已废弃方案

- 

### 新出现的问题

- [ ] 

### 当前矛盾 / 需人工确认

- 

### 本轮更新

- problem.md：
- decision-log.md：
- solution-draft.md：

### 下一步建议

- 
````

## 轻量同步格式

当用户明确要求“先讨论，不改文件”时，使用：

```md
本轮建议同步到账本：

- problem.md：
  - 
- decision-log.md：
  - 
- solution-draft.md：
  - 
- checkpoint.md：
  - 
```
