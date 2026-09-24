# SimulatorBuildProgressModel

The model the builder and reviewer run on. A build recorded before model selection reports its provider's default.

## Example Usage

```python
from continuous.models import SimulatorBuildProgressModel

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorBuildProgressModel = "gpt-6-astra"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"gpt-6-astra"`
- `"gpt-6-sol"`
- `"claude-opus-5-5"`
- `"claude-fable-5-1"`
