# skill-repo

个人 Codex Skill 包集合。

## Skills

### finite-review

用于有限轮次代码 / 文档审查，重点识别高置信度、可验证、会影响交付质量的问题，并判断是否可以进入人工最终决策、合并或发布流程。

适用场景：

- PR / MR 代码审查
- AI 生成代码审查
- 产品规格、技术规格、设计文档审查
- Bug fix 方案复审
- 上线前变更审查

位置：

```text
skills/finite-review/SKILL.md
```

### documentation-governance

用于评估项目文档体系是否完整、结构是否合理、事实源是否清晰、阶段是否匹配，以及是否足以支撑 AI Coding、人工协作、review、验证和交付决策。

适用场景：

- 文档治理和文档目录盘点
- Pre-Code / Post-Code 就绪度检查
- Light Mode / Standard Mode 结构判断
- 事实源冲突和断链检查
- Change Report、Verification Report、Known Issues 治理
- 交付就绪度判断

位置：

```text
skills/documentation-governance/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    ├── design-principles.md
    ├── full-report-template.md
    └── governance-rules.md
```

### change-report

用于在 Post-Code 阶段根据 git diff、commit range、PR 改动或已完成代码变更生成 / 更新 Change Report，并在必要时生成 Commit Record。

适用场景：

- 生成或更新 Change Report
- 记录 Base Commit、Head Commit、Change Range
- 汇总实际改动 CHG
- 映射关联 REQ / PLAN
- 标出与 Plan 的偏差、新增风险和 Verification 关注点
- 大 PR、多人并行或审计场景下生成 Commit Record

位置：

```text
skills/change-report/
├── SKILL.md
├── agents/
│   └── openai.yaml
└── references/
    └── templates.md
```

## 目录约定

每个 skill 至少包含一个 `SKILL.md`：

- `name` 和 `description` 用于触发 skill。
- 正文放核心流程和必须执行的规则。
- `references/` 用于按需加载详细规则、模板或参考材料。
- `agents/openai.yaml` 是可选 UI 元数据，用于展示名称、短描述和默认提示词；删除后不影响 skill 的核心可用性。
