# APIBoxSnapshotSpecKind

The specification the Snapshot was built from, or null until the build has read it.

## Example Usage

```python
from continuous.models import APIBoxSnapshotSpecKind

# Open enum: unrecognized values are captured as UnrecognizedStr
value: APIBoxSnapshotSpecKind = "openapi"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"openapi"`
- `"wsdl"`
- `"graphql"`
