# WorldBuildProgressStage

Current phase of starting-data preparation.

## Example Usage

```python
from continuous.models import WorldBuildProgressStage

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldBuildProgressStage = "planning"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"planning"`
- `"generating"`
- `"validating"`
- `"complete"`
