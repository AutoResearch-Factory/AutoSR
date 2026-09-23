# artifacts

Problem workspaces live here, one directory per problem. Nothing in this directory except this README is tracked.

```
artifacts/
└── <problem>/
    ├── .venv/                      # shared venv for the problem
    ├── problem.md                  # the problem statement /llm-mcts reads
    ├── ...                         # data, evaluation scripts, docs referenced by problem.md
    └── runs/
        ├── llm-mcts_YYMMDD_HHMM/
        └── llm-sr_YYMMDD_HHMM/
```

See `agonsr/references/project_manual.md` for the run layout and the ansatz file format.
