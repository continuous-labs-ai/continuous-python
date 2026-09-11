# WorldStatus

building while the build runs; ready when it can be started; running or stopped once its Simulations exist; failed when building or first start fails; canceled when the build was canceled.

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
