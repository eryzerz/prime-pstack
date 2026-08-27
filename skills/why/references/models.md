# why model configuration

One line per role. Delete a line (or this file) to inherit the parent model.

```
# investigators: <model selector>
# synthesizer: <model selector>
```

`investigators`: applies to every parallel evidence-category investigator. `synthesizer`: model for the final synthesizer subagent. Resolve a valid selector with `await rlm.find_models(...)` and pass the exact returned selector as `model=` when spawning. `inherit-parent` or omitting `model` runs the child on the parent chat model.
