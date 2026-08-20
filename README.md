# docwell

> A drop-in Markdown ruleset that keeps AI-assisted projects' knowledge **single-source, discoverable, and non-rotting** — paste into any `CLAUDE.md` / `AGENTS.md`.
>
> 一份可直接粘进 `CLAUDE.md` / `AGENTS.md` 的文档归一化规约 —— 让 AI 协作项目的知识唯一权威、可被发现、不腐烂。

## Why

AI 协作的项目有三个反复出现的病：

1. **知识散写** —— AI 随手在 `notes.md` / `plan.md` / `readme2.md` 里留东西，同主题在多处并存并逐渐矛盾。
2. **无法发现** —— 即使文档写好了，新 session 不知道去哪查，每次都重新推导。
3. **静默腐烂** —— 代码改了，文档没同步，最后比没文档更误导人。

docwell 是一份**最简规约**，把这三个问题一次性堵住。核心就一句话：**唯一权威 + 可被发现 + 过时必处理**。

## Quickstart

1. 打开 [`docwell.md`](./docwell.md)，整段复制。
2. 粘到你项目根的 `CLAUDE.md` 或 `AGENTS.md` 顶部。
3. 按项目实际情况微调：
   - 第 1、2 节：`docs/` 用 `${NNN}_`（`001_`、`002_`…）**三位编号**的扁平 `.md` 文件，类型/领域写进文件名、不建子目录，一般无需改动。
   - 第 5 节：把你用的 skill/workflow 工作区（OpenSpec / Superpowers / 等）登记上去。
   - 第 6 节：UI 设计稿留痕。项目没有 UI 协作就整节删除。
4. 提交。后续 AI session 读到根 `CLAUDE.md` 时会自动遵守。

## 核心模型：三层中间产物

这是 docwell 最独特的一条 —— 把"AI 产出的东西去哪"严格分三层：

| 层 | 去哪 | 判定 | 典型 |
|----|------|------|------|
| **1. durable** | `docs/` | 未来 session 会反复查阅 | 架构决策、踩坑记录、约定、UI 设计稿 |
| **2. working** | 仓库内**已登记**的 skill 工作区 | 由某个 skill/workflow 托管的阶段性产物 | OpenSpec `openspec/changes/<id>/`、Superpowers `specs/`/`plans/`/`notes/` |
| **3. ephemeral** | 项目 `.tmp/`（gitignore） | 本 session 消化完即扔 | 单次 grep 缓存、subagent 中间文本 |

关键分界：
- 有 skill 托管 → 第 2 层（进仓库）
- 没 skill 托管、AI 自发想记 → 第 3 层（去项目 `.tmp/`，已 gitignore，**不进版本库**）
- 任务结束后还要被反复查阅 → 第 1 层（`docs/`）

这套划分解决了"AI 没完没了在仓库里建 `plan.md`"的常见痛点，同时不掐死 OpenSpec / Superpowers 这些把工作产物放仓库的成熟技能。

## UI 设计稿留痕（第 6 节）

多个 AI 同事轮流改同一份设计稿时，最常见的坏结局是：`登录页-v2-final-真final.html` 堆了一屏，没人知道哪份算数。第 6 节用一条命名规则把这件事钉死：

```
docs/${NNN}-${名称}-${版本}-${YYYY-MM-DD}-${agent名称}.html

docs/110-登录流程交互-定稿1.4-2026-08-23-claude.html
docs/111-登录流程视觉-2.0-2026-08-25-claude.html
```

- **交互稿在前，视觉稿在后**：交互稿只谈功能与流程（可以画框线示意，但不准定颜色字体）；视觉稿等交互稿定稿了才开始。相邻编号成对（`110` 交互 / `111` 视觉），从 `110` 起。
- **一份稿只有一个文件**：调整就 `git mv` 改名（版本号、日期、agent 名一起换），旧文件不留。**留痕靠文件名 + git 历史，不靠堆文件。**
- **谁都能接手**：agent 名字段记的是"这一版是谁改的"，版本号全局递增、不分叉。
- **定稿只是标记不是冻结**：版本号前加"定稿"二字；要再改就去掉两个字继续递增。
- 大版本（`1.x` → `2.0`）**由人指定**，AI 不许自己升。

## 文件结构

```
docwell/
├── README.md      # 本文
├── docwell.md     # 通用规约（粘贴到 CLAUDE.md / AGENTS.md）
└── LICENSE
```

## 适用范围

- 任何期望长期维护、多人协作的项目。
- 尤其适合**大量使用 Claude Code / Codex / Cursor 等 AI 协作**的项目，这些 AI 在没有约束时最爱散写 `.md` 文件。
- 不适合一次性脚本仓库或纯玩具项目 —— 规约本身的心智开销不值得。

## 贡献

有更好的分层、更简洁的表述、发现反例都欢迎 issue / PR。规约文件本身追求**最短、最不容易被 AI 误解、最容易粘贴**，不追求覆盖所有边缘情况。

## License

MIT
