# MCPBoxStatus

starting while its runtime boots; running while it serves MCP; stopping while it releases its runtime; stopped when it holds no runtime.

## Example Usage

```python
from continuous.models import MCPBoxStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: MCPBoxStatus = "starting"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"starting"`
- `"running"`
- `"stopping"`
- `"stopped"`
