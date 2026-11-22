# Kind

Whether the input is invalid, unsupported, incomplete, exceeds a build limit, or has no specific diagnosis.

## Example Usage

```python
from continuous.models import Kind

# Open enum: unrecognized values are captured as UnrecognizedStr
value: Kind = "invalid"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"invalid"`
- `"unsupported"`
- `"incomplete"`
- `"limit"`
- `"unknown"`
