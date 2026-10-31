# continuous-labs

Python SDK for the Continuous Simulation API.

Speakeasy generates this SDK from the public Continuous Simulation API contract. The [API reference](https://docs.continuouslabs.ai/api-reference/overview) documents every operation. Report SDK and API problems in this repository.

<!-- Start Summary [summary] -->
## Summary

Continuous API: Start API Boxes, Web Boxes and MCP Boxes from Snapshots. Authenticate every request with an API key sent as a Bearer token.
<!-- End Summary [summary] -->

<!-- Start Table of Contents [toc] -->
## Table of Contents
<!-- $toc-max-depth=2 -->
* [continuous-labs](https://github.com/continuous-labs-ai/continuous-python/blob/master/#continuous-labs)
  * [SDK Installation](https://github.com/continuous-labs-ai/continuous-python/blob/master/#sdk-installation)
  * [IDE Support](https://github.com/continuous-labs-ai/continuous-python/blob/master/#ide-support)
  * [SDK Example Usage](https://github.com/continuous-labs-ai/continuous-python/blob/master/#sdk-example-usage)
  * [Authentication](https://github.com/continuous-labs-ai/continuous-python/blob/master/#authentication)
  * [Available Resources and Operations](https://github.com/continuous-labs-ai/continuous-python/blob/master/#available-resources-and-operations)
  * [Pagination](https://github.com/continuous-labs-ai/continuous-python/blob/master/#pagination)
  * [File uploads](https://github.com/continuous-labs-ai/continuous-python/blob/master/#file-uploads)
  * [Retries](https://github.com/continuous-labs-ai/continuous-python/blob/master/#retries)
  * [Error Handling](https://github.com/continuous-labs-ai/continuous-python/blob/master/#error-handling)
  * [Server Selection](https://github.com/continuous-labs-ai/continuous-python/blob/master/#server-selection)
  * [Custom HTTP Client](https://github.com/continuous-labs-ai/continuous-python/blob/master/#custom-http-client)
  * [Resource Management](https://github.com/continuous-labs-ai/continuous-python/blob/master/#resource-management)
  * [Debugging](https://github.com/continuous-labs-ai/continuous-python/blob/master/#debugging)
* [Development](https://github.com/continuous-labs-ai/continuous-python/blob/master/#development)
  * [Maturity](https://github.com/continuous-labs-ai/continuous-python/blob/master/#maturity)
  * [Contributions](https://github.com/continuous-labs-ai/continuous-python/blob/master/#contributions)

<!-- End Table of Contents [toc] -->

<!-- Start SDK Installation [installation] -->
## SDK Installation

> [!NOTE]
> **Python version upgrade policy**
>
> Once a Python version reaches its [official end of life date](https://devguide.python.org/versions/), a 3-month grace period is provided for users to upgrade. Following this grace period, the minimum python version supported in the SDK will be updated.

The SDK can be installed with *uv*, *pip*, or *poetry* package managers.

### uv

*uv* is a fast Python package installer and resolver, designed as a drop-in replacement for pip and pip-tools. It's recommended for its speed and modern Python tooling capabilities.

```bash
uv add continuous-labs
```

### PIP

*PIP* is the default package installer for Python, enabling easy installation and management of packages from PyPI via the command line.

```bash
pip install continuous-labs
```

### Poetry

*Poetry* is a modern tool that simplifies dependency management and package publishing by using a single `pyproject.toml` file to handle project metadata and dependencies.

```bash
poetry add continuous-labs
```

### Shell and script usage with `uv`

You can use this SDK in a Python shell with [uv](https://docs.astral.sh/uv/) and the `uvx` command that comes with it like so:

```shell
uvx --from continuous-labs python
```

It's also possible to write a standalone Python script without needing to set up a whole project like so:

```python
#!/usr/bin/env -S uv run --script
# /// script
# requires-python = ">=3.10"
# dependencies = [
#     "continuous-labs",
# ]
# ///

from continuous import Continuous

sdk = Continuous(
  # SDK arguments
)

# Rest of script here...
```

Once that is saved to a file, you can run it with `uv run script.py` where
`script.py` can be replaced with the actual file name.
<!-- End SDK Installation [installation] -->

<!-- Start IDE Support [idesupport] -->
## IDE Support

### PyCharm

Generally, the SDK will work well with most IDEs out of the box. However, when using PyCharm, you can enjoy much better integration with Pydantic by installing an additional plugin.

- [PyCharm Pydantic Plugin](https://docs.pydantic.dev/latest/integrations/pycharm/)
<!-- End IDE Support [idesupport] -->

<!-- Start SDK Example Usage [usage] -->
## SDK Example Usage

### Example

```python
# Synchronous Example
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.list_apiboxes(limit=50)

    while res is not None:
        # Handle items

        res = res.next()
```

</br>

The same SDK client can also be used to make asynchronous requests by importing asyncio.

```python
# Asynchronous Example
import asyncio
from continuous import Continuous
import os

async def main():

    async with Continuous(
        api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
    ) as c_client:

        res = await c_client.apibox.list_apiboxes_async(limit=50)

        while res is not None:
            # Handle items

            res = res.next()

asyncio.run(main())
```
<!-- End SDK Example Usage [usage] -->

<!-- Start Authentication [security] -->
## Authentication

### Per-Client Security Schemes

This SDK supports the following security scheme globally:

| Name           | Type | Scheme      | Environment Variable      |
| -------------- | ---- | ----------- | ------------------------- |
| `api_key_auth` | http | HTTP Bearer | `CONTINUOUS_API_KEY_AUTH` |

To authenticate with the API the `api_key_auth` parameter must be set when initializing the SDK client instance. For example:
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.list_apiboxes(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```
<!-- End Authentication [security] -->

<!-- Start Available Resources and Operations [operations] -->
## Available Resources and Operations

<details open>
<summary>Available methods</summary>

### [Apibox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md)

* [list_apiboxes](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#list_apiboxes) - List API Boxes
* [create_apibox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#create_apibox) - Create API Box
* [delete_apibox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#delete_apibox) - Delete API Box
* [get_apibox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#get_apibox) - Get API Box
* [update_apibox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#update_apibox) - Update API Box
* [get_apibox_connection](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#get_apibox_connection) - Get API Box connection
* [start_apibox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#start_apibox) - Start API Box
* [list_apibox_steps](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#list_apibox_steps) - List API Box Steps
* [stop_apibox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsdk/README.md#stop_apibox) - Stop API Box

### [ApiboxSnapshots](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsnapshots/README.md)

* [list_apibox_snapshots](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsnapshots/README.md#list_apibox_snapshots) - List API Box Snapshots
* [create_apibox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsnapshots/README.md#create_apibox_snapshot) - Create API Box Snapshot
* [delete_apibox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsnapshots/README.md#delete_apibox_snapshot) - Delete API Box Snapshot
* [get_apibox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsnapshots/README.md#get_apibox_snapshot) - Get API Box Snapshot
* [update_apibox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsnapshots/README.md#update_apibox_snapshot) - Update API Box Snapshot
* [cancel_apibox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsnapshots/README.md#cancel_apibox_snapshot) - Cancel Snapshot build
* [resume_apibox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/apiboxsnapshots/README.md#resume_apibox_snapshot) - Resume Snapshot build

### [Credentials](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/credentials/README.md)

* [list_credentials](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/credentials/README.md#list_credentials) - List credentials
* [create_credential](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/credentials/README.md#create_credential) - Create credential
* [delete_credential](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/credentials/README.md#delete_credential) - Delete credential
* [get_credential](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/credentials/README.md#get_credential) - Get credential
* [update_credential](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/credentials/README.md#update_credential) - Update credential

### [Mcpbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md)

* [list_mcpboxes](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md#list_mcpboxes) - List MCP Boxes
* [create_mcpbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md#create_mcpbox) - Create MCP Box
* [delete_mcpbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md#delete_mcpbox) - Delete MCP Box
* [get_mcpbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md#get_mcpbox) - Get MCP Box
* [update_mcpbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md#update_mcpbox) - Update MCP Box
* [get_mcpbox_connection](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md#get_mcpbox_connection) - Get MCP Box connection
* [start_mcpbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md#start_mcpbox) - Start MCP Box
* [stop_mcpbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsdk/README.md#stop_mcpbox) - Stop MCP Box

### [McpboxSnapshots](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsnapshots/README.md)

* [list_mcpbox_snapshots](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsnapshots/README.md#list_mcpbox_snapshots) - List MCP Box Snapshots
* [create_mcpbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsnapshots/README.md#create_mcpbox_snapshot) - Create MCP Box Snapshot
* [delete_mcpbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsnapshots/README.md#delete_mcpbox_snapshot) - Delete MCP Box Snapshot
* [get_mcpbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsnapshots/README.md#get_mcpbox_snapshot) - Get MCP Box Snapshot
* [update_mcpbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsnapshots/README.md#update_mcpbox_snapshot) - Update MCP Box Snapshot
* [cancel_mcpbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsnapshots/README.md#cancel_mcpbox_snapshot) - Cancel Snapshot build
* [resume_mcpbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/mcpboxsnapshots/README.md#resume_mcpbox_snapshot) - Resume Snapshot build

### [~~SimulationSnapshots~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulationsnapshots/README.md)

* [~~list_simulation_snapshots~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulationsnapshots/README.md#list_simulation_snapshots) - List Simulation Snapshots :warning: **Deprecated**
* [~~create_simulation_snapshot~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulationsnapshots/README.md#create_simulation_snapshot) - Create Simulation Snapshot :warning: **Deprecated**
* [~~delete_simulation_snapshot~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulationsnapshots/README.md#delete_simulation_snapshot) - Delete Simulation Snapshot :warning: **Deprecated**
* [~~get_simulation_snapshot~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulationsnapshots/README.md#get_simulation_snapshot) - Get Simulation Snapshot :warning: **Deprecated**
* [~~update_simulation_snapshot~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulationsnapshots/README.md#update_simulation_snapshot) - Update Simulation Snapshot :warning: **Deprecated**

### [~~Simulations~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md)

* [~~list_simulations~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#list_simulations) - List Simulations :warning: **Deprecated**
* [~~create_simulation~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#create_simulation) - Create Simulation :warning: **Deprecated**
* [~~delete_simulation~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#delete_simulation) - Delete Simulation :warning: **Deprecated**
* [~~get_simulation~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#get_simulation) - Get Simulation :warning: **Deprecated**
* [~~update_simulation~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#update_simulation) - Update Simulation :warning: **Deprecated**
* [~~create_simulation_advance~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#create_simulation_advance) - Advance Simulation Time :warning: **Deprecated**
* [~~get_simulation_advance~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#get_simulation_advance) - Get Simulation Clock Advance :warning: **Deprecated**
* [~~list_simulation_advance_events~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#list_simulation_advance_events) - List Clock Advance Events :warning: **Deprecated**
* [~~start_simulation~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#start_simulation) - Start Simulation :warning: **Deprecated**
* [~~list_simulation_steps~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#list_simulation_steps) - List Simulation Steps :warning: **Deprecated**
* [~~stop_simulation~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#stop_simulation) - Stop Simulation :warning: **Deprecated**
* [~~get_simulation_token~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#get_simulation_token) - Get Current Simulation Token :warning: **Deprecated**
* [~~regenerate_simulation_token~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulations/README.md#regenerate_simulation_token) - Regenerate Simulation Token :warning: **Deprecated**

### [~~Simulators~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md)

* [~~list_simulators~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md#list_simulators) - List Simulators :warning: **Deprecated**
* [~~build_simulator~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md#build_simulator) - Build Simulator :warning: **Deprecated**
* [~~delete_simulator~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md#delete_simulator) - Delete Simulator :warning: **Deprecated**
* [~~get_simulator~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md#get_simulator) - Get Simulator :warning: **Deprecated**
* [~~update_simulator~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md#update_simulator) - Update Simulator :warning: **Deprecated**
* [~~cancel_simulator~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md#cancel_simulator) - Cancel Simulator Build :warning: **Deprecated**
* [~~clone_simulator~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md#clone_simulator) - Clone Simulator :warning: **Deprecated**
* [~~resume_simulator~~](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/simulators/README.md#resume_simulator) - Resume Simulator Build :warning: **Deprecated**

### [Webbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md)

* [list_webboxes](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#list_webboxes) - List Web Boxes
* [create_webbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#create_webbox) - Create Web Box
* [delete_webbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#delete_webbox) - Delete Web Box
* [get_webbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#get_webbox) - Get Web Box
* [update_webbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#update_webbox) - Update Web Box
* [get_webbox_connection](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#get_webbox_connection) - Get Web Box connection
* [start_webbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#start_webbox) - Start Web Box
* [list_webbox_steps](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#list_webbox_steps) - List Web Box Steps
* [stop_webbox](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsdk/README.md#stop_webbox) - Stop Web Box

### [WebboxSnapshots](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsnapshots/README.md)

* [list_webbox_snapshots](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsnapshots/README.md#list_webbox_snapshots) - List Web Box Snapshots
* [create_webbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsnapshots/README.md#create_webbox_snapshot) - Create Web Box Snapshot
* [delete_webbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsnapshots/README.md#delete_webbox_snapshot) - Delete Web Box Snapshot
* [get_webbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsnapshots/README.md#get_webbox_snapshot) - Get Web Box Snapshot
* [update_webbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsnapshots/README.md#update_webbox_snapshot) - Update Web Box Snapshot
* [cancel_webbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsnapshots/README.md#cancel_webbox_snapshot) - Cancel Snapshot build
* [resume_webbox_snapshot](https://github.com/continuous-labs-ai/continuous-python/blob/master/docs/sdks/webboxsnapshots/README.md#resume_webbox_snapshot) - Resume Snapshot build

</details>
<!-- End Available Resources and Operations [operations] -->

<!-- Start Pagination [pagination] -->
## Pagination

Some of the endpoints in this SDK support pagination. To use pagination, you make your SDK calls as usual, but the
returned response object will have a `Next` method that can be called to pull down the next group of results. If the
return value of `Next` is `None`, then there are no more pages to be fetched.

Here's an example of one such pagination call:
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.list_apiboxes(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```
<!-- End Pagination [pagination] -->

<!-- Start File uploads [file-upload] -->
## File uploads

Certain SDK methods accept file objects as part of a request body or multi-part request. It is possible and typically recommended to upload files as a stream rather than reading the entire contents into memory. This avoids excessive memory consumption and potentially crashing with out-of-memory errors when working with very large files. The following example demonstrates how to attach a file stream to a request.

> [!TIP]
>
> For endpoints that handle file uploads bytes arrays can also be used. However, using streams is recommended for large files.
>

```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox_snapshots.create_apibox_snapshot(request={})

    # Handle response
    print(res)

```
<!-- End File uploads [file-upload] -->

<!-- Start Retries [retries] -->
## Retries

Some of the endpoints in this SDK support retries. If you use the SDK without any configuration, it will fall back to the default retry strategy provided by the API. However, the default retry strategy can be overridden on a per-operation basis, or across the entire SDK.

To change the default retry strategy for a single API call, simply provide a `RetryConfig` object to the call:
```python
from continuous import Continuous
from continuous.utils import BackoffStrategy, RetryConfig
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.list_apiboxes(limit=50,
        RetryConfig("backoff", BackoffStrategy(1, 50, 1.1, 100), False))

    while res is not None:
        # Handle items

        res = res.next()

```

If you'd like to override the default retry strategy for all operations that support retries, you can use the `retry_config` optional parameter when initializing the SDK:
```python
from continuous import Continuous
from continuous.utils import BackoffStrategy, RetryConfig
import os


with Continuous(
    retry_config=RetryConfig("backoff", BackoffStrategy(1, 50, 1.1, 100), False),
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.list_apiboxes(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```
<!-- End Retries [retries] -->

<!-- Start Error Handling [errors] -->
## Error Handling

[`ContinuousError`](https://github.com/continuous-labs-ai/continuous-python/blob/master/./src/continuous/errors/continuouserror.py) is the base class for all HTTP error responses. It has the following properties:

| Property           | Type             | Description                                                                             |
| ------------------ | ---------------- | --------------------------------------------------------------------------------------- |
| `err.message`      | `str`            | Error message                                                                           |
| `err.status_code`  | `int`            | HTTP response status code eg `404`                                                      |
| `err.headers`      | `httpx.Headers`  | HTTP response headers                                                                   |
| `err.body`         | `str`            | HTTP body. Can be empty string if no body is returned.                                  |
| `err.raw_response` | `httpx.Response` | Raw HTTP response                                                                       |
| `err.data`         |                  | Optional. Some errors may contain structured data. [See Error Classes](https://github.com/continuous-labs-ai/continuous-python/blob/master/#error-classes). |

### Example
```python
from continuous import Continuous, errors
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:
    res = None
    try:

        res = c_client.apibox.list_apiboxes(limit=50)

        while res is not None:
            # Handle items

            res = res.next()


    except errors.ContinuousError as e:
        # The base class for HTTP error responses
        print(e.message)
        print(e.status_code)
        print(e.body)
        print(e.headers)
        print(e.raw_response)

        # Depending on the method different errors may be thrown
        if isinstance(e, errors.Error):
            print(e.data.code)  # str
            print(e.data.detail)  # str
            print(e.data.validation)  # Nullable[models.SpecificationValidation]
```

### Error Classes
**Primary errors:**
* [`ContinuousError`](https://github.com/continuous-labs-ai/continuous-python/blob/master/./src/continuous/errors/continuouserror.py): The base class for HTTP error responses.
  * [`Error`](https://github.com/continuous-labs-ai/continuous-python/blob/master/./src/continuous/errors/error.py): Generic error.

<details><summary>Less common errors (6)</summary>

<br />

**Network errors:**
* [`httpx.RequestError`](https://www.python-httpx.org/exceptions/#httpx.RequestError): Base class for request errors.
    * [`httpx.ConnectError`](https://www.python-httpx.org/exceptions/#httpx.ConnectError): HTTP client was unable to make a request to a server.
    * [`httpx.TimeoutException`](https://www.python-httpx.org/exceptions/#httpx.TimeoutException): HTTP request timed out.


**Inherit from [`ContinuousError`](https://github.com/continuous-labs-ai/continuous-python/blob/master/./src/continuous/errors/continuouserror.py)**:
* [`SimulatorDeleteConflictError`](https://github.com/continuous-labs-ai/continuous-python/blob/master/./src/continuous/errors/simulatordeleteconflicterror.py): Conflict. Status code `409`. Applicable to 1 of 78 methods.*
* [`ResponseValidationError`](https://github.com/continuous-labs-ai/continuous-python/blob/master/./src/continuous/errors/responsevalidationerror.py): Type mismatch between the response data and the expected Pydantic model. Provides access to the Pydantic validation error via the `cause` attribute.

</details>

\* Check [the method documentation](https://github.com/continuous-labs-ai/continuous-python/blob/master/#available-resources-and-operations) to see if the error is applicable.
<!-- End Error Handling [errors] -->

<!-- Start Server Selection [server] -->
## Server Selection

### Override Server URL Per-Client

The default server can be overridden globally by passing a URL to the `server_url: str` optional parameter when initializing the SDK client instance. For example:
```python
from continuous import Continuous
import os


with Continuous(
    server_url="https://api.continuouslabs.ai",
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.apibox.list_apiboxes(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```
<!-- End Server Selection [server] -->

<!-- Start Custom HTTP Client [http-client] -->
## Custom HTTP Client

The Python SDK makes API calls using the [httpx](https://www.python-httpx.org/) HTTP library.  In order to provide a convenient way to configure timeouts, cookies, proxies, custom headers, and other low-level configuration, you can initialize the SDK client with your own HTTP client instance.
Depending on whether you are using the sync or async version of the SDK, you can pass an instance of `HttpClient` or `AsyncHttpClient` respectively, which are Protocol's ensuring that the client has the necessary methods to make API calls.
This allows you to wrap the client with your own custom logic, such as adding custom headers, logging, or error handling, or you can just pass an instance of `httpx.Client` or `httpx.AsyncClient` directly.

For example, you could specify a header for every request that this sdk makes as follows:
```python
from continuous import Continuous
import httpx

http_client = httpx.Client(headers={"x-custom-header": "someValue"})
s = Continuous(client=http_client)
```

or you could wrap the client with your own custom logic:
```python
from continuous import Continuous
from continuous.httpclient import AsyncHttpClient
import httpx

class CustomClient(AsyncHttpClient):
    client: AsyncHttpClient

    def __init__(self, client: AsyncHttpClient):
        self.client = client

    async def send(
        self,
        request: httpx.Request,
        *,
        stream: bool = False,
        auth: Union[
            httpx._types.AuthTypes, httpx._client.UseClientDefault, None
        ] = httpx.USE_CLIENT_DEFAULT,
        follow_redirects: Union[
            bool, httpx._client.UseClientDefault
        ] = httpx.USE_CLIENT_DEFAULT,
    ) -> httpx.Response:
        request.headers["Client-Level-Header"] = "added by client"

        return await self.client.send(
            request, stream=stream, auth=auth, follow_redirects=follow_redirects
        )

    def build_request(
        self,
        method: str,
        url: httpx._types.URLTypes,
        *,
        content: Optional[httpx._types.RequestContent] = None,
        data: Optional[httpx._types.RequestData] = None,
        files: Optional[httpx._types.RequestFiles] = None,
        json: Optional[Any] = None,
        params: Optional[httpx._types.QueryParamTypes] = None,
        headers: Optional[httpx._types.HeaderTypes] = None,
        cookies: Optional[httpx._types.CookieTypes] = None,
        timeout: Union[
            httpx._types.TimeoutTypes, httpx._client.UseClientDefault
        ] = httpx.USE_CLIENT_DEFAULT,
        extensions: Optional[httpx._types.RequestExtensions] = None,
    ) -> httpx.Request:
        return self.client.build_request(
            method,
            url,
            content=content,
            data=data,
            files=files,
            json=json,
            params=params,
            headers=headers,
            cookies=cookies,
            timeout=timeout,
            extensions=extensions,
        )

s = Continuous(async_client=CustomClient(httpx.AsyncClient()))
```
### httpx2 (Pydantic's httpx fork)

[httpx2](https://httpx2.pydantic.dev/) is Pydantic's maintained fork of `httpx`. To run this SDK on httpx2, call `alias_httpx()` at your program's entry point, before importing the SDK, so every `import httpx` — including the ones inside the SDK — resolves to `httpx2`:
```python
import httpx2

httpx2.alias_httpx()

from continuous import Continuous

s = Continuous()
```

An SDK can also be generated against httpx2 directly, so it depends on the fork instead of `httpx`, by setting `python.httpClientLibrary: httpx2` in `gen.yaml`.
<!-- End Custom HTTP Client [http-client] -->

<!-- Start Resource Management [resource-management] -->
## Resource Management

The `Continuous` class implements the context manager protocol and registers a finalizer function to close the underlying sync and async HTTPX clients it uses under the hood. This will close HTTP connections, release memory and free up other resources held by the SDK. In short-lived Python programs and notebooks that make a few SDK method calls, resource management may not be a concern. However, in longer-lived programs, it is beneficial to create a single SDK instance via a [context manager][context-manager] and reuse it across the application.

[context-manager]: https://docs.python.org/3/reference/datamodel.html#context-managers

```python
from continuous import Continuous
import os
def main():

    with Continuous(
        api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
    ) as c_client:
        # Rest of application here...


# Or when using async:
async def amain():

    async with Continuous(
        api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
    ) as c_client:
        # Rest of application here...
```
<!-- End Resource Management [resource-management] -->

<!-- Start Debugging [debug] -->
## Debugging

You can setup your SDK to emit debug logs for SDK requests and responses.

You can pass your own logger class directly into your SDK.
```python
from continuous import Continuous
import logging

logging.basicConfig(level=logging.DEBUG)
s = Continuous(debug_logger=logging.getLogger("continuous"))
```

You can also enable a default debug logger by setting an environment variable `CONTINUOUS_DEBUG` to true.
<!-- End Debugging [debug] -->

<!-- Placeholder for Future Speakeasy SDK Sections -->

# Development

## Maturity

This SDK is in beta, and there may be breaking changes between versions without a major version update. Therefore, we recommend pinning usage
to a specific package version. This way, you can install the same version each time without breaking changes unless you are intentionally
looking for the latest version.

## Contributions

While we value open-source contributions to this SDK, this library is generated programmatically. Any manual changes added to internal files will be overwritten on the next generation. 
We look forward to hearing your feedback. Feel free to open a PR or an issue with a proof of concept and we'll do our best to include it in a future release. 

### SDK Created by [Speakeasy](https://www.speakeasy.com/?utm_source=continuous-simulation&utm_campaign=python)
