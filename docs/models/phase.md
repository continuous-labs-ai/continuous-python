# Phase

Agent phase: research while the research agent indexes the documents and saves the vendor pages they lack; build for generation, review for the separate reviewer, qa while QA tests the running service from outside, after the last review round of a build with credential_ids, repair while the author fixes the findings of a review or QA. Null when not recorded.

## Example Usage

```python
from continuous.models import Phase

# Open enum: unrecognized values are captured as UnrecognizedStr
value: Phase = "research"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"research"`
- `"build"`
- `"review"`
- `"qa"`
- `"repair"`
