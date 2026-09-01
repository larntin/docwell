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

1. 把 `.tmp/` 加进项目 `.gitignore`（规约第 3 层产物的落点，绝不进版本库）。
2. 打开 [`docwell.md`](./docwell.md)，整段复制，粘到项目根 `CLAUDE.md` 或 `AGENTS.md` 顶部。
3. **只改第 5 节**：把本项目实际使用的 skill/workflow 工作区（OpenSpec / Superpowers / 等）登记上去，没用到的删掉。其余各节通用，不必改。
4. 建 `docs/000_index.md` 作为文档索引入口。
5. 提交。后续 AI session 读到根 `CLAUDE.md` 时会自动遵守。

## 核心模型：三层中间产物

docwell 最独特的一条 —— 把「AI 产出的东西去哪」严格分三层，判据是**寿命**：

| 层 | 去哪 | 判定 | 典型 |
|----|------|------|------|
| **1. durable** | `docs/` | 未来 session 会反复查阅 | 架构决策、踩坑记录、约定 |
| **2. working** | 规约第 5 节**已登记**的 skill 工作区 | 由某个 skill/workflow 托管的阶段性产物 | OpenSpec `openspec/changes/<id>/`、Superpowers `specs/`/`plans/`/`notes/` |
| **3. ephemeral** | 项目 `.tmp/`（gitignore） | 本 session 消化完即扔 | 单次 grep 缓存、subagent 中间文本 |

这套划分解决了「AI 没完没了在仓库里建 `plan.md`」的常见痛点，同时不掐死 OpenSpec / Superpowers 这些把工作产物放仓库的成熟技能。**未登记的目录不是工作区** —— 想加新技能，去第 5 节登记，而不是改正文。

细节见 [`docwell.md`](./docwell.md) 第 4、5 节。

## 另外三条容易被低估的规则

- **`000_index.md` 强制同步** —— 扁平目录超过几十篇后，只靠 grep 必然漏。新增/删除/改名文档必须同一次改动内更新索引。
- **编号永不重排** —— 撞号时后来者取下一个空号。重排会让所有外部引用集体失效，代价远大于编号连续的收益。
- **只写代码里读不出来的东西** —— 函数签名、目录结构、依赖清单一律不写进 `docs/`。这类内容腐烂最快，是「文档比没文档更糟」的主要来源。

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
