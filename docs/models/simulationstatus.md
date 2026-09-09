# SimulationStatus

Current Simulation status.

## Example Usage

```python
from continuous.models import SimulationStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulationStatus = "running"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"running"`
- `"paused"`
- `"stopped"`
