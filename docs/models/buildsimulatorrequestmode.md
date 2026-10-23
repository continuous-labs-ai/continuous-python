# BuildSimulatorRequestMode

What the Simulator simulates. api, the default, builds the vendor's API from a spec, or from a parent with instructions. web_app builds the vendor's logged-in web app and the private API behind it: it takes no spec, spec_kind or filter, needs exactly one Web App login credential in credential_ids, runs on a Claude model or combined, and starts with a Discovery stage that logs in and captures the app. With parent_id and instructions, it builds from a ready web_app Simulator and starts with that Simulator's captures instead of Discovery.

## Example Usage

```python
from continuous.models import BuildSimulatorRequestMode
value: BuildSimulatorRequestMode = "api"
```


## Values

- `"api"`
- `"web_app"`
