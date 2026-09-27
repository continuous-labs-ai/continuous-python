# WorldErrorReason

Bounded failure reason for selecting recovery guidance, or null when unavailable.

## Example Usage

```python
from continuous.models import WorldErrorReason

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldErrorReason = "canceled"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"canceled"`
- `"time_limit"`
- `"service_unavailable"`
- `"build_failed"`
- `"population_unsupported"`
- `"world_start_failed"`
- `"world_operation_failed"`
