# WorldChangeStatus

Durable change state. Poll while pending, building, or applying.

## Example Usage

```python
from continuous.models import WorldChangeStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldChangeStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"building"`
- `"applying"`
- `"completed"`
- `"failed"`
