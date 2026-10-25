# CreatedSimulationStatus

Current status. starting means its runtime is booting; read the Simulation until it is running. running serves requests; after a period without requests, the first request can take longer while the Simulation resumes. stopping means its state is being saved, and stopped means it is saved; requests return 409 in both until you start it.

## Example Usage

```python
from continuous.models import CreatedSimulationStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: CreatedSimulationStatus = "starting"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"starting"`
- `"running"`
- `"stopping"`
- `"stopped"`
