# SimulatorSource

workspace for a Simulator your workspace built; catalog for a read-only Simulator that Continuous publishes.

## Example Usage

```python
from continuous.models import SimulatorSource

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorSource = "workspace"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"workspace"`
- `"catalog"`
