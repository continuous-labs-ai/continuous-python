# WorldBuildProgressStage

planning while the build prepares; generating while the builder writes starting data; validating while the data is checked through the Simulators' APIs; reviewing while the separate reviewer checks it; complete when verified starting data is saved.

## Example Usage

```python
from continuous.models import WorldBuildProgressStage

# Open enum: unrecognized values are captured as UnrecognizedStr
value: WorldBuildProgressStage = "planning"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"planning"`
- `"generating"`
- `"validating"`
- `"reviewing"`
- `"complete"`
