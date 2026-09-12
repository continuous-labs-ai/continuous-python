# SimulatorBuildProgressStage

derive while the effective spec, build skeleton, and sandbox are prepared; build while the coding loop runs; assemble while an accepted artifact publishes.

## Example Usage

```python
from continuous.models import SimulatorBuildProgressStage

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorBuildProgressStage = "derive"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"derive"`
- `"build"`
- `"assemble"`
