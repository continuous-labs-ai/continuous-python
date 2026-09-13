# WorldBuildBuilder

Selected model provider. Absent for builds created before provider selection.

## Example Usage

```python
from continuous.models import WorldBuildBuilder

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldBuildBuilder = "openai"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"openai"`
- `"claude"`
