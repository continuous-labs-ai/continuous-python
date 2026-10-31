# WebBoxSnapshotSource

catalog for a Snapshot Continuous publishes, build for one built into the workspace.

## Example Usage

```python
from continuous.models import WebBoxSnapshotSource

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WebBoxSnapshotSource = "catalog"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"catalog"`
- `"build"`
