# CreatedSimulationStatus

Current Simulation status.

## Example Usage

```python
from continuous.models import CreatedSimulationStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: CreatedSimulationStatus = "running"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"running"`
- `"paused"`
- `"stopped"`
