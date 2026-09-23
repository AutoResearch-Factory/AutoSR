# AutoSR

[![Project page](https://img.shields.io/badge/project-page-1f6feb.svg)](https://autosr.app) [![arXiv](https://img.shields.io/badge/arXiv-2608.16876-b31b1b.svg)](https://arxiv.org/abs/2608.16876) [![Claude Code](https://img.shields.io/badge/Claude%20Code-plugin-8A2BE2.svg)](https://claude.com/claude-code)

[English](README.md) | 中文

AutoSR ([论文](https://arxiv.org/abs/2608.16876)) 是一个用于符号 ansatz 搜索的 Claude Code 插件.
AutoSR 模拟人类进行科学发现的流程通过在 Research State 中搜索进行符号回归.

## 在网页上直接使用

直接登录 [autosr.app](https://autosr.app) 即可开始使用!

## 以 Claude Code 插件方式启动

问题和生成出的产物放在插件之外, 通常在一个 problem workspace 里, 例如 `artifacts/pile_efficiency/`.
`artifacts/` 目录用来存放 problem workspace; 仓库里只追踪它的 README, 数据, 运行记录和生成产物都只留在本地.

在一个 problem workspace 里:

```
cd /path/to/problem-workspace
claude --plugin-dir /path/to/AutoSR/agonsr --dangerously-skip-permissions
```

然后运行 `/llm-mcts 10 problem.md`.

恢复一次已有的运行:

```
/llm-mcts 10 problem.md --resume runs/llm-mcts_YYMMDD_HHMM
```

### `problem.md`

`problem.md` 里应该写实际的问题定义: 目标, 变量, 要读的数据/文档/脚本, 约束, 评测方法, 评分标准, 以及期望的 `ansatz.md` 内容. 路径用相对于 problem workspace 的相对路径.

### `IGNOREME.md`

可选. 当某些针对特定角色的说明不适合写进通用问题描述时, 放在这里. 格式:

```
## Notes to ansatz-proposer

## Notes to ansatz-reviewer

## Notes to dispatcher

```

dispatcher 会读这个文件, 并且只把各部分分别传给对应的子 agent.

## 流水线

`llm-mcts` 是一个 dispatcher. 它初始化或 resume 一次运行, 从 `mcts.py next` 获得下一个候选, 给 `ansatz-proposer` 和 `ansatz-reviewer` 发固定的最小 prompt, 从 `ansatz.md` 里读取 `<review score="X">` 块, 调用 `mcts.py update`, 最后展示最好的若干候选.

运行文件写在 `runs/` 下. 确切的 workspace 和 ansatz 文件约定见 `agonsr/references/project_manual.md`.

## 引用

```
@misc{zhang2026autosrautomaticsymbolicregression,
      title={AutoSR: Automatic Symbolic Regression by Searching Research States},
      author={Youran Sun and Kejia Zhang and Xinyu Ren and Chugang Yi and Haizhao Yang},
      year={2026},
      eprint={2608.16876},
      archivePrefix={arXiv},
      primaryClass={cs.SC},
      url={https://arxiv.org/abs/2608.16876},
}
```
