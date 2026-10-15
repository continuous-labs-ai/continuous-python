# SimulatorResearchStatus

pending until the stage starts; running while the research agent indexes the documents and saves the vendor pages they lack; complete when it wrote an index that names every document; failed when it could not finish, which fails the build, or when the build ended first; provided when the documents included INDEX.md, so research did not run.

## Example Usage

```python
from continuous.models import SimulatorResearchStatus

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorResearchStatus = "pending"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"pending"`
- `"running"`
- `"complete"`
- `"failed"`
- `"provided"`
