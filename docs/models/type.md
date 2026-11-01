# Type

live_credentials while QA waits for the credentials it names to be refreshed; time_limit while the build waits for a resume that adds time with add_seconds.

## Example Usage

```python
from continuous.models import Type

# Open enum: unrecognized values are captured as UnrecognizedStr
value: Type = "live_credentials"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"live_credentials"`
- `"time_limit"`
