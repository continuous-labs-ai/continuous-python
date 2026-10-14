# BuildSimulatorRequestModel

Model that builds and reviews the Simulator. Defaults to combined. Its provider is derived from the model. combined currently builds, reviews, and runs QA exactly as claude-opus-5-5 does.

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
