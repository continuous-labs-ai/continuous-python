# SimulatorErrorCode

Stable Simulator build error code.

## Example Usage

```python
from continuous.models import SimulatorErrorCode

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorErrorCode = "build_failed"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"build_failed"`
- `"build_cancelled"`
