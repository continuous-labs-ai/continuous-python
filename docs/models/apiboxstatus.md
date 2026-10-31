# APIBoxStatus

starting while its runtime boots; running while it serves requests; stopping while it saves its state; stopped when it holds no runtime. A start resumes the saved state.

## Example Usage

```python
from continuous.models import APIBoxStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: APIBoxStatus = "starting"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"starting"`
- `"running"`
- `"stopping"`
- `"stopped"`
