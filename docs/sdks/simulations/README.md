# Simulations

## Overview

Create and control isolated runtime instances of ready Simulators.

### Available Operations

* [list_simulations](#list_simulations) - List Simulations
* [create_simulation](#create_simulation) - Create Simulation
* [delete_simulation](#delete_simulation) - Delete Simulation
* [get_simulation](#get_simulation) - Get Simulation
* [fork_simulation](#fork_simulation) - Fork Simulation
* [start_simulation](#start_simulation) - Start Simulation
* [list_simulation_steps](#list_simulation_steps) - List Simulation Steps
* [stop_simulation](#stop_simulation) - Stop Simulation
* [mint_simulation_token](#mint_simulation_token) - Mint Simulation Token

## list_simulations

Returns all Simulations that the API key can access. Results can be filtered by status or Simulator.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-simulations" method="get" path="/v1/simulations" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.list_simulations(limit=50)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                       | Type                                                                            | Required                                                                        | Description                                                                     |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `status`                                                                        | [Optional[models.ListSimulationsStatus]](../../models/listsimulationsstatus.md) | :heavy_minus_sign:                                                              | Optional lifecycle status filter.                                               |
| `simulator_id`                                                                  | *Optional[str]*                                                                 | :heavy_minus_sign:                                                              | Optional stable Simulator ID filter.                                            |
| `limit`                                                                         | *Optional[int]*                                                                 | :heavy_minus_sign:                                                              | Page size. Values below 1 use 50. Values above 200 use 200.                     |
| `cursor`                                                                        | *Optional[str]*                                                                 | :heavy_minus_sign:                                                              | Opaque next_cursor value from a previous page.                                  |
| `retries`                                                                       | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                | :heavy_minus_sign:                                                              | Configuration to override the default retry behavior of the client.             |

### Response

**[models.ListSimulationsResponse](../../models/listsimulationsresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 422       | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_simulation

Creates an isolated runtime from a ready Simulator. The response includes its endpoint and a 1-hour token.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-simulation" method="post" path="/v1/simulations" example="simulator_unknown" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.create_simulation(simulator_id="smr_01J8Z5X4K7M2N9P0Q1R2S3T4V5", name="billing-sandbox")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `simulator_id`                                                      | *str*                                                               | :heavy_check_mark:                                                  | Stable ID of the ready Simulator.                                   |
| `name`                                                              | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Optional Simulation name. Omission generates a name.                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.CreatedSimulation](../../models/createdsimulation.md)**

### Errors

| Error Type                                       | Status Code                                      | Content Type                                     |
| ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ |
| errors.Error                                     | 400, 401, 403, 404, 408, 409, 413, 415, 422, 429 | application/problem+json                         |
| errors.Error                                     | 500, 503, 504                                    | application/problem+json                         |
| errors.ContinuousDefaultError                    | 4XX, 5XX                                         | \*/\*                                            |

## delete_simulation

Deletes a Simulation and its saved runtime state. Delete World-owned Simulations through their World.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-simulation" method="delete" path="/v1/simulations/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.simulations.delete_simulation(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable resource ID.                                                 |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409       | application/problem+json      |
| errors.Error                  | 500, 503, 504                 | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_simulation

Returns a Simulation and its current runtime status. The response does not include data-plane tokens.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-simulation" method="get" path="/v1/simulations/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.get_simulation(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable Simulation ID.                                               |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Simulation](../../models/simulation.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## fork_simulation

Creates a Simulation with a 1-hour token. The source Simulation continues to run.

### Example Usage

<!-- UsageSnippet language="python" operationID="fork-simulation" method="post" path="/v1/simulations/{id}/fork" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.fork_simulation(id="<id>", at_step=42, name="billing-fork")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable source Simulation ID.                                        |
| `at_step`                                                           | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Completed step to fork from. Omission forks from the latest state.  |
| `name`                                                              | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Optional child Simulation name. Omission generates a name.          |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.CreatedSimulation](../../models/createdsimulation.md)**

### Errors

| Error Type                                       | Status Code                                      | Content Type                                     |
| ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ |
| errors.Error                                     | 400, 401, 403, 404, 408, 409, 413, 415, 422, 429 | application/problem+json                         |
| errors.Error                                     | 500, 503, 504                                    | application/problem+json                         |
| errors.ContinuousDefaultError                    | 4XX, 5XX                                         | \*/\*                                            |

## start_simulation

Starts a stopped Simulation from its saved runtime state. Its endpoint becomes available after the runtime starts.

### Example Usage

<!-- UsageSnippet language="python" operationID="start-simulation" method="post" path="/v1/simulations/{id}/start" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.start_simulation(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable Simulation ID.                                               |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Simulation](../../models/simulation.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409, 429  | application/problem+json      |
| errors.Error                  | 500, 503, 504                 | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## list_simulation_steps

Returns recorded steps for a running or paused Simulation. Use any step to create a deterministic fork.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-simulation-steps" method="get" path="/v1/simulations/{id}/steps" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.list_simulation_steps(id="<id>", limit=50)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable Simulation ID.                                               |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Opaque next_cursor value from a previous page.                      |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size. Values below 1 use 50. Values above 200 use 200.         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListSimulationStepsResponse](../../models/listsimulationstepsresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409, 422  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## stop_simulation

Stops a Simulation and saves its runtime state. You can start it later from the saved state.

### Example Usage

<!-- UsageSnippet language="python" operationID="stop-simulation" method="post" path="/v1/simulations/{id}/stop" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.stop_simulation(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable Simulation ID.                                               |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Simulation](../../models/simulation.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409       | application/problem+json      |
| errors.Error                  | 500, 503, 504                 | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## mint_simulation_token

Mints an expiring data-plane token. Send it in X-Continuous-Simulation-Token. Other unexpired tokens remain valid.

### Example Usage

<!-- UsageSnippet language="python" operationID="mint-simulation-token" method="post" path="/v1/simulations/{id}/tokens" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.mint_simulation_token(id="<id>", ttl_seconds=3600)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Stable Simulation ID.                                               |
| `ttl_seconds`                                                       | *int*                                                               | :heavy_check_mark:                                                  | Token lifetime in seconds, from 60 through 86,400.                  |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.SimulationToken](../../models/simulationtoken.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 403, 404, 408, 409, 413, 415, 422 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |