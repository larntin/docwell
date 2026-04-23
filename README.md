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
3. 按项目实际情况改三处：
   - 第 1 节：把 `docs/` / `deploy/` 换成你项目真实的目录名。
   - 第 2 节：选编号方式（`${num}_` 前缀 或 日期后缀），全项目统一。
   - 第 6 节：把你用的 skill/workflow 工作区（OpenSpec / Superpowers / 等）登记上去。
4. 提交。后续 AI session 读到根 `CLAUDE.md` 时会自动遵守。

想看一份"已特化到具体项目"的成品对照，见 [`examples/docs-enterprise-saas.md`](./examples/docs-enterprise-saas.md)。

## 核心模型：三层中间产物

这是 docwell 最独特的一条 —— 把"AI 产出的东西去哪"严格分三层：

| 层 | 去哪 | 判定 | 典型 |
|----|------|------|------|
| **1. durable** | `docs/` 或 `deploy/` | 未来 session 会反复查阅 | 架构决策、踩坑记录、部署脚本 |
| **2. working** | 仓库内**已登记**的 skill 工作区 | 由某个 skill/workflow 托管的阶段性产物 | OpenSpec `openspec/changes/<id>/`、Superpowers `specs/`/`plans/`/`notes/` |
| **3. ephemeral** | OS 临时目录（`mktemp -d`） | 本 session 消化完即扔 | 单次 grep 缓存、subagent 中间文本 |

关键分界：
- 有 skill 托管 → 第 2 层（进仓库）
- 没 skill 托管、AI 自发想记 → 第 3 层（去 temp，**不准进仓库**）
- 任务结束后还要被反复查阅 → 第 1 层（`docs/`）

这套划分解决了"AI 没完没了在仓库里建 `plan.md`"的常见痛点，同时不掐死 OpenSpec / Superpowers 这些把工作产物放仓库的成熟技能。

## 文件结构

```
docwell/
├── README.md                            # 本文
├── docwell.md                           # 通用规约（粘贴到 CLAUDE.md / AGENTS.md）
├── examples/
│   └── docs-enterprise-saas.md          # 针对 docs/ + enterprise/ + saas/ + deploy/ 结构的特化示例
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
