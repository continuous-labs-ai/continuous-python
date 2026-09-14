# SimulatorSpecKind

The specification the Simulator was built from, or null until a build has read it.

## Example Usage

```python
from continuous.models import SimulatorSpecKind

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorSpecKind = "openapi"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"openapi"`
- `"wsdl"`
