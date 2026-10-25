# SimulationSnapshotStatus

creating while the state is saved. A Simulation can start from a ready Snapshot. A failed Snapshot holds no state.

## Example Usage

```python
from continuous.models import SimulationSnapshotStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulationSnapshotStatus = "creating"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"creating"`
- `"ready"`
- `"failed"`
