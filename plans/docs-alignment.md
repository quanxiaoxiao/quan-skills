# 文档对齐调整方案

> 目标：以 `skills/` 为中心，建立一套可执行、可审计的文档治理结构，对齐 `rules/`、`checklists/`、`prompts/`、根级路由与 skill 本体质量，消除缺失、重复、命名漂移和引用断链。

---

## 一、调整原则

### 1. 以 skill 为入口，而不是以目录补洞

- `skills/<name>/SKILL.md` 是任务触发入口
- `rules/` 承载持久约束
- `checklists/` 承载复核标准
- `prompts/` 承载可复用任务模板
- 根级 `SKILL.md` 和 `AGENTS.md` 负责路由与激活声明

### 2. 不要求每个 skill 强制拥有完整三件套

本仓库的 authoring 规范并未要求每个 skill 都必须同时拥有 `rule + checklist + prompt`。

因此本次调整先定义制品策略，再补文件，避免“为了凑矩阵而新增低价值文档”。

### 3. 先修治理断点，再补充低频能力

优先级应从高到低处理：

1. skill 本体健康度
2. 根级路由 / 激活声明 / 引用链
3. 高复用 skill 的缺失制品
4. 低频 skill 的轻量补充

---

## 二、制品策略

### A 类：必须有 `rule + checklist + prompt`

适用于同时满足以下条件的 skill：

- 有稳定、可复用的持久约束
- 有明确的验收标准
- 有反复出现的任务模板

当前归类：

| Skill | 说明 |
|---|---|
| `behavior-safe-code-repair` | 有稳定风险约束、明确验证口径、已有 prompt/checklist，只缺 rule |
| `hurl-testing` | 已完整 |
| `zx-script` | 已完整 |

### B 类：至少有 `checklist + prompt`，rule 视约束稳定性决定

适用于流程型或调查型 skill：

- 重点在“怎么查 / 怎么比对 / 怎么输出”
- 约束多体现在流程，不一定需要独立 rule

当前归类：

| Skill | 说明 |
|---|---|
| `doc-evidence-compare` | 优先补 checklist；是否拆出独立 rule 取决于后续复用频率 |
| `auto-proxy-detect` | 优先补 prompt + checklist；先不强制抽 rule |

### C 类：先修 skill 本体，再决定是否外提制品

适用于表达性或模板型 skill：

- 先保证 `SKILL.md` 自身符合 authoring 规范
- 再根据实际使用频率决定是否外提 checklist / prompt

当前归类：

| Skill | 说明 |
|---|---|
| `admin-crud-style` | 先修 frontmatter、标准章节、引用与可发现性，再补轻量 checklist / prompt |
| `doc-governance-init` | 自治型 skill，内部已有 templates / prompts / checklists，不外提 |
| `ts-backend-standard` | skill 本体已完整，主要补 task prompt |

---

## 三、当前问题矩阵

| 类别 | 问题 | 当前证据 | 影响 |
|---|---|---|---|
| 制品策略 | 覆盖矩阵默认每个 skill 都应有完整三件套，但动作表并未遵守同一标准 | 当前矩阵与后续动作存在偏差 | 会导致补齐标准不一致 |
| 根级路由 | 根级 `SKILL.md` 未注册 `auto-proxy-detect`、`admin-crud-style` | `AGENTS.md` 已列为 active skill，但根级路由缺失 | 入口层声明不一致 |
| skill 本体 | `admin-crud-style` 不符合 skill authoring 结构要求，frontmatter 也异常 | 缺标准章节、无 `agents/openai.yaml` | 该 skill 质量低于仓库基线 |
| 规则重复 | `functional-only-js-ts.rule.md` 与 `forbid-oop-patterns.rule.md` 高度重叠 | 主题重复 | 维护成本高，易漂移 |
| 命名漂移 | `no-oop-js-ts.checklist.md` 与对应 rule 命名不一致 | 名称不统一 | 检索和引用成本高 |
| 引用链 | 计划只覆盖 `Pre-Read Order` 和 prompt 文件头，未覆盖 `agents/openai.yaml`、workflow、全仓检索 | 审计范围偏窄 | 重命名后容易留残引用 |
| 验收缺失 | 当前计划没有统一的完成定义 | 无统一 DoD | 难以判断“对齐已完成” |

---

## 四、目标结构

### 1. skill 本体层

所有 active skill 至少满足：

- `SKILL.md` 有合法 YAML frontmatter
- 包含 `Use This Skill`
- 包含 `Pre-Read Order`
- 包含 `Workflow`
- 包含 `Verification`
- 如支持隐式发现，补 `agents/openai.yaml`

### 2. 外部治理层

按制品策略维护：

- `rules/` 只放持久约束
- `checklists/` 只放复核项
- `prompts/` 只放任务模板
- 根级 `SKILL.md` 只做共享默认值与 skill 路由
- `AGENTS.md` 只做激活声明与共享治理规则

### 3. 审计层

新增一个对齐审计文档放在：

- `audits/docs-alignment-structure-audit.md`

用于核对：

- 目标文件是否存在
- 废弃文件是否已移除
- skill 本体是否健康
- 根级路由是否一致
- 重命名后是否还有残留引用

---

## 五、覆盖矩阵（按目标状态）

| Skill | Rule | Checklist | Prompt | Skill 本体 | 目标状态 |
|---|---|---|---|---|---|
| `ts-backend-standard` | `backend-functional-architecture` ✓ | `backend-architecture` ✓ | 补 `ts-backend-audit.md` | 已健康 | 补 prompt |
| `behavior-safe-code-repair` | 补 `behavior-safe-code-repair.rule.md` | `lint-build-repair` ✓ | `lint-build-repair` ✓ | 已健康 | 补 rule |
| `hurl-testing` | `hurl-testing` ✓ | `hurl-quality` ✓ | 现有 3 个 ✓ | 已健康 | 保持 |
| `doc-evidence-compare` | 暂不强制 | 补 `doc-evidence-compare.checklist.md` | 现有 2 个 ✓ | 已健康 | 补 checklist |
| `zx-script` | `scripts-global` ✓ | `scripts-global` ✓ | `create-or-review-script` ✓ | 已健康 | 保持 |
| `doc-governance-init` | 内置治理 ✓ | 内置 ✓ | 内置 ✓ | 已健康 | 保持自治 |
| `auto-proxy-detect` | 暂不强制 | 补 `auto-proxy-detect.checklist.md` | 补 `auto-proxy-detect.md` | 已有 `agents/openai.yaml` | 补轻量制品 |
| `admin-crud-style` | 暂不强制 | 补 `admin-crud-style.checklist.md` | 补 `admin-crud-scaffold.md` | 先修复 | 先修 skill 再补轻量制品 |

---

## 六、调整动作

### P0 — 先修治理断点

| # | 动作 | 产出文件 | 说明 |
|---|---|---|---|
| 1 | 修复 `admin-crud-style` 的 `SKILL.md` 结构 | `skills/admin-crud-style/SKILL.md` | 修复 frontmatter，补标准章节，压实边界 |
| 2 | 为 `admin-crud-style` 补发现入口 | `skills/admin-crud-style/agents/openai.yaml` | 与其他 active skill 保持一致 |
| 3 | 补全根级 skill 路由 | `SKILL.md` | 注册 `auto-proxy-detect` 与 `admin-crud-style` |

### P1 — 补齐高价值缺失制品

| # | 动作 | 产出文件 | 说明 |
|---|---|---|---|
| 4 | 为 `ts-backend-standard` 补 task prompt | `prompts/ts-backend-audit.md` | 现有 skill 已稳定，补外部调用模板 |
| 5 | 为 `behavior-safe-code-repair` 提取持久约束 | `rules/behavior-safe-code-repair.rule.md` | 固化风险分级、最小 diff、禁用伪修复 |
| 6 | 为 `doc-evidence-compare` 补复核清单 | `checklists/doc-evidence-compare.checklist.md` | 固化证据矩阵和置信度复核口径 |

### P2 — 去重与命名对齐

| # | 动作 | 产出文件 | 说明 |
|---|---|---|---|
| 7 | 合并 OOP 禁止类规则 | `rules/functional-only-js-ts.rule.md` | 吸收 `forbid-oop-patterns` 独有内容后删除旧文件 |
| 8 | 统一 checklist 命名 | `checklists/functional-only-js-ts.checklist.md` | 由 `no-oop-js-ts` 重命名而来 |
| 9 | 修复所有受影响引用 | `AGENTS.md` / `SKILL.md` / `rules/` / `prompts/` / `skills/` | 防止重命名后断链 |

### P3 — 补低频但可复用的轻量制品

| # | 动作 | 产出文件 | 说明 |
|---|---|---|---|
| 10 | 为 `auto-proxy-detect` 提取 prompt | `prompts/auto-proxy-detect.md` | 复用诊断步骤 |
| 11 | 为 `auto-proxy-detect` 补 checklist | `checklists/auto-proxy-detect.checklist.md` | 固化 5 步诊断结果检查 |
| 12 | 为 `admin-crud-style` 提取 prompt | `prompts/admin-crud-scaffold.md` | 复用 CRUD 页面脚手架任务 |
| 13 | 为 `admin-crud-style` 补 checklist | `checklists/admin-crud-style.checklist.md` | 固化布局 / 交互 / 组件一致性检查 |

### P4 — 引用链与审计收口

| # | 动作 | 产出文件 | 说明 |
|---|---|---|---|
| 14 | 审计所有 active skill 的 `Pre-Read Order` | `skills/*/SKILL.md` | 确保指向正确的 rule / checklist / prompt |
| 15 | 审计所有 `agents/openai.yaml` | `skills/*/agents/openai.yaml` | 确保 display 与 default prompt 与 skill 状态一致 |
| 16 | 审计根级路由与 active list | `AGENTS.md` / `SKILL.md` | 防止“active 但不可路由” |
| 17 | 审计 prompts 文件头部 | `prompts/*.md` | 标明 authoritative source 与相关 rule/checklist |
| 18 | 做一次全仓残留检索 | 全仓 | 检查旧命名、旧路径、旧文件名是否仍被引用 |
| 19 | 输出结构核对文档 | `audits/docs-alignment-structure-audit.md` | 作为本轮调整的验收基线 |

---

## 七、不动的部分

- `doc-governance-init` 内置 `prompts/`、`checklists/`、`templates/`
  这是 skill 自身产品的一部分，保持自治，不外提到仓库根级。
- `node-*` 系列通用规则暂不强制一一配对 checklist
  这类规则更多通过 workflow 与 audit prompt 编排消费，当前无需为了对称性机械补齐。
- `skill-repo.rule.md` 与 `skill-quality.checklist.md` 保持现状
  它们属于元治理基线，本轮只消费，不重构。

---

## 八、完成定义

本轮“文档对齐完成”至少满足以下条件：

### 结构完成

- 所有 active skill 都存在可用的 `SKILL.md`
- `admin-crud-style` 满足 skill authoring 的最低结构要求
- 根级 `SKILL.md` 与 `AGENTS.md` 的 active skill 集合一致

### 制品完成

- P1 / P2 / P3 计划中的目标文件均已创建或更新
- 已废弃文件已删除或明确标记为保留
- 不再存在重复维护的 OOP 禁止类 rule

### 引用完成

- `Pre-Read Order`、`agents/openai.yaml`、prompts、根级路由均已更新
- 仓库内不存在旧命名的残留引用
- 重命名文件的所有入链都可追踪到新路径

### 审计完成

- `audits/docs-alignment-structure-audit.md` 已生成
- 审计文档中的存在性、边界、交叉引用检查项可逐条执行
- 至少完成一次基于该 audit 的人工核对

---

## 九、执行顺序

```text
P0 先修治理断点
→ P1 补高价值缺失制品
→ P2 去重与命名对齐
→ P3 补低频轻量制品
→ P4 引用链审计与最终收口
```

说明：

- `P0` 必须前置，否则后续补充建立在不稳定 skill 本体之上
- `P2` 放在 `P1` 之后，避免在补核心制品前先触发大面积重命名
- `P4` 是最终收口阶段，不应省略

---

## 十、产出清单

本轮调整应至少产出：

- 更新后的 `plans/docs-alignment.md`
- 一份结构核对文档：`audits/docs-alignment-structure-audit.md`
- 后续执行阶段产生的新增或重命名文件

该计划文档负责定义目标结构、动作顺序与验收标准；`audits/` 文档负责落地核对。
