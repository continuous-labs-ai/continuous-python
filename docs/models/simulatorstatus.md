# SimulatorStatus

Current build status.

## Example Usage

```python
from continuous.models import SimulatorStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorStatus = "building"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"building"`
- `"ready"`
- `"failed"`
- `"canceled"`
