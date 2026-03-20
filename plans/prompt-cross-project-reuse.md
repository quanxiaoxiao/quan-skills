# Prompts 跨项目复用方案

> 目标：让 `quan-skills` 仓库中的 `prompts/` 能在别的项目中稳定复用，同时解决 prompt 内部关联 `skills/`、`rules/`、`checklists/`、`docs/` 路径只在本仓库内成立的问题。

---

## 一、结论

默认采用：

- **全局仓库级引用**

不默认采用：

- 把 prompts 复制到每个项目
- 继续依赖 `../skills/...` 这类相对路径

核心思路：

1. 把 `~/.codex/skills/quan-skills` 视为全局 prompt pack
2. 在别的项目中直接引用这里的 prompts
3. prompt 内部统一使用逻辑路径协议，而不是 markdown 相对路径
4. 目标项目只提供“如何解析这些逻辑路径”的约定，不复制内容

---

## 二、当前问题

当前仓库中的 prompts 存在两类引用方式：

### 1. 文本式引用

例如：

- `rules/scripts-global.rule.md`
- `checklists/scripts-global.checklist.md`
- `skills/ts-backend-standard/SKILL.md`

这类写法可读，但默认假设“读取者知道仓库根在哪里”。

### 2. 相对路径链接

例如：

- `../skills/doc-evidence-compare/SKILL.md`
- `../docs/skill-authoring.md`

这类写法只在当前仓库目录结构下稳定。一旦 prompt 被复制到别的项目，或由外部项目直接读取，路径语义就会漂移。

---

## 三、推荐目标结构

### 1. 引入逻辑路径协议

统一使用以下格式表示本仓库内的关联资源：

```text
quan-skills:prompts/lint-build-repair.md
quan-skills:skills/doc-evidence-compare/SKILL.md
quan-skills:rules/scripts-global.rule.md
quan-skills:checklists/scripts-global.checklist.md
quan-skills:docs/skill-authoring.md
```

约定：

- `quan-skills:` 是逻辑前缀，不是相对路径
- 它始终解析到全局仓库根目录
- 默认根目录为 `~/.codex/skills/quan-skills`

### 2. 定义全局根变量

统一约定：

```text
QUAN_SKILLS_HOME=~/.codex/skills/quan-skills
```

逻辑解析规则：

```text
quan-skills:<subpath>
=> $QUAN_SKILLS_HOME/<subpath>
```

示例：

```text
quan-skills:skills/doc-evidence-compare/SKILL.md
=> ~/.codex/skills/quan-skills/skills/doc-evidence-compare/SKILL.md
```

---

## 四、Prompts 写法规范

### 1. 头部统一格式

每个可跨项目复用的 prompt 统一使用以下头部字段：

```md
> Authoritative prompt: `quan-skills:prompts/<name>.md`
> Authoritative skill: `quan-skills:skills/<skill>/SKILL.md`
> Related rule: `quan-skills:rules/<rule>.rule.md`
> Related checklist: `quan-skills:checklists/<checklist>.checklist.md`
```

说明：

- `Authoritative prompt` 必填
- `Authoritative skill` 在 prompt 依附某个 skill 时必填
- `Related rule` / `Related checklist` 按需填写
- 一个 prompt 可关联多个治理制品，但必须明确主 skill

### 2. 不再把 markdown 相对链接作为主引用机制

允许：

- 保留少量辅助链接，便于在本仓库中直接点击阅读

不允许：

- 把 `../skills/...`、`../docs/...` 作为跨项目使用时的唯一定位方式

### 3. Prompt 内容中的“读取顺序”仍保留文本表达

例如：

- 先读目标项目的 `README.md`
- 再读目标项目的 `AGENTS.md`
- 再读 `quan-skills:skills/...`

也就是说：

- **项目上下文** 用目标项目本地路径表达
- **方法论上下文** 用 `quan-skills:` 逻辑路径表达

---

## 五、目标项目接入方案

### 方案 A：全局仓库级引用（默认）

适用场景：

- 多个项目共享同一套 prompts / skills
- 希望统一升级，不做多仓同步
- 使用者接受依赖本机全局 skill pack

目标项目需补一段接入约定，建议写入其 `AGENTS.md` 或等价文件：

```md
## Shared Prompt Pack

This repository uses the shared `quan-skills` pack installed at:

`~/.codex/skills/quan-skills`

When a prompt references `quan-skills:<subpath>`, resolve it as:

`~/.codex/skills/quan-skills/<subpath>`

When applying a shared prompt:

1. Read the target repository context first
2. Read the referenced prompt / skill / rule / checklist from `quan-skills`
3. Apply the prompt using this repository's local code and docs as runtime context
```

优点：

- 单一事实源
- 升级集中
- 不会在每个项目产生 prompt 副本

缺点：

- 依赖全局安装路径存在
- 需要使用者理解 `quan-skills:` 逻辑前缀

### 方案 B：导出到目标项目（备选）

适用场景：

- 某项目必须自包含
- CI 或离线环境不能依赖全局路径
- 需要冻结某一版 prompt 快照

做法：

1. 从本仓库导出指定 prompt
2. 同时导出其关联的 skill / rule / checklist 元信息
3. 在导出时把 `quan-skills:` 重写成 vendored 路径

建议 vendored 目录：

```text
docs/shared-prompts/quan-skills/
```

此方案只作为补充，不作为默认工作流。

---

## 六、实施步骤

### P0 — 建立协议

1. 在仓库内确认 `quan-skills:` 为唯一逻辑路径协议
2. 在相关文档中声明 `QUAN_SKILLS_HOME`
3. 明确“项目上下文路径”和“全局方法论路径”的边界

### P1 — 改造 prompts

1. 审计 `prompts/*.md`
2. 将所有主引用改为 `quan-skills:` 逻辑路径
3. 统一头部格式
4. 保留必要的文本化 Pre-Read 指引

### P2 — 补文档

1. 在根级文档中新增“跨项目使用 prompts”说明
2. 给出至少一个完整示例
3. 给出目标项目接入片段

### P3 — 可选导出机制

1. 仅在确有需求时增加导出脚本或导出流程
2. 导出产物只做快照，不反向维护

---

## 七、验收标准

完成后应满足：

- 任意一个 prompt 都能在外部项目中通过 `quan-skills:` 找到其关联资源
- prompt 不再依赖 `../skills/...` 作为唯一主路径
- 目标项目只需声明一次 `QUAN_SKILLS_HOME` 解析规则即可使用共享 prompts
- 使用共享 prompt 时，方法论从全局仓库读取，业务上下文从目标项目读取
- 若采用导出模式，导出后的路径也能独立解析

---

## 八、不采用的方案

### 1. 每个项目复制一份 prompts

不作为默认方案，原因：

- 内容会漂移
- 升级成本高
- 难以知道哪份是最新

### 2. 继续使用相对路径作为主机制

不作为默认方案，原因：

- 仓库外部使用时不稳定
- 一旦复制或移动文件，路径就失效

### 3. 完全去掉路径，只保留纯自然语言提示

不作为默认方案，原因：

- 失去可追踪性
- 关联 skill / rule / checklist 的能力会变弱

---

## 九、后续落地建议

后续可以基于本方案继续做两件事：

1. 在 `docs/` 增加一份面向使用者的接入说明
2. 在 `audits/` 增加一份 `prompt-cross-project-reuse-audit.md`，专门核对：
   - 是否仍有相对路径主引用
   - 是否所有 prompt 都声明了 authoritative source
   - 是否目标项目接入说明完整
