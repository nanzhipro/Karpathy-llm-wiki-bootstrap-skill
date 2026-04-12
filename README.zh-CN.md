# Karpathy LLM Wiki Bootstrap

[English](./README.md)

一个可安装的 Skill，加上一个真实运行中的参考实现，用来构建由 LLM 持续维护的 Markdown Wiki。

![Karpathy LLM Wiki Bootstrap 封面图](./cover-image/llm-wiki/cover.png)

这个仓库最值得看的，不只是一个可以初始化 Wiki 的 Skill，而是**一个已经跑起来的样本**。从 [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) 出发，LLM 把原始材料逐步编译进 [llm-wiki/](./llm-wiki)，长出 [index](./llm-wiki/wiki/index.md)、[log](./llm-wiki/wiki/log.md)、概念页、对比页和综合页。它展示的不是“如何做一次总结”，而是“如何把一篇材料变成一个**可维护的知识工件**”。

如果你在学习一篇文章、论文、调研报告或一本书，这套模式的关键是把维护劳动交给 LLM：

- 原始资料保留在 `raw/`
- 结构化理解沉淀在 `wiki/`
- 后续通过 **ingest**、**query**、**lint** 持续扩展和校正

> **完整展开版 →** [不是读完就算：如何把一篇文章编译成一个能长期记住它的 LLM Wiki](./from-article-to-llm-wiki.article.zh-CN.md)  ·  [English](./from-article-to-llm-wiki.article.en.md)

## 仓库包含什么

这个仓库由两部分组成，而且两者是配套设计的：

- [skill/](./skill) 是可安装、可分发的 Skill 包，用来初始化你自己的 Wiki。
- [llm-wiki/](./llm-wiki) 是基于该 Skill 创建出来，并持续经过 ingest、query、lint 维护的真实示例 Wiki。

这样的拆分是有意为之的：`skill/` 是可复用的产品本体，`llm-wiki/` 则负责展示这套模式真正跑起来之后是什么样子。

## Quick Install

推荐安装方式：

```bash
npx skills add nanzhipro/Karpathy-llm-wiki-bootstrap-skill@llm-wiki-bootstrap
```

如果你希望用非交互方式做用户级安装：

```bash
npx skills add nanzhipro/Karpathy-llm-wiki-bootstrap-skill@llm-wiki-bootstrap -g -y
```

## 第一次运行示例

下面是一条最小可跑通的首次使用路径，起点就是 [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md)。

这个示例默认你使用的是 OpenAI Codex，所以生成的 schema 文件会是 `AGENTS.md`。如果你选择的是 Claude Code，把同一步里的 `AGENTS.md` 换成 `CLAUDE.md` 即可。

1. 在你的 agent 里触发这个 Skill：

   > `bootstrap a wiki`

2. 当 Skill 询问初始化问题时，可以这样选择：

   - Domain：`Research topic`
   - Wiki name：`llm-wiki-demo`
   - Agent：`OpenAI Codex`
   - Editor：`Obsidian`
   - Source types：`Web articles`
   - Output location：`Current directory`

3. 等 Wiki 脚手架生成后，把这份原始理念文档复制到新 Wiki 的 `raw/` 目录：

   ```bash
   cp karpathy-llm-wiki-original.md llm-wiki-demo/raw/
   ```

4. 然后对 agent 说：

   > `Read llm-wiki-demo/AGENTS.md, then ingest llm-wiki-demo/raw/karpathy-llm-wiki-original.md`

5. 第一次 ingest 完成后，重点查看这几个文件：

   - `llm-wiki-demo/wiki/index.md`
   - `llm-wiki-demo/wiki/log.md`
   - `llm-wiki-demo/wiki/overview.md`

跑完第一轮之后，你通常会看到：

- `wiki/sources/` 下生成了一页 source summary
- 如果 agent 识别出了关键概念或实体，还会创建新的概念页或实体页
- `index`、`log` 和 `overview` 都会被同步更新

如果你想先看一份已经跑完的结果，再决定自己动手，可以直接打开 [llm-wiki/](./llm-wiki)。

一个很实用的小 tips：

如果你想从一开始就构建中文 Wiki，在调用时直接对 agent 说：

> `使用中文编译 karpathy-llm-wiki-original.md`

## 为什么要用这种模式

现在大多数 LLM 文档工作流，本质上还是 RAG：上传文件、提问时临时检索若干片段、再从头拼出答案。它能解决问题，但不会沉淀结构。

这个项目封装的是另一种做法：

- 原始资料始终保持**不可变**
- Agent 持续把知识“**编译**”进 Wiki
- Wiki 会成为一个**不断增长的长期知识制品**
- 有价值的回答可以继续归档回 Wiki，而不是消失在聊天记录里

**结果就是：**知识会持续累积，而不是每次提问都从零开始。

## 系统模型

完整来看，这个系统有四层：

| 层级 | 位置 | 角色 |
| --- | --- | --- |
| Skill 包 | `skill/` | bootstrap 逻辑、模板和工作流规则 |
| 原始资料层 | `raw/` | 不可变的证据层 |
| Schema 层 | `AGENTS.md` / `CLAUDE.md` / `SCHEMA.md` | Agent 的操作契约 |
| Wiki 页面层 | `wiki/` | 持续维护的知识层 |

Skill 负责在一个新 Wiki 里生成后三层。

`llm-wiki/` 则展示了这套机制已经实际运行过之后的结果。

## 参考 Wiki

[llm-wiki/](./llm-wiki) 不是占位内容，而是一个真实的参考实现。它确实是从这个 Skill 生成出来的，而且已经作为一个活的 Wiki 被维护过。

当前结构如下：

```text
llm-wiki/
├── AGENTS.md
├── raw/
│   ├── Karpathy x.md
│   └── llm-wiki-pattern.md
└── wiki/
    ├── index.md
    ├── log.md
    ├── overview.md
    ├── concepts/
    ├── entities/
    ├── comparisons/
    ├── sources/
    └── synthesis/
```

建议先看这几个入口：

- [llm-wiki/AGENTS.md](./llm-wiki/AGENTS.md)，看生成后的 Agent 指令
- [llm-wiki/wiki/index.md](./llm-wiki/wiki/index.md)，看 Agent 如何导航整个知识库
- [llm-wiki/wiki/log.md](./llm-wiki/wiki/log.md)，看按时间顺序记录的操作历史
- [llm-wiki/wiki/overview.md](./llm-wiki/wiki/overview.md)，看当前阶段的顶层综合判断

如果你想最快理解这套模式，直接看 `llm-wiki/` 是最直观的方式。

## 来源与语料脉络

这套思路来自 Karpathy 最初提出的 LLM Wiki 原始笔记：

- 原始 gist：<https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f>
- 仓库内副本：[karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md)

当前示例 Wiki 就是建立在这份原始想法之上的。语料脉络如下：

| 语料 | 角色 |
| --- | --- |
| [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) | 原始理念的仓库内参考副本 |
| [llm-wiki/raw/llm-wiki-pattern.md](./llm-wiki/raw/llm-wiki-pattern.md) | 基于理念整理的示例原始语料 |
| [llm-wiki/raw/Karpathy x.md](./llm-wiki/raw/Karpathy%20x.md) | 展示新资料如何被吸收进演化中的 Wiki |

对外介绍时，最清晰的说法是：

1. **Skill** 负责把方法封装成可安装的能力
2. **示例 Wiki** 负责展示这套方法已经真实跑起来的效果
3. **语料** 以 Karpathy 的原始想法为起点，再继续向外扩展

## 推荐安装布局

建议把 `.agent/skills/` 作为统一安装位置。

对于 Claude、Codex 或其他需要单独发现目录的运行时，不要复制多份内容，而是把它们链接回同一份已安装 Skill。

```text
.agent/
└── skills/
    └── llm-wiki-bootstrap/
        ├── SKILL.md
        └── references/
```

符号链接示例：

```bash
ln -s /absolute/path/to/.agent/skills/llm-wiki-bootstrap ~/.claude/skills/llm-wiki-bootstrap
ln -s /absolute/path/to/.agent/skills/llm-wiki-bootstrap ~/.codex/skills/llm-wiki-bootstrap
```

> **原则很简单：**只保留一份真实安装副本，其余运行时都回链到它。

---

## Skill 会生成什么

当你用这个 Skill 初始化一个新 Wiki 时，生成结构如下：

```text
{wiki-name}/
├── raw/
├── wiki/
│   ├── index.md
│   ├── log.md
│   └── overview.md
├── {schema-file}
└── .gitignore
```

不同运行时对应的 schema 文件名如下：

| Agent | Schema 文件名 |
| --- | --- |
| Claude Code | `CLAUDE.md` |
| OpenAI Codex | `AGENTS.md` |
| Copilot (VS Code) | `.github/copilot-instructions.md` |
| 其他 / 通用 | `SCHEMA.md` |

只有文件名会变，运行模型本身是一致的。

---

## 三类核心操作

| 操作 | 触发方式 | 结果 |
| --- | --- | --- |
| Ingest | `"ingest raw/{file}"` | 把资料转成摘要、实体、概念、链接、索引更新和日志记录 |
| Query | 直接提领域问题 | 先读索引，再读相关页面，最后输出带引用的综合回答 |
| Lint | `"lint"` 或 `"health check"` | 检查矛盾、过期结论、孤儿页和缺失链接 |

## 仓库结构

| 路径 | 用途 |
| --- | --- |
| [skill/SKILL.md](./skill/SKILL.md) | 可安装的 Skill 定义 |
| [skill/references/templates](./skill/references/templates) | bootstrap 过程中使用的模板 |
| [skill/references/workflows](./skill/references/workflows) | ingest、query、lint 的详细工作流参考 |
| [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) | 原始理念笔记的仓库内副本 |
| [llm-wiki/AGENTS.md](./llm-wiki/AGENTS.md) | 示例 Wiki 的 Agent 指令文件 |
| [llm-wiki/raw](./llm-wiki/raw) | 示例原始资料层 |
| [llm-wiki/wiki](./llm-wiki/wiki) | 示例编译后的 Wiki 输出层 |

## 一句话定位

`Karpathy LLM Wiki Bootstrap` 是一个可安装的 Skill，用来创建由 LLM 持续维护的 Markdown Wiki，同时附带一个真实的 `llm-wiki/` 参考实现，展示这套模式如何以 Karpathy 的原始 LLM Wiki 思路为起点真正运行起来。
