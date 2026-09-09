# Simulators

## Overview

Build, inspect, and delete reusable Simulator artifacts.

### Available Operations

* [list_simulators](#list_simulators) - List Simulators
* [build_simulator](#build_simulator) - Build Simulator
* [delete_simulator](#delete_simulator) - Delete Simulator
* [get_simulator](#get_simulator) - Get Simulator
* [cancel_simulator_build](#cancel_simulator_build) - Cancel Simulator Build

## list_simulators

Returns all Simulators that the API key can access. Results can be filtered by status or name.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-simulators" method="get" path="/v1/simulators" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulators.list_simulators(limit=50)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                            | Type                                                                                                 | Required                                                                                             | Description                                                                                          |
| ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `status`                                                                                             | [Optional[models.ListSimulatorsStatus]](../../models/listsimulatorsstatus.md)                        | :heavy_minus_sign:                                                                                   | Optional build status filter.                                                                        |
| `name`                                                                                               | *Optional[str]*                                                                                      | :heavy_minus_sign:                                                                                   | Optional exact Simulator name. Prefix a catalog name with continuous/. Names cannot start with smr_. |
| `limit`                                                                                              | *Optional[int]*                                                                                      | :heavy_minus_sign:                                                                                   | Page size. Values below 1 use 50. Values above 200 use 200.                                          |
| `cursor`                                                                                             | *Optional[str]*                                                                                      | :heavy_minus_sign:                                                                                   | Opaque next_cursor value from a previous page.                                                       |
| `retries`                                                                                            | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                     | :heavy_minus_sign:                                                                                   | Configuration to override the default retry behavior of the client.                                  |

### Response

**[models.ListSimulatorsResponse](../../models/listsimulatorsresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 422       | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## build_simulator

Creates a Simulator from an OpenAPI or WSDL source. The build runs asynchronously. Send multipart/form-data with one JSON request part, and one spec file part for a spec build.

### Example Usage

<!-- UsageSnippet language="python" operationID="build-simulator" method="post" path="/v1/simulators" example="simulator_unknown_parent" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulators.build_simulator(request={
        "filter_": [],
        "instructions": "Return stable example data for every operation.",
        "name": "billing-api",
        "spec_kind": "openapi",
    })

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                               | Type                                                                                                                                                    | Required                                                                                                                                                | Description                                                                                                                                             | Example                                                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request`                                                                                                                                               | [models.BuildSimulatorRequest](../../models/buildsimulatorrequest.md)                                                                                   | :heavy_check_mark:                                                                                                                                      | N/A                                                                                                                                                     | {<br/>"builder": "claude",<br/>"filter": [],<br/>"instructions": "Return stable example data for every operation.",<br/>"name": "billing-api",<br/>"spec_kind": "openapi"<br/>} |
| `spec`                                                                                                                                                  | [Optional[models.Spec]](../../models/spec.md)                                                                                                           | :heavy_minus_sign:                                                                                                                                      | OpenAPI or WSDL file. A spec build sends it. At most 67,108,864 UTF-8 bytes.                                                                            |                                                                                                                                                         |
| `retries`                                                                                                                                               | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                        | :heavy_minus_sign:                                                                                                                                      | Configuration to override the default retry behavior of the client.                                                                                     |                                                                                                                                                         |

### Response

**[models.Simulator](../../models/simulator.md)**

### Errors

| Error Type                                       | Status Code                                      | Content Type                                     |
| ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ |
| errors.Error                                     | 400, 401, 403, 404, 408, 409, 413, 415, 422, 429 | application/problem+json                         |
| errors.Error                                     | 500, 503                                         | application/problem+json                         |
| errors.ContinuousDefaultError                    | 4XX, 5XX                                         | \*/\*                                            |

## delete_simulator

Deletes a Simulator that has no dependent Worlds, Simulations, or child Simulators.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-simulator" method="delete" path="/v1/simulators/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.simulators.delete_simulator(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable Simulator ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409, 422  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_simulator

Returns a Simulator and its current build status.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-simulator" method="get" path="/v1/simulators/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulators.get_simulator(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable Simulator ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Simulator](../../models/simulator.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 422       | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## cancel_simulator_build

Requests cancellation of an active Simulator build. The build can finish before cancellation takes effect.

### Example Usage

<!-- UsageSnippet language="python" operationID="cancel-simulator-build" method="post" path="/v1/simulators/{id}/cancel" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulators.cancel_simulator_build(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable Simulator ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Simulator](../../models/simulator.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409, 422  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |