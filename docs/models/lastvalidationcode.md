# LastValidationCode

Fixed code for the latest validation finding. Null when no finding is available. Authored details stay private.

## Example Usage

```python
from continuous.models import LastValidationCode

# Open enum: unrecognized values are captured as UnrecognizedStr
value: LastValidationCode = "invalid_plan"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"invalid_plan"`
- `"unsupported_claim"`
- `"invalid_requirement"`
- `"nested_proof"`
- `"request_not_satisfied"`
- `"verification_unavailable"`
