# AutoSR

[English](README.md) | 中文

AutoSR ([论文](https://arxiv.org/abs/2608.16876)) 是一个用于符号 ansatz 搜索的 Claude Code 插件.
问题和生成出的产物放在插件之外, 通常在一个 problem workspace 里, 例如 `artifacts/pile_efficiency/`.
`artifacts/` 目录用来存放 problem workspace; 仓库里只追踪它的 README, 数据, 运行记录和生成产物都只留在本地.

## 以插件方式启动 Claude Code

在一个 problem workspace 里:

```
cd /path/to/problem-workspace
claude --plugin-dir /path/to/AutoSR/agonsr --dangerously-skip-permissions --model claude-sonnet-5[1m]
```

然后运行 `/llm-mcts 10 problem.md`.

恢复一次已有的运行:

```
/llm-mcts 10 problem.md --resume runs/llm-mcts_YYMMDD_HHMM
```

## `problem.md`

`problem.md` 里应该写实际的问题定义: 目标, 变量, 要读的数据/文档/脚本, 约束, 评测方法, 评分标准, 以及期望的 `ansatz.md` 内容. 路径用相对于 problem workspace 的相对路径.

## `IGNOREME.md`

可选. 当某些针对特定角色的说明不适合写进通用问题描述时, 放在这里. 格式:

```
## Notes to ansatz-proposer

## Notes to ansatz-reviewer

## Notes to dispatcher

```

dispatcher 会读这个文件, 并且只把各段分别传给对应的子 agent.

## 流水线

`llm-mcts` 只是一个 dispatcher. 它初始化或恢复一次运行, 向 `mcts.py next` 要下一个候选, 给 `ansatz-proposer` 和 `ansatz-reviewer` 发固定的最小 prompt, 从 `ansatz.md` 里读取 `<review score="X">` 块, 调用 `mcts.py update`, 最后展示最好的若干候选.

运行文件写在 `runs/` 下. 确切的 workspace 和 ansatz 文件约定见 `agonsr/references/project_manual.md`.

## 引用

```bibtex
@misc{zhang2026autosrautomaticsymbolicregression,
      title={AutoSR: Automatic Symbolic Regression by Searching Research States},
      author={Kejia Zhang and Youran Sun and Xinyu Ren and Chugang Yi and Haizhao Yang},
      year={2026},
      eprint={2608.16876},
      archivePrefix={arXiv},
      primaryClass={cs.SC},
      url={https://arxiv.org/abs/2608.16876},
}
```
