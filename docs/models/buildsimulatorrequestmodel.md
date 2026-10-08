# BuildSimulatorRequestModel

Model that builds and reviews the Simulator. Defaults to claude-opus-5-5. Its provider is derived from the model. combined: builder on claude-opus-5-5, with two parallel reviews on claude-opus-5-5 and gpt-6-astra.

## Example Usage

```python
from continuous.models import BuildSimulatorRequestModel
value: BuildSimulatorRequestModel = "gpt-6-astra"
```


## Values

- `"gpt-6-astra"`
- `"gpt-6.1-sol"`
- `"claude-opus-5-5"`
- `"claude-fable-5-1"`
- `"combined"`
