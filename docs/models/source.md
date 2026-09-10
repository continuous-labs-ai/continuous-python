# Source

workspace for a Simulator your workspace built; catalog for a read-only Simulator that Continuous publishes.

## Example Usage

```python
from continuous.models import Source

# Open enum: unrecognized values are captured as UnrecognizedStr
value: Source = "workspace"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"workspace"`
- `"catalog"`
