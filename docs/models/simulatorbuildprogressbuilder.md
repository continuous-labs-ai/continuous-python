# SimulatorBuildProgressBuilder

The coding-loop provider.

## Example Usage

```python
from continuous.models import SimulatorBuildProgressBuilder

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorBuildProgressBuilder = "claude"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"claude"`
- `"openai"`
