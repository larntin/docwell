# 示例：特化到 `docs/` + `docs/enterprise/` + `docs/saas/` + `deploy/` 结构

> 这是把 [`../docwell.md`](../docwell.md) 适配到一个典型"双产品线（企业版 + SaaS）"项目的成品。直接替换项目根 `CLAUDE.md` / `AGENTS.md` 中的对应段落即可。

## 项目基础约定（最高优先级，写任何文档/脚本/SQL 前必读）

**权威位置总览**：本项目权威知识全部位于 `docs/` —— `docs/` 根目录为全局公用、`docs/enterprise/` 为企业版专属、`docs/saas/` 为 SaaS 专属；可执行交付物位于 `deploy/<version>/`。任何实现、排错、决策前，先在对应目录按编号检索。

### 文档（docs/）

- `docs/` 下以 `${num}_` 前缀编号管理：
  - `docs/00_*`、`docs/01_*` … 是**全局公用知识**（跨版本通用，SaaS/企业版都适用）。
  - `docs/enterprise/` 是**企业版专属**知识，内部同样按 `${num}_` 编号。
  - `docs/saas/` 是 **SaaS 专属**知识，内部同样按 `${num}_` 编号。
- **写前必查**：用 glob/grep 工具按文件名和标题搜现有编号与所属目录，不要逐个 `cat` 文件；命中 3 个以下候选才全文读。复用/扩展已有文件，不要新建重复主题。
- **唯一权威**：同一主题禁止在多处并存。发现重复时，合并到唯一权威文件，其它位置改为引用链接。
- **简明、链接优先**：每篇文档只做必要、简明、无歧义的内容。背景只写**读懂本文必须的部分**，其它通过链接指向对应权威文档。不复述 `CLAUDE.md` 或相邻文档里已有的规则。
- **编号冲突处理**：多分支并行写文档时若撞号，后合并者负责重排并同步更新所有引用链接。
- **过时处理**：修改代码/架构若影响既有文档，同一次改动内必须同步更新对应权威文档；无法即时更新时，在文档头部标 `deprecated: <原因，指向替代文档>`。禁止留下与实现不符的文档。

### 交付物（deploy/）

- 按版本交付的脚本、SQL、配置模板放在对应版本目录：
  - `deploy/enterprise/` — 企业版自助部署的脚本、SQL baseline/migrations、`.env` 模板、CLI 工具等。
  - `deploy/saas/` — SaaS 版部署资产（如有）。
- **禁止**把版本专属脚本/SQL 放到仓库其它位置（如根目录、`scripts/` 等）。写脚本/SQL 前先查对应版本目录是否已有同功能产物。

### 中间产物分层（三层模型）

**第 1 层 —— durable（权威沉淀）**
进 `docs/` 或 `deploy/`。

**第 2 层 —— working（阶段性工作产物）**
进仓库的专用工作区目录，随任务生命周期存在：

- `openspec/changes/<change-id>/` —— OpenSpec 提案、规约 diff、任务清单。任务落地后按 OpenSpec 流程归档到 `openspec/specs/`。
- `docs/superpower/specs/`、`docs/superpower/plans/`、`docs/superpower/notes/` —— Superpowers 技能在本项目的工作区（收纳到 `docs/` 下统一管理）。

规则：工作区产物不是项目知识，不得被其它模块当作权威引用；要被长期引用的结论必须提炼进 `docs/`。

**第 3 层 —— ephemeral（一次性中间产物）**
进 OS 临时目录（`mktemp -d`）。

**分界**：有 skill 托管 → 第 2 层；AI 自发想记 → 第 3 层（temp，不进仓库）；反复查阅 → 第 1 层（`docs/`）。

### 禁止事项

- 禁止在任意位置散写项目知识（`notes.md` / `readme2.md` / 临时笔记等）。
- 禁止在 `docs/` 目录外新建 `*.md` 文档解释业务概念。**例外需在本规约中显式登记**，未登记视为违规。
- 禁止在多处复制粘贴同一段说明。只允许一处权威 + 其它地方链接。
- 禁止在仓库内散写没有 skill/workflow 托管的中间产物。AI 自发的 `plan.md` / `analysis.md` / `notes.md` 若不属于任何已登记工作区，一律去 OS 临时目录。
- 禁止在代码改动时"只改代码不动文档"，若对应权威文档被影响。

### 登记的例外

- `CLAUDE.md` / `AGENTS.md` —— 根目录元规则入口。
- 各 skill 目录下的 `skill.md` / `SKILL.md` —— skill 自带规范。
- `_dev_appendix.md` —— 研发调试临时笔记。
- `openspec/changes/<id>/`、`openspec/specs/` —— OpenSpec 技能工作区。
- `docs/superpower/` —— Superpowers 技能工作区（`specs/`、`plans/`、`notes/` 子目录）。
