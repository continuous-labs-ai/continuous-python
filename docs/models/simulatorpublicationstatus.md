# SimulatorPublicationStatus

published once the Simulator's image is in the environment's own registry; failed when the last attempt failed. Creating a Simulation in the environment tries again.

## Example Usage

```python
from continuous.models import SimulatorPublicationStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorPublicationStatus = "published"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"published"`
- `"failed"`
