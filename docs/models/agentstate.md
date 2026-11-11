# AgentState

What the build's agent was doing when the build last checked on it, every few minutes: working while it thinks or runs a command, idle when its turn ended without a tool call, waiting_for_limit while a subscription model waits for its usage limit to reset. Null when the build is not running or while it waits for the requester.

## Example Usage

```python
from continuous.models import AgentState

# Open enum: unrecognized values are captured as UnrecognizedStr
value: AgentState = "working"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"working"`
- `"idle"`
- `"waiting_for_limit"`
