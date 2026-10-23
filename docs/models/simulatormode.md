# SimulatorMode

api for a Simulator of the vendor's API; web_app for one of its logged-in web app.

## Example Usage

```python
from continuous.models import SimulatorMode

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorMode = "api"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"api"`
- `"web_app"`
