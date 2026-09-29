# SpecificationWarningCode

Stable warning code. build.limitation marks behavior the Simulator does not serve like the real system.

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
- `"build.limitation"`
