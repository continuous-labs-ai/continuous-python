# SimulatorErrorReason

Bounded failure reason for selecting recovery guidance, or null when unavailable. Older servers can omit this field.

## Example Usage

```python
from continuous.models import SimulatorErrorReason

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorErrorReason = "canceled"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"canceled"`
- `"specification_invalid"`
- `"time_limit"`
- `"service_unavailable"`
- `"build_failed"`
- `"population_unsupported"`
- `"world_start_failed"`
- `"world_operation_failed"`
