# MCPBoxSnapshotStatus

pending while waiting for capacity; building while the build runs; ready when MCP Boxes can serve it; failed when the build failed; canceled when a cancel request took effect.

## Example Usage

```python
from continuous.models import MCPBoxSnapshotStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: MCPBoxSnapshotStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"building"`
- `"ready"`
- `"failed"`
- `"canceled"`
