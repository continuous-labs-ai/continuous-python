# APIBoxSnapshotSource

catalog for a Snapshot Continuous publishes, build for one built or cloned into the workspace, box for one saved from an API Box.

## Example Usage

```python
from continuous.models import APIBoxSnapshotSource

# Open enum: unrecognized values are captured as UnrecognizedStr
value: APIBoxSnapshotSource = "catalog"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"catalog"`
- `"build"`
- `"box"`
