# Type

live_credentials while the build waits for the credentials it names to be refreshed; live_rate_limit while it waits for the live system's rate limit to reset; unavailable while it waits for a service it could not reach, such as its live system, its model or sandbox provider, or its own test runner; time_limit while it waits for a resume that adds time with add_seconds; paused while it waits for a resume after its resource's pause operation.

## Example Usage

```python
from continuous.models import Type

# Open enum: unrecognized values are captured as UnrecognizedStr
value: Type = "live_credentials"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"live_credentials"`
- `"live_rate_limit"`
- `"unavailable"`
- `"time_limit"`
- `"paused"`
