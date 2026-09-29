# SimulationDetailStatus

Current status. running serves requests. paused means the Simulation was idle and the platform paused it; the next request wakes it. stopped means its state is saved and requests return 409 until you start it.

## Example Usage

```python
from continuous.models import SimulationDetailStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulationDetailStatus = "running"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"running"`
- `"paused"`
- `"stopped"`
