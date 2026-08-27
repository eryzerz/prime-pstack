# swarm model configuration

One line per role. Delete a line (or this file) to inherit the parent model.

```
# workers: <model selector 1> <model selector 2> <model selector 3> ...
```

`workers` is a list: one worker subagent runs per entry; for a model race, each arm's selector is named up front from the list. Resolve valid selectors with `await rlm.find_models(...)` and pass exact returned selectors as `model=`. `inherit-parent` or omitting `model` runs the child on the parent chat model.
