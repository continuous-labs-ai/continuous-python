# Reason

Bounded failure reason for selecting recovery guidance, or null when unavailable.

## Example Usage

```python
from continuous.models import Reason

# Open enum: unrecognized values are captured as UnrecognizedStr
value: Reason = "canceled"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"canceled"`
- `"specification_invalid"`
- `"time_limit"`
- `"service_unavailable"`
- `"build_failed"`
