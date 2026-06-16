# skill-repo

个人 Codex Skill 包集合，用来把反复出现的 AI 协作流程沉淀成可复用的工作协议。

## 怎么选

| 问题类型 | Skill | 解决什么问题 | 适用场景 | 入口 |
| -------- | ----- | ------------ | -------- | ---- |
| 长链路上下文 | `context-ledger` | 长链路讨论容易丢上下文、混淆事实 / 假设 / 决策 | 架构设计、复杂调试、重构 / 迁移方案、多轮讨论和 context 压缩 | `skills/context-ledger/` |
| 改动事实记录 | `change-report` | 已完成改动缺少可追踪事实记录 | 根据 git diff、commit range、PR 改动生成 / 更新 Change Report，记录 CHG、REQ / PLAN 关联、偏差和风险 | `skills/change-report/` |
| 有限轮次审查 | `finite-review` | AI review 容易发散、重复或纠缠低价值建议 | 代码审查、文档审查、规格复审、Bug fix 复审、上线前风险审查、多模型 review 归并 | `skills/finite-review/` |
| 文档体系治理 | `documentation-governance` | 文档体系是否完整、事实源是否清晰、是否支撑交付难以判断 | 文档目录盘点、Pre-Code / Post-Code 就绪度、事实源冲突、断链、Known Issues 和交付状态检查 | `skills/documentation-governance/` |

## 使用方式

这些 skill 不是一条固定流水线，设计初衷是各自独立解决一类协作问题。按当前问题单独选用即可，需要时再组合。

可选组合示例：

- 长链路方案讨论：只用 `context-ledger`。
- 已完成代码改动记录：只用 `change-report`。
- 单次代码 / 文档审查：只用 `finite-review`。
- 文档体系盘点：只用 `documentation-governance`。
- 大型改造交付：可以按需组合多个 skill，但不要默认套用完整链路。

## 目录约定

每个 skill 至少包含：

```text
skills/<skill-name>/
  SKILL.md
```

可选目录：

```text
skills/<skill-name>/
  agents/openai.yaml
  references/
  scripts/
  assets/
```

- `SKILL.md`：核心触发描述和执行规则。
- `references/`：按需读取的模板、规则或参考材料。
- `agents/openai.yaml`：Codex UI 元数据。
- `scripts/` / `assets/`：确定性脚本或可复用素材。
