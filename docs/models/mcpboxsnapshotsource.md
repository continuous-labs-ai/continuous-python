# MCPBoxSnapshotSource

catalog for a Snapshot Continuous publishes, build for one built or published into the workspace, box for one saved from an MCP Box.

## Example Usage

```python
from continuous.models import MCPBoxSnapshotSource

# Open enum: unrecognized values are captured as UnrecognizedStr
value: MCPBoxSnapshotSource = "catalog"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"catalog"`
- `"build"`
- `"box"`
