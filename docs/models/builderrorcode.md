# BuildErrorCode

Stable build error code. live_credential_rejected is a build that waited for its rejected live credentials and was not resumed within 7 days; live_rate_limited is one that waited for the live system's rate limit and was not resumed within 7 days.

## Example Usage

```python
from continuous.models import BuildErrorCode

# Open enum: unrecognized values are captured as UnrecognizedStr
value: BuildErrorCode = "build_failed"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"build_failed"`
- `"build_canceled"`
- `"specification_validation_failed"`
- `"live_credential_rejected"`
- `"live_rate_limited"`
