# BuildWorldRequestBuilder

Legacy provider selection. Alone it selects the provider's default model (openai is gpt-6-astra, claude is claude-fable-5-1); with model it must name the model's provider.

## Example Usage

```python
from continuous.models import BuildWorldRequestBuilder
value: BuildWorldRequestBuilder = "openai"
```


## Values

- `"openai"`
- `"claude"`
