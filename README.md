# AgonSR

AgonSR is a Claude Code plugin for symbolic ansatz search.
Problems and generated artifacts live outside the plugin, usually in a problem workspace such as `artifacts/pile_efficiency/`.
The [`artifacts/`](https://github.com/WhymustIhaveaname/AutoSR-artifacts) directory is a separate git submodule/repo for problem workspaces, data, runs, and generated outputs.

## Start Claude Code with the plugin

From a problem workspace:

```
cd /path/to/problem-workspace
claude --plugin-dir /path/to/AgonSR/agonsr --dangerously-skip-permissions --model claude-sonnet-5[1m]
```

Then run `/llm-mcts 10 problem.md`.

Resume an existing run:

```
/llm-mcts 10 problem.md --resume runs/llm-mcts_YYMMDD_HHMM
```

## `problem.md`

`problem.md` should contain the actual problem definition: objective, variables, data/docs/scripts to read, constraints, evaluation method, scoring rubric, and expected `ansatz.md` contents. Use paths relative to the problem workspace.

`problem.md` must also give the score scale. All reviews in the project use that
scale. Then you can compare the scores of different candidates and of different
runs.

Give the scale. Also give an example of the calculation:

> The `<review score="X">` value must be the TEST balanced accuracy as a
> percentage. If the accuracy is `0.9732`, write `<review score="97.32">`.

Some scoring rules have no limits, because they have penalties or logarithmic
terms. If a scoring rule has no limits, write this in `problem.md`. Do not
invent limits. Each round must use the same scale. The scores do not have to be
between two given values.

## `IGNOREME.md`

Optional. Put special per-role notes here when they should not live in the general problem statement. Format:

```
## Notes to ansatz-proposer

## Notes to ansatz-reviewer

## Notes to dispatcher

```

The dispatcher reads this file and passes each section only to the corresponding subagent.

## Pipeline

`llm-mcts` is only a dispatcher. It initializes or resumes a run, asks `mcts.py next` for the next candidate, sends fixed minimal prompts to `ansatz-proposer` and `ansatz-reviewer`, reads the `<review score="X">` block from `ansatz.md`, calls `mcts.py update`, and finally shows the best candidates.

Run files are written under `runs/`. See `agonsr/references/project_manual.md` for the exact workspace and ansatz-file conventions.
