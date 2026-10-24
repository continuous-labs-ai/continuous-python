# SimulationDetailStatus

Current status. running serves requests; after a period without requests, the first request can take longer while the Simulation resumes. stopped means its state is saved and requests return 409 until you start it.

## Example Usage

```python
from continuous.models import SimulationDetailStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulationDetailStatus = "running"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"running"`
- `"stopped"`
