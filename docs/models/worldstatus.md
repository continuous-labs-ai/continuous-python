# WorldStatus

pending while waiting for capacity; building while the build runs; ready when it can be started; starting while its Simulations start; running or stopped once its Simulations exist; stopping while they stop; failed when building or first start fails; canceled when the build was canceled.

## Example Usage

```python
from continuous.models import WorldStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"building"`
- `"ready"`
- `"starting"`
- `"running"`
- `"stopping"`
- `"stopped"`
- `"failed"`
- `"canceled"`
