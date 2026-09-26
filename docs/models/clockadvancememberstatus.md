# ClockAdvanceMemberStatus

Whether this member is pending, committed, failed, or skipped because it is not running.

## Example Usage

```python
from continuous.models import ClockAdvanceMemberStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: ClockAdvanceMemberStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"completed"`
- `"failed"`
- `"skipped"`
