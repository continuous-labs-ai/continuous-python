# SimulatorErrorCode

Stable Simulator build error code. live_credential_rejected is a build that waited at QA for its rejected live credentials and was not resumed within 7 days.

## Example Usage

```python
from continuous.models import SimulatorErrorCode

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorErrorCode = "build_failed"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"build_failed"`
- `"build_canceled"`
- `"specification_validation_failed"`
- `"live_credential_rejected"`
