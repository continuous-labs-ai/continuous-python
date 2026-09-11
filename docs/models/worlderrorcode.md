# WorldErrorCode

Stable World error code.

## Example Usage

```python
from continuous.models import WorldErrorCode

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldErrorCode = "world_build_canceled"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"world_build_canceled"`
- `"world_start_failed"`
- `"world_create_failed"`
- `"world_failed"`
- `"world_population_failed"`
- `"world_population_unsupported"`
- `"world_population_invalid"`
- `"world_population_unavailable"`
