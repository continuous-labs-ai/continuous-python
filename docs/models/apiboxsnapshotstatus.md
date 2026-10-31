# APIBoxSnapshotStatus

pending while waiting for capacity; building while the build runs; ready when API Boxes can start from it; failed when the build failed; canceled when a cancel request took effect.

## Example Usage

```python
from continuous.models import APIBoxSnapshotStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: APIBoxSnapshotStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"building"`
- `"ready"`
- `"failed"`
- `"canceled"`
