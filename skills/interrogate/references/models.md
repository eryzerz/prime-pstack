# interrogate model configuration

One line per role. Delete a line (or this file) to inherit the parent model.

```
# reviewers: <model selector 1> <model selector 2> <model selector 3> <model selector 4>
```

`reviewers` is a list: one reviewer subagent runs per entry, labeled Reviewer A, B, C, D, ... in order. Resolve valid selectors with `await rlm.find_models(...)` and pass exact returned selectors as `model=`. `inherit-parent` or omitting `model` runs the child on the parent chat model.
