# LLM Wiki Bootstrap

[English](./README.md)

一个既可安装、又带真实参考实现的 Skill，用来构建由 LLM 持续维护的 Markdown Wiki。

这个仓库对外发布时，最适合用两层结构来解释：

- `skill/` 是可以安装和分发的 Skill 包
- `llm-wiki/` 是基于该 Skill 创建出来、并且已经真实运行过一轮 ingest/query/lint 的示例 Wiki

## Quick Install（推荐）

```bash
npx skills add nanzhipro/Karpathy-llm-wiki-bootstrap-skill@llm-wiki-bootstrap
```

如果你希望用非交互方式做用户级安装：

```bash
npx skills add nanzhipro/Karpathy-llm-wiki-bootstrap-skill@llm-wiki-bootstrap -g -y
```

## 这个仓库实际发布了什么

### 1. 可安装的 Skill

Skill 位于 [skill](./skill)，它是整个仓库唯一的分发事实来源。

它负责为一个新 Wiki 生成：

- 原始资料层
- 由 LLM 维护的 `wiki/` 层
- `AGENTS.md`、`CLAUDE.md` 或 `SCHEMA.md` 这类 schema 文件
- ingest、query、lint 三类运行约定

### 2. 真实的参考实现

[llm-wiki](./llm-wiki) 现在不是占位目录，而是一个真实示例。它就是基于这个 Skill 创建出来，并且已经被实际运行、维护和“编译”为可浏览 Wiki 的参考实现。

它完整展示了这套模式落地之后的样子：

- 原始资料在 [llm-wiki/raw](./llm-wiki/raw)
- Agent 运行契约在 [llm-wiki/AGENTS.md](./llm-wiki/AGENTS.md)
- 维护后的 Wiki 页面在 [llm-wiki/wiki](./llm-wiki/wiki)

对外解释时，最清晰的口径是：

- 如果你想自己生成 Wiki，就安装 `skill/`
- 如果你想看这套模式真实跑出来是什么样子，就打开 `llm-wiki/`

## 为什么需要这个模式

大多数 LLM 文档工作流都停留在 RAG：上传文件、查询时临时检索、每次都重新拼装答案。它能用，但不会积累。

这个 Skill 封装的是另一种方式：

- `raw/` 保持不可变
- Agent 把知识逐步“编译”进 Wiki
- Wiki 变成一个会持续生长的知识制品
- 有价值的回答可以反向归档回 Wiki，而不是消失在聊天记录里

## 系统模型

如果从发布视角完整介绍，这个仓库其实有四层：

| 层级 | 位置 | 角色 |
| --- | --- | --- |
| Skill 包 | `skill/` | 可复用的 bootstrap 逻辑与模板 |
| 原始资料层 | `raw/` | 不可变证据层 |
| Schema 层 | `AGENTS.md` / `CLAUDE.md` / `SCHEMA.md` | Agent 的操作契约 |
| Wiki 页面层 | `wiki/` | 持续维护的知识层 |

Skill 包负责生成后三层。`llm-wiki/` 则展示了这个过程已经发生之后的结果。

## 参考示例：`llm-wiki/`

当前示例 Wiki 的结构大致如下：

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

几个最值得先看的入口：

- [llm-wiki/AGENTS.md](./llm-wiki/AGENTS.md) 展示生成后的 Agent 运行契约
- [llm-wiki/wiki/index.md](./llm-wiki/wiki/index.md) 展示 Agent 如何导航整个知识库
- [llm-wiki/wiki/log.md](./llm-wiki/wiki/log.md) 展示时间顺序上的操作历史
- [llm-wiki/wiki/overview.md](./llm-wiki/wiki/overview.md) 展示当前的顶层综合判断

这个示例背后的 canonical 原始理念文档位于仓库顶层：[karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md)。

## 示例的理念来源与原始语料

这个参考 Wiki 的 `raw/` 不是随便塞的演示内容，而是建立在 Karpathy 的 llm-wiki 原始理念之上。这个原始理念的 canonical 文本就是仓库顶层的 [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md)。

也就是说，这个示例本身就在演示：如何把一篇原始理念文档，真正转换成一个可维护、可增长的知识系统。

当前示例的原始资料层包括：

- [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md)，作为原始理念的 canonical 文本
- [llm-wiki/raw/llm-wiki-pattern.md](./llm-wiki/raw/llm-wiki-pattern.md)，作为基于这份原始理念整理后的示例原始语料
- [llm-wiki/raw/Karpathy x.md](./llm-wiki/raw/Karpathy%20x.md)，展示新资料如何继续被摄入同一个演化中的 Wiki

对外发布时，推荐这样系统表述：

- Skill 负责把方法封装起来
- `llm-wiki/` 负责展示这套方法已经跑起来后的真实样子
- 原始语料从 [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) 出发，再继续扩展更多资料

## 推荐的部署结构

建议把 `.agent/skills/` 作为这个 Skill 的统一安装位置。对于 Claude、Codex 或其他需要单独发现目录的运行时，不要复制多份 Skill，而是通过符号链接回到同一份已安装内容。

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

核心原则是：保留一份真实安装副本，其余运行时都链接回它。

## Skill 会生成什么

当用户安装这个 Skill 并创建一个新 Wiki 时，生成结构如下：

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

## 三类核心操作

| 操作 | 触发方式 | 结果 |
| --- | --- | --- |
| Ingest | `"ingest raw/{file}"` | 把资料转成摘要、实体、概念、链接、索引更新和日志记录 |
| Query | 直接提领域问题 | 先读索引，再读相关页面，最后输出带引用的综合回答 |
| Lint | `"lint"` / `"health check"` | 检查矛盾、过期结论、孤儿页和缺失链接 |

## Schema 文件命名规则

| Agent | Schema 文件名 |
| --- | --- |
| Claude Code | `CLAUDE.md` |
| OpenAI Codex | `AGENTS.md` |
| Copilot (VS Code) | `.github/copilot-instructions.md` |
| 其他 / 通用 | `SCHEMA.md` |

运行逻辑一致，只是文件名适配不同 Agent 的约定。

## 仓库结构

| 路径 | 用途 |
| --- | --- |
| [skill/SKILL.md](./skill/SKILL.md) | 可安装的 Skill 定义 |
| [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) | 作为示例语料起点的原始理念文档 |
| [skill/references/templates](./skill/references/templates) | bootstrap 过程中用到的模板 |
| [skill/references/workflows](./skill/references/workflows) | ingest、query、lint 的详细工作流参考 |
| [llm-wiki/AGENTS.md](./llm-wiki/AGENTS.md) | 示例 Wiki 的 Agent 指令文件 |
| [llm-wiki/raw](./llm-wiki/raw) | 示例原始资料层 |
| [llm-wiki/wiki](./llm-wiki/wiki) | 示例编译后的 Wiki 输出层 |

## 对外口径

如果你要对外用一句话介绍这个项目，最顺手的版本是：

> `LLM Wiki Bootstrap` 是一个可安装的 Skill，用来创建由 LLM 持续维护的 Markdown Wiki；仓库同时附带一个真实的 `llm-wiki/` 参考实现，展示这套模式如何在以 [karpathy-llm-wiki-original.md](./karpathy-llm-wiki-original.md) 为起点的原始语料上真正运行起来。
