# BuildProgressModel

The model the builder and reviewer run on, or combined, which currently runs as claude-opus-5-5.

## Example Usage

```python
from continuous.models import BuildProgressModel

# Open enum: unrecognized values are captured as UnrecognizedStr
value: BuildProgressModel = "gpt-6-astra"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"gpt-6-astra"`
- `"gpt-6.1-sol"`
- `"claude-opus-5-5"`
- `"claude-fable-5-1"`
- `"gpt-6-sol"`
- `"combined"`
