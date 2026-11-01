# BoxErrorCode

Stable Box error code: <box>_start_failed when its runtime did not start, <box>_runtime_lost when its runtime was lost, mcpbox_apibox_gone when an MCP Box's API Box was deleted, and mcpbox_apibox_unissued when that API Box has no stored token.

## Example Usage

```python
from continuous.models import BoxErrorCode

# Open enum: unrecognized values are captured as UnrecognizedStr
value: BoxErrorCode = "apibox_start_failed"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"apibox_start_failed"`
- `"apibox_runtime_lost"`
- `"webbox_start_failed"`
- `"webbox_runtime_lost"`
- `"mcpbox_start_failed"`
- `"mcpbox_runtime_lost"`
- `"mcpbox_apibox_gone"`
- `"mcpbox_apibox_unissued"`
