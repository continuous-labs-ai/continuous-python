# ClockAdvanceStatus

Durable operation state. Poll while pending or running.

## Example Usage

```python
from continuous.models import ClockAdvanceStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: ClockAdvanceStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"running"`
- `"completed"`
- `"failed"`
