# reflect model configuration

One line per role. Delete a line (or this file) to inherit the parent model.

```
# judgment: <model selector>
# tooling: <model selector>
```

Resolve a valid selector with `await rlm.find_models(...)` and pass the exact returned selector as `model=` when spawning. `inherit-parent` or omitting `model` runs the child on the parent chat model.
