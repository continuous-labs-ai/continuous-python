# SimulatorBuildProgressPhase

Agent phase: build for generation, review for the separate reviewer, finalize for author repair after review. Null when not recorded.

## Example Usage

```python
from continuous.models import SimulatorBuildProgressPhase

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorBuildProgressPhase = "build"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"build"`
- `"review"`
- `"finalize"`
