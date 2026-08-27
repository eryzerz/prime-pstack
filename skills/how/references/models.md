# how model configuration

One line per role. Delete a line (or this file) to inherit the parent model.

```
# explorer: <model selector>
# explainer: <model selector>
# critics: <model selector 1> <model selector 2> <model selector 3> <model selector 4>
```

`explorer`: model for the parallel exploration agents on complex questions. `explainer`: model for direct-explain and synthesis agents. `critics`: list for Critique Mode — one critic subagent runs per entry. Resolve valid selectors with `await rlm.find_models(...)` and pass the exact returned selector as `model=` when spawning. `inherit-parent` or omitting `model` runs the child on the parent chat model.
