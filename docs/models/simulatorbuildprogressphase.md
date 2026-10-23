# SimulatorBuildProgressPhase

Agent phase: discovery while the discovery agent logs in to the vendor's web app and captures it, in a Web App build; research while the research agent indexes the documents and saves the vendor pages they lack; build for generation, review for the separate reviewer, qa while QA tests the running Simulator from outside, after the last review round of a build with credential_ids, finalize for author repair after review or QA. Null when not recorded.

## Example Usage

```python
from continuous.models import SimulatorBuildProgressPhase

# Open enum: unrecognized values are captured as UnrecognizedStr
value: SimulatorBuildProgressPhase = "discovery"
```


## Values

This is an open enum. Unrecognized values will not fail type checks.

- `"discovery"`
- `"research"`
- `"build"`
- `"review"`
- `"qa"`
- `"finalize"`
