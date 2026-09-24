# WorldBuildProgressModel

The model the builder and reviewer run on, or null for a build created before provider selection. A build recorded before model selection reports its provider's default.

## Example Usage

```python
from continuous.models import WorldBuildProgressModel

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldBuildProgressModel = "gpt-6-astra"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"gpt-6-astra"`
- `"gpt-6-sol"`
- `"claude-opus-5-5"`
- `"claude-fable-5-1"`
