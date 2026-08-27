# architect model configuration

One line per role. Delete a line (or this file) to inherit the parent model.

```
# runners: <model selector 1> <model selector 2>
```

`runners` is a list: one runner subagent runs per entry. Resolve valid selectors with `await rlm.find_models(...)` and pass exact returned selectors as `model=` on the arena spawns.
