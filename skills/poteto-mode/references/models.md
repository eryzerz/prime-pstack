# poteto-mode model configuration

One line per role. Delete a line (or this file) to inherit the parent model.

```
# feature, refactoring: <model selector>   # fast code model for trivial mechanical edits
# bug-fix: <model selector>                # instruction-following model for precisely specified fix scope
# perf-issue: <model selector>             # instruction-following model for precisely specified fix scope
# hillclimb: <model selector>              # instruction-following model for tightly scoped attempts
# judgment and prose: <model selector>     # judgment and prose work
# hardest tasks: <model selector>          # strongest judgment model: design, concurrency, subtle algorithms
```

Resolve valid selectors with `await rlm.find_models(...)` and pass the exact returned selector as `model=` when spawning. `inherit-parent` or omitting `model` runs the child on the parent chat model.
