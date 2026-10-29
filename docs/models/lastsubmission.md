# LastSubmission

Outcome of the most recent submit attempt, or null.

## Example Usage

```python
from continuous.models import LastSubmission

# Open enum: unrecognized values are captured as UnrecognizedStr
value: LastSubmission = "accepted"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"accepted"`
- `"rejected"`
