# organizing-project-docs

A skill for AI coding agents: clean up a project whose documentation and `AGENTS.md` have turned into a mess — and keep them from turning back into one.

> 中文说明在下方。[English summary above.](#english)

## 它解决什么

「文档很乱」有四种不同的病，修法不同。这个 skill 先让你诊断，再动手：

| 症状 | 真问题 | 修法 |
| --- | --- | --- |
| 文档很多，不知道读哪个 | 没有分层：主路径与取证材料混在一起 | 按读者需要分物理目录 |
| 文档互相矛盾，改一处漏两处 | 同一事实有多个「家」 | 每个事实指定唯一归属 |
| 规则越写越多，没人遵守 | 规则不可判 | 能机检的进脚本，其余删掉 |
| 文档越来越长、越来越旧 | 把文档当历史记录 | 只描述 HEAD |

## 三条核心原则

1. **每类事实只有一个家**——第二个副本是 bug，不是冗余容忍。
2. **只描述当前状态**——历史交给 git；不写「以前是」「已改成」「勘误」。
3. **规则要么可判，要么不写**——写不成判据的规则是噪声。

## 内容

| 文件 | 作用 |
| --- | --- |
| [`SKILL.md`](SKILL.md) | 主文件：8 步流程 + 常见错误表 + 边界与代价 |
| [`agents-md-template.md`](agents-md-template.md) | 可直接复制的 `AGENTS.md` 模板，含元规则 M1–M5 |
| [`check_docs.py`](check_docs.py) | 可复用的机检脚本，退出码 0/1 |

## 安装

DeepSeek Harness（用户级，所有项目可用）：

```bash
git clone https://github.com/ChenXu233/organizing-project-docs \
  ~/.dsh/skills/organizing-project-docs
```

项目级（只对该仓库生效）：

```bash
git clone https://github.com/ChenXu233/organizing-project-docs \
  ./.dsh/skills/organizing-project-docs
```

其它支持 `AGENTS.md` / skills 约定的 agent：把 `SKILL.md` 作为该 agent 的技能说明，把 `agents-md-template.md` 与 `check_docs.py` 放在目标仓库的 `scripts/` 下即可。

## 自检脚本

```bash
python check_docs.py /path/to/repo
```

检查九项：H1 唯一、标题不跳级、标题 ≤4 级、围栏带语言、无盘符绝对路径、无连续 `---`、超 300 行有目录、相对链接有效、标题里无「勘误/变更记录/TODO/以前是/已废弃」。

禁词只查标题行——正文里讨论这些规则时会提到它们，属自指误报。

## English

A skills-format instruction set for coding agents that need to bring order to a project's `docs/` tree and `AGENTS.md`.

Core idea: **one home per fact, HEAD-only, and rules must be judgeable.** The skill walks through splitting human-facing `README.md` from agent-facing `AGENTS.md`, laying documents out by Diátaxis quadrant as *directories* (not filename numbers), pinning each fact to a single owner, deleting errata/changelogs/TODOs, keeping measurements in one registry, and enforcing the mechanical rules with a script.

It also includes the meta-rules for the rules file itself (approval required to add a rule; write stable mental models rather than volatile details; one rule in one place; delete rules that stop applying).

## Status and honesty

**This skill has not been validated by a failing-test baseline.** It was distilled from one real refactor of a repository whose docs had grown to nine files / 257 KB with roughly 28 KB of load-bearing content.

When you first use it, record which steps actually changed an agent's behavior versus which merely restated what the agent already does — then delete the latter (rule M5 in `SKILL.md`).

If a step never changes behavior, it is decoration, not instruction.

## License

[MIT](LICENSE)