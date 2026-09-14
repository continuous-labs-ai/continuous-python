# WorldBuildProgressBuilder

Selected model provider, or null for a build created before provider selection.

## Example Usage

```python
from continuous.models import WorldBuildProgressBuilder

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldBuildProgressBuilder = "openai"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"openai"`
- `"claude"`
