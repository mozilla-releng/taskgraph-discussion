# Task labels

Per discussion with Tom today (2020.06.16):

- We should set the `task.name` generically
- Variants should append suffixes to `task.name` dynamically in transforms, rather than by string formatting
- The job transform will prepend the `task.kind` to the label, making the final label `{kind}-{name}-{variant}`.
- Since not all tasks use the job transform, we may want to transform the label in the task transform.

Essentially, rather than

```
job-defaults:
    label: "{name}-k8s-image-builder-python{python_version}"
```

We would

1. Use the default `job["name"]`
2. Append `python{python_version}` to `task["name"]` when yielding a python-specific task
3. Use the job transform, or later similar transform, to set the label to `{kind}-{task["name"]}`
