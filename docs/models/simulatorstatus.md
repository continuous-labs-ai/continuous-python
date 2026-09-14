# SimulatorStatus

pending while waiting for capacity; building while the build runs; ready when Simulations, Worlds, and incremental builds can use it; failed when the build failed; canceled when a cancel request took effect.

## Example Usage

```python
from continuous.models import SimulatorStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"building"`
- `"ready"`
- `"failed"`
- `"canceled"`
