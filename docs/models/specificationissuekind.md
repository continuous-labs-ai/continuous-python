# SpecificationIssueKind

Whether the input is invalid, unsupported, incomplete, or has no specific diagnosis.

## Example Usage

```python
from continuous.models import SpecificationIssueKind

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SpecificationIssueKind = "invalid"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"invalid"`
- `"unsupported"`
- `"incomplete"`
- `"unknown"`
