# SpecificationWarningCode

Stable warning code. build.limitation marks behavior the Simulator does not serve like the real system. build.unrepaired marks such behavior that review did not mark a limit of the pinned contract or the scaffold, or that the build declared after review.

## Example Usage

```python
from continuous.models import SpecificationWarningCode

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SpecificationWarningCode = "spec.enum_duplicate"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"spec.enum_duplicate"`
- `"spec.path_parameter_optional"`
- `"spec.default_invalid"`
- `"spec.response_untyped"`
- `"spec.schema_limit"`
- `"spec.operation_unservable"`
- `"spec.version_ambiguous"`
- `"spec.example_null"`
- `"build.limitation"`
- `"build.unrepaired"`
