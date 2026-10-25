# Simulations

## Overview

Create Simulations from ready Simulators or Snapshots, then rename, stop, start, and delete them.

### Available Operations

* [list_simulations](#list_simulations) - List Simulations
* [create_simulation](#create_simulation) - Create Simulation
* [delete_simulation](#delete_simulation) - Delete Simulation
* [get_simulation](#get_simulation) - Get Simulation
* [update_simulation](#update_simulation) - Update Simulation
* [create_simulation_advance](#create_simulation_advance) - Advance Simulation Time
* [get_simulation_advance](#get_simulation_advance) - Get Simulation Clock Advance
* [list_simulation_advance_events](#list_simulation_advance_events) - List Clock Advance Events
* [start_simulation](#start_simulation) - Start Simulation
* [list_simulation_steps](#list_simulation_steps) - List Simulation Steps
* [stop_simulation](#stop_simulation) - Stop Simulation
* [get_simulation_token](#get_simulation_token) - Get Current Simulation Token
* [regenerate_simulation_token](#regenerate_simulation_token) - Regenerate Simulation Token

## list_simulations

Returns all Simulations that the API key can access. Results can be filtered by status, Simulator ID, pinned Simulator digest, or the Snapshot they started from.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-simulations" method="get" path="/v1/simulations" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.list_simulations(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                                       | Type                                                                            | Required                                                                        | Description                                                                     |
| ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| `status`                                                                        | [Optional[models.ListSimulationsStatus]](../../models/listsimulationsstatus.md) | :heavy_minus_sign:                                                              | Optional status filter.                                                         |
| `simulator_id`                                                                  | *Optional[str]*                                                                 | :heavy_minus_sign:                                                              | Optional Simulator ID filter.                                                   |
| `simulator_digest`                                                              | *Optional[str]*                                                                 | :heavy_minus_sign:                                                              | Optional exact pinned OCI manifest digest filter.                               |
| `snapshot_id`                                                                   | *Optional[str]*                                                                 | :heavy_minus_sign:                                                              | Optional filter for the Simulations started from this Snapshot.                 |
| `limit`                                                                         | *Optional[int]*                                                                 | :heavy_minus_sign:                                                              | Page size. Values below 1 use 50. Values above 200 use 200.                     |
| `cursor`                                                                        | *Optional[str]*                                                                 | :heavy_minus_sign:                                                              | Opaque next_cursor value from a previous page.                                  |
| `retries`                                                                       | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                | :heavy_minus_sign:                                                              | Configuration to override the default retry behavior of the client.             |

### Response

**[models.ListSimulationsResponseResponse](../../models/listsimulationsresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_simulation

Creates a Simulation from a ready Simulator, or from a Snapshot with its saved state, and starts it. The response returns once the Simulation is admitted, with status starting; read it until it is running. The response includes the endpoint and the Simulation's token, which does not expire. If its runtime cannot start, the Simulation is deleted.

### Example Usage: bad_request_body

<!-- UsageSnippet language="python" operationID="create-simulation" method="post" path="/v1/simulations" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.create_simulation(metadata={
        "customer_id": "cust_123",
    }, name="billing-sandbox", simulator_id="smr_01J8Z5X4K7M2N9P0Q1R2S3T4V5")

    # Handle response
    print(res)

```
### Example Usage: bad_request_sample_data

<!-- UsageSnippet language="python" operationID="create-simulation" method="post" path="/v1/simulations" example="bad_request_sample_data" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.create_simulation(metadata={
        "customer_id": "cust_123",
    }, name="billing-sandbox", simulator_id="smr_01J8Z5X4K7M2N9P0Q1R2S3T4V5")

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

    res = c_client.simulations.create_simulation(metadata={
        "customer_id": "cust_123",
    }, name="billing-sandbox", simulator_id="smr_01J8Z5X4K7M2N9P0Q1R2S3T4V5")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                                                    | Type                                                                                                                                                                                                                                                         | Required                                                                                                                                                                                                                                                     | Description                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `idempotency_key`                                                                                                                                                                                                                                            | *Optional[str]*                                                                                                                                                                                                                                              | :heavy_minus_sign:                                                                                                                                                                                                                                           | Optional key for retrying this create safely. A retry with the same key and request returns the original result with its status; the same key with a different request returns 409 idempotency_conflict. Without a key, each request creates a new resource. |
| `include_sample_data`                                                                                                                                                                                                                                        | *Optional[bool]*                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                           | Start with the Simulator's sample data: a realistic account loaded on top of its seed before the first request. Defaults to false, a brand-new workspace. Requires a Simulator that carries sample data; see its composition. Not allowed with snapshot_id.  |
| `metadata`                                                                                                                                                                                                                                                   | *Optional[Any]*                                                                                                                                                                                                                                              | :heavy_minus_sign:                                                                                                                                                                                                                                           | Customer JSON metadata, up to 16 KiB and 64 nesting levels. Returned by create, get, and list. Omission uses null.                                                                                                                                           |
| `name`                                                                                                                                                                                                                                                       | *Optional[str]*                                                                                                                                                                                                                                              | :heavy_minus_sign:                                                                                                                                                                                                                                           | Name for the Simulation. Omission generates a name. The ID stays its identity, and names need not be unique.                                                                                                                                                 |
| `simulator_id`                                                                                                                                                                                                                                               | *Optional[str]*                                                                                                                                                                                                                                              | :heavy_minus_sign:                                                                                                                                                                                                                                           | ID of the ready Simulator. Set exactly one of simulator_id and snapshot_id.                                                                                                                                                                                  |
| `snapshot_id`                                                                                                                                                                                                                                                | *Optional[str]*                                                                                                                                                                                                                                              | :heavy_minus_sign:                                                                                                                                                                                                                                           | ID of the Snapshot to start from. The Simulation takes its state, clock, Simulator digest and runtime placement. Set exactly one of simulator_id and snapshot_id.                                                                                            |
| `start_time`                                                                                                                                                                                                                                                 | [date](https://docs.python.org/3/library/datetime.html#date-objects)                                                                                                                                                                                         | :heavy_minus_sign:                                                                                                                                                                                                                                           | Initial simulated time in RFC 3339 format. Omission uses 2024-01-01T00:00:00Z. Precision is milliseconds. Not allowed with snapshot_id.                                                                                                                      |
| `retries`                                                                                                                                                                                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                           | Configuration to override the default retry behavior of the client.                                                                                                                                                                                          |

### Response

**[models.CreateSimulationResponse](../../models/createsimulationresponse.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 404, 408, 409, 413, 415, 422, 429 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## delete_simulation

Deletes a Simulation and its saved runtime state, and cancels an active advance. Delete World-owned Simulations through their World.

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

Returns a Simulation, its current status, and the actors a request can act as. The response does not include tokens.

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

**[models.SimulationDetail](../../models/simulationdetail.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_simulation

Renames a Simulation or replaces its metadata. Omitted fields keep their current values. Update World-owned Simulations through their World.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-simulation" method="patch" path="/v1/simulations/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.update_simulation(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                    | Type                                                                                         | Required                                                                                     | Description                                                                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `id`                                                                                         | *str*                                                                                        | :heavy_check_mark:                                                                           | Simulation ID.                                                                               |
| `metadata`                                                                                   | *Optional[Any]*                                                                              | :heavy_minus_sign:                                                                           | Replacement customer JSON metadata, up to 16 KiB and 64 nesting levels, or null to clear it. |
| `name`                                                                                       | *Optional[str]*                                                                              | :heavy_minus_sign:                                                                           | New Simulation name.                                                                         |
| `retries`                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                             | :heavy_minus_sign:                                                                           | Configuration to override the default retry behavior of the client.                          |

### Response

**[models.Simulation](../../models/simulation.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 403, 404, 408, 409, 413, 415, 422 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## create_simulation_advance

Schedules an absolute clock advance. An advance commits due local events in ordered batches, one step each; a failure keeps the batches already committed. World members advance through their World. Poll the returned operation until it completes.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-simulation-advance" method="post" path="/v1/simulations/{id}/advances" example="bad_request_body" -->
```python
from continuous import Continuous
from continuous.utils import parse_datetime
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.create_simulation_advance(id="<id>", idempotency_key="<value>", to=parse_datetime("2026-07-12T00:40:22.318Z"))

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                           | Type                                                                                                                                                | Required                                                                                                                                            | Description                                                                                                                                         |
| --------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                                                                                                                                                | *str*                                                                                                                                               | :heavy_check_mark:                                                                                                                                  | Simulation ID.                                                                                                                                      |
| `idempotency_key`                                                                                                                                   | *str*                                                                                                                                               | :heavy_check_mark:                                                                                                                                  | Stable key for this request. Reuse with the same target returns the same operation; reuse with a different target returns 409 idempotency_conflict. |
| `to`                                                                                                                                                | [date](https://docs.python.org/3/library/datetime.html#date-objects)                                                                                | :heavy_check_mark:                                                                                                                                  | Absolute target time in RFC 3339, with at most millisecond precision.                                                                               |
| `retries`                                                                                                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                    | :heavy_minus_sign:                                                                                                                                  | Configuration to override the default retry behavior of the client.                                                                                 |

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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
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

Returns the ordered event trace for a committed advance. The Simulation must be running. A Simulation created from a Snapshot keeps the traces in the state it started from.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-simulation-advance-events" method="get" path="/v1/simulations/{id}/advances/{advance_id}/events" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.list_simulation_advance_events(id="<id>", advance_id="<id>", limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                                                                    | Type                                                                                                         | Required                                                                                                     | Description                                                                                                  |
| ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| `id`                                                                                                         | *str*                                                                                                        | :heavy_check_mark:                                                                                           | Simulation ID.                                                                                               |
| `advance_id`                                                                                                 | *str*                                                                                                        | :heavy_check_mark:                                                                                           | Advance ID. A Simulation created from a Snapshot can read the runtime receipts in the state it started from. |
| `limit`                                                                                                      | *Optional[int]*                                                                                              | :heavy_minus_sign:                                                                                           | Page size. Values below 1 use 50. Values above 200 use 200.                                                  |
| `cursor`                                                                                                     | *Optional[str]*                                                                                              | :heavy_minus_sign:                                                                                           | Opaque next_cursor value from a previous page.                                                               |
| `retries`                                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                             | :heavy_minus_sign:                                                                                           | Configuration to override the default retry behavior of the client.                                          |

### Response

**[models.ListSimulationAdvanceEventsResponse](../../models/listsimulationadvanceeventsresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409, 422  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## start_simulation

Starts a stopped Simulation from its saved state. The response returns once the start is admitted, with status starting; read the Simulation until it is running. If its runtime cannot start, it returns to stopped. Stop and start do not change its token; read it with GET /v1/simulations/{id}/token. A running or starting Simulation is returned unchanged.

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
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## list_simulation_steps

Lists the Simulation's steps in order. Each request that changed state is one step; a request that only reads registers none. A stopped Simulation returns 409 simulation_stopped; start it first.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-simulation-steps" method="get" path="/v1/simulations/{id}/steps" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.list_simulation_steps(id="<id>", limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Opaque next_cursor value from a previous page.                      |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size. Values below 1 use 50. Values above 200 use 200.         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListSimulationStepsResponseResponse](../../models/listsimulationstepsresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 403, 404, 409, 422  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## stop_simulation

Stops a Simulation and saves its state. The response returns once the stop is accepted, with status stopping; read the Simulation until it is stopped. Requests to its endpoint are refused while it stops, and return 409 simulation_stopped once it is stopped, until you start it again. A stopped or stopping Simulation is returned unchanged.

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

## get_simulation_token

Returns the Simulation's current token without rotating it, including while the Simulation is stopped.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-simulation-token" method="get" path="/v1/simulations/{id}/token" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.get_simulation_token(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.GetSimulationTokenResponse](../../models/getsimulationtokenresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## regenerate_simulation_token

Replaces the Simulation's token, including while the Simulation is stopped. When the replacement commits, requests with the previous token return 401 auth_invalid with X-Continuous-Simulation-Token-Rejected: true. Reuse the Idempotency-Key to retry safely.

### Example Usage

<!-- UsageSnippet language="python" operationID="regenerate-simulation-token" method="post" path="/v1/simulations/{id}/token/regenerate" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulations.regenerate_simulation_token(id="<id>", idempotency_key="<value>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation ID.                                                      |
| `idempotency_key`                                                   | *str*                                                               | :heavy_check_mark:                                                  | Stable key for this regeneration request.                           |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.RegenerateSimulationTokenResponse](../../models/regeneratesimulationtokenresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409, 422       | application/problem+json      |
| errors.Error                  | 500, 502, 503                 | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |