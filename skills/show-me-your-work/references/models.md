# show-me-your-work model configuration

One line per role. Delete a line (or this file) to inherit the parent model.

```
# reviewer: <model selector>
```

Resolve a valid selector with `await rlm.find_models(...)` and pass the exact returned selector as `model=` when spawning. Omitting `model` runs the child on the parent chat model.
