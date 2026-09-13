# Simulations

## Overview

Create Simulations from ready Simulators, then fork, stop, start, and delete them.

### Available Operations

* [list_simulations](#list_simulations) - List Simulations
* [create_simulation](#create_simulation) - Create Simulation
* [delete_simulation](#delete_simulation) - Delete Simulation
* [get_simulation](#get_simulation) - Get Simulation
* [advance_simulation_time](#advance_simulation_time) - Advance Simulation Time
* [get_simulation_advance](#get_simulation_advance) - Get Simulation Clock Advance
* [list_simulation_advance_events](#list_simulation_advance_events) - List Clock Advance Events
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
| `status`                                                                        | [Optional[models.ListSimulationsStatus]](../../models/listsimulationsstatus.md) | :heavy_minus_sign:                                                              | Optional status filter.                                                         |
| `simulator_id`                                                                  | *Optional[str]*                                                                 | :heavy_minus_sign:                                                              | Optional Simulator ID filter.                                                   |
| `limit`                                                                         | *Optional[int]*                                                                 | :heavy_minus_sign:                                                              | Page size. Values below 1 use 50. Values above 200 use 200.                     |
| `cursor`                                                                        | *Optional[str]*                                                                 | :heavy_minus_sign:                                                              | Opaque next_cursor value from a previous page.                                  |
| `retries`                                                                       | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                | :heavy_minus_sign:                                                              | Configuration to override the default retry behavior of the client.             |

### Response

**[models.ListSimulationsResponse](../../models/listsimulationsresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_simulation

Creates a Simulation from a ready Simulator and starts it. The response includes the Simulation endpoint and a token that expires in 1 hour. List and get do not return the token.

### Example Usage: bad_request_body

<!-- UsageSnippet language="python" operationID="create-simulation" method="post" path="/v1/simulations" example="bad_request_body" -->
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
### Example Usage: simulator_unknown

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

| Parameter                                                                                                 | Type                                                                                                      | Required                                                                                                  | Description                                                                                               |
| --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| `simulator_id`                                                                                            | *str*                                                                                                     | :heavy_check_mark:                                                                                        | ID of the ready Simulator.                                                                                |
| `name`                                                                                                    | *Optional[str]*                                                                                           | :heavy_minus_sign:                                                                                        | Optional Simulation name. Omission generates a name.                                                      |
| `start_time`                                                                                              | [date](https://docs.python.org/3/library/datetime.html#date-objects)                                      | :heavy_minus_sign:                                                                                        | Initial simulated time in RFC 3339 format. Omission uses 2024-01-01T00:00:00Z. Precision is milliseconds. |
| `retries`                                                                                                 | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                          | :heavy_minus_sign:                                                                                        | Configuration to override the default retry behavior of the client.                                       |

### Response

**[models.CreatedSimulation](../../models/createdsimulation.md)**

### Errors

| Error Type                             | Status Code                            | Content Type                           |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| errors.Error                           | 400, 401, 408, 409, 413, 415, 422, 429 | application/problem+json               |
| errors.Error                           | 500, 503, 504                          | application/problem+json               |
| errors.ContinuousDefaultError          | 4XX, 5XX                               | \*/\*                                  |

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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_simulation

Returns a Simulation and its current status. The response does not include tokens.

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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Simulation](../../models/simulation.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## advance_simulation_time

Schedules an absolute clock advance. Each successful advance commits all due local events in one step. World members advance through their World. Poll the returned operation until it completes.

### Example Usage

<!-- UsageSnippet language="python" operationID="advance-simulation-time" method="post" path="/v1/simulations/{id}/advance-time" example="bad_request_body" -->
```python
from continuous import Continuous
from continuous.utils import parse_datetime
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.advance_simulation_time(id="<id>", idempotency_key="<value>", to=parse_datetime("2026-11-25T01:01:24.107Z"))

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                           | Type                                                                                | Required                                                                            | Description                                                                         |
| ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| `id`                                                                                | *str*                                                                               | :heavy_check_mark:                                                                  | Simulation or World ID.                                                             |
| `idempotency_key`                                                                   | *str*                                                                               | :heavy_check_mark:                                                                  | Stable key for this request. Reuse with the same target returns the same operation. |
| `to`                                                                                | [date](https://docs.python.org/3/library/datetime.html#date-objects)                | :heavy_check_mark:                                                                  | Absolute target time in RFC 3339, with at most millisecond precision.               |
| `retries`                                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                    | :heavy_minus_sign:                                                                  | Configuration to override the default retry behavior of the client.                 |

### Response

**[models.ClockAdvance](../../models/clockadvance.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 403, 404, 408, 409, 413, 415, 422 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## get_simulation_advance

Returns durable clock progress, the event count, and the committed step or failure.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-simulation-advance" method="get" path="/v1/simulations/{id}/advances/{advance_id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.get_simulation_advance(id="<id>", advance_id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation or World ID.                                             |
| `advance_id`                                                        | *str*                                                               | :heavy_check_mark:                                                  | Clock advance operation ID.                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ClockAdvance](../../models/clockadvance.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## list_simulation_advance_events

Returns the ordered event trace for a committed advance. The Simulation must be running or paused. Forks retain traces in their inherited state.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-simulation-advance-events" method="get" path="/v1/simulations/{id}/advances/{advance_id}/events" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.list_simulation_advance_events(id="<id>", advance_id="<id>", limit=50)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
| `advance_id`                                                        | *str*                                                               | :heavy_check_mark:                                                  | Advance ID. A historical fork can read inherited runtime receipts.  |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Cursor from the previous page.                                      |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size, up to 200.                                               |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListAdvanceEventsOutputBody](../../models/listadvanceeventsoutputbody.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409, 422  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## fork_simulation

Creates a new Simulation from the source Simulation's current state, or from an earlier recorded step when you set at_step. The source must be running or paused; a stopped source returns 409 simulation_stopped. Forking does not change the source. The response includes the new endpoint and a token that expires in 1 hour.

### Example Usage

<!-- UsageSnippet language="python" operationID="fork-simulation" method="post" path="/v1/simulations/{id}/fork" example="bad_request_body" -->
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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Source Simulation ID.                                               |
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

Starts a stopped Simulation from its saved state. The endpoint serves requests once the response returns. A Simulation that is already running or paused is returned unchanged.

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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Simulation](../../models/simulation.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409, 429       | application/problem+json      |
| errors.Error                  | 500, 503, 504                 | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## list_simulation_steps

Lists the Simulation's steps in order. Each request that changed state is one step; a request that only reads registers none. Pass a step number as at_step when you fork to start the child from the state after that step. A stopped Simulation returns 409 simulation_stopped; start it first.

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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
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

Stops a Simulation and saves its state. Requests to its endpoint return 409 simulation_stopped until you start it again. A stopped Simulation is returned unchanged.

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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.Simulation](../../models/simulation.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## mint_simulation_token

Creates another token for requests to the Simulation endpoint. Send it in the X-Continuous-Simulation-Token header. Earlier tokens stay valid until they expire.

### Example Usage

<!-- UsageSnippet language="python" operationID="mint-simulation-token" method="post" path="/v1/simulations/{id}/tokens" example="bad_request_body" -->
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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
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