# WorldStatus

Current World lifecycle status.

## Example Usage

```python
from continuous.models import WorldStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldStatus = "building"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"building"`
- `"ready"`
- `"running"`
- `"stopped"`
- `"failed"`
- `"canceled"`
