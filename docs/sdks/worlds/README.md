# Worlds

## Overview

Build Worlds from one or more Simulators, rename them, and start or stop their Simulations together.

### Available Operations

* [list_worlds](#list_worlds) - List Worlds
* [build_world](#build_world) - Build World
* [delete_world](#delete_world) - Delete World
* [get_world](#get_world) - Get World
* [update_world](#update_world) - Update World
* [create_world_advance](#create_world_advance) - Advance World Time
* [get_world_advance](#get_world_advance) - Get World Clock Advance
* [cancel_world](#cancel_world) - Cancel World Build
* [get_world_change](#get_world_change) - Get World Change
* [add_world_simulation](#add_world_simulation) - Add World Simulation
* [remove_world_simulation](#remove_world_simulation) - Remove World Simulation
* [start_world](#start_world) - Start World
* [stop_world](#stop_world) - Stop World

## list_worlds

Returns all Worlds that the API key can access.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-worlds" method="get" path="/v1/worlds" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.list_worlds(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                             | Type                                                                  | Required                                                              | Description                                                           |
| --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- | --------------------------------------------------------------------- |
| `status`                                                              | [Optional[models.ListWorldsStatus]](../../models/listworldsstatus.md) | :heavy_minus_sign:                                                    | Optional status filter.                                               |
| `limit`                                                               | *Optional[int]*                                                       | :heavy_minus_sign:                                                    | Page size. Values below 1 use 50. Values above 200 use 200.           |
| `cursor`                                                              | *Optional[str]*                                                       | :heavy_minus_sign:                                                    | Opaque next_cursor value from a previous page.                        |
| `retries`                                                             | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)      | :heavy_minus_sign:                                                    | Configuration to override the default retry behavior of the client.   |

### Response

**[models.ListWorldsResponseResponse](../../models/listworldsresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## build_world

Starts an asynchronous World build from ready Simulators and returns it in the pending state. Builds start in queue order when workspace capacity is available. Instructions generate and validate initial synthetic data. Start the World once it is ready to create its Simulations.

### Example Usage: bad_request_body

<!-- UsageSnippet language="python" operationID="build-world" method="post" path="/v1/worlds" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.build_world(simulators=[
        "smr_01J8Z5X4K7M2N9P0Q1R2S3T4V5",
    ], instructions="Use stable example data for each Simulator.", metadata={
        "customer_id": "cust_123",
    }, model="claude-opus-5-5", timeout_seconds=14400)

    # Handle response
    print(res)

```
### Example Usage: simulator_unknown

<!-- UsageSnippet language="python" operationID="build-world" method="post" path="/v1/worlds" example="simulator_unknown" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.build_world(simulators=[
        "smr_01J8Z5X4K7M2N9P0Q1R2S3T4V5",
    ], instructions="Use stable example data for each Simulator.", metadata={
        "customer_id": "cust_123",
    }, model="claude-opus-5-5", timeout_seconds=14400)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                                                                   | Type                                                                                                                                                                                                                                                                        | Required                                                                                                                                                                                                                                                                    | Description                                                                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `simulators`                                                                                                                                                                                                                                                                | List[*str*]                                                                                                                                                                                                                                                                 | :heavy_check_mark:                                                                                                                                                                                                                                                          | Simulator IDs for the World.                                                                                                                                                                                                                                                |
| `idempotency_key`                                                                                                                                                                                                                                                           | *Optional[str]*                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Optional key for retrying this create safely. A retry with the same key and request returns the original result with its status; the same key with a different request returns 409 idempotency_conflict. Without a key, each request creates a new resource.                |
| `instructions`                                                                                                                                                                                                                                                              | *Optional[str]*                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Describe the initial data, scenario, and relationships. Populated Worlds support up to 8 selected Simulators and 1,000 starting records in total. Named record types replace their default data. At most 16,384 characters and 65,536 UTF-8 bytes. U+0000 is not permitted. |
| `metadata`                                                                                                                                                                                                                                                                  | *Optional[Any]*                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Customer JSON metadata, up to 16 KiB and 64 nesting levels. Returned by build, get, and list. Omission uses null.                                                                                                                                                           |
| `model`                                                                                                                                                                                                                                                                     | [Optional[models.BuildWorldRequestModel]](../../models/buildworldrequestmodel.md)                                                                                                                                                                                           | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Model that builds and reviews starting data. Defaults to claude-opus-5-5. Its provider is derived from the model.                                                                                                                                                           |
| `name`                                                                                                                                                                                                                                                                      | *Optional[str]*                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Name for the World. Omission generates a name. The ID stays its identity, and names need not be unique.                                                                                                                                                                     |
| `start_time`                                                                                                                                                                                                                                                                | [date](https://docs.python.org/3/library/datetime.html#date-objects)                                                                                                                                                                                                        | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Simulated time the World starts at, in RFC 3339 format. Omission uses 2024-01-01T00:00:00Z. Saved starting data keeps its build dates.                                                                                                                                      |
| `timeout_seconds`                                                                                                                                                                                                                                                           | *Optional[int]*                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Time limit for generation and validation in seconds, from 1 to 72000. Defaults to 14400 (four hours). Excludes queue wait and finalization. Retries share the same deadline.                                                                                                |
| `retries`                                                                                                                                                                                                                                                                   | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                                            | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Configuration to override the default retry behavior of the client.                                                                                                                                                                                                         |

### Response

**[models.World](../../models/world.md)**

### Errors

| Error Type                        | Status Code                       | Content Type                      |
| --------------------------------- | --------------------------------- | --------------------------------- |
| errors.Error                      | 400, 401, 408, 409, 413, 415, 422 | application/problem+json          |
| errors.Error                      | 500, 503                          | application/problem+json          |
| errors.ContinuousDefaultError     | 4XX, 5XX                          | \*/\*                             |

## delete_world

Deletes a World, all its Simulations, and their saved runtime states.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-world" method="delete" path="/v1/worlds/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.worlds.delete_world(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | World ID.                                                           |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_world

Returns a World, its build instructions, and its Simulator IDs.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-world" method="get" path="/v1/worlds/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.get_world(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | World ID.                                                           |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.World](../../models/world.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_world

Renames a World or replaces its metadata. Omitted fields keep their current values.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-world" method="patch" path="/v1/worlds/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.update_world(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                    | Type                                                                                         | Required                                                                                     | Description                                                                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `id`                                                                                         | *str*                                                                                        | :heavy_check_mark:                                                                           | World ID.                                                                                    |
| `metadata`                                                                                   | *Optional[Any]*                                                                              | :heavy_minus_sign:                                                                           | Replacement customer JSON metadata, up to 16 KiB and 64 nesting levels, or null to clear it. |
| `name`                                                                                       | *Optional[str]*                                                                              | :heavy_minus_sign:                                                                           | New World name.                                                                              |
| `retries`                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                             | :heavy_minus_sign:                                                                           | Configuration to override the default retry behavior of the client.                          |

### Response

**[models.World](../../models/world.md)**

### Errors

| Error Type                             | Status Code                            | Content Type                           |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| errors.Error                           | 400, 401, 403, 404, 408, 413, 415, 422 | application/problem+json               |
| errors.Error                           | 500, 503                               | application/problem+json               |
| errors.ContinuousDefaultError          | 4XX, 5XX                               | \*/\*                                  |

## create_world_advance

Advances each member independently, waking an idle member first, and skips stopped members. Successful advances remain committed when another member fails. The World current_time records the last settled request target; member clocks can differ.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-world-advance" method="post" path="/v1/worlds/{id}/advances" example="bad_request_body" -->
```python
from continuous import Continuous
from continuous.utils import parse_datetime
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.create_world_advance(id="<id>", idempotency_key="<value>", to=parse_datetime("2024-03-26T21:37:05.744Z"))

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                           | Type                                                                                                                                                | Required                                                                                                                                            | Description                                                                                                                                         |
| --------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                                                                                                                                                | *str*                                                                                                                                               | :heavy_check_mark:                                                                                                                                  | World ID.                                                                                                                                           |
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

## get_world_advance

Returns completed, failed, or skipped outcomes for each member. A failed call can have an unconfirmed runtime outcome. Reusing the request key returns the same result.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-world-advance" method="get" path="/v1/worlds/{id}/advances/{advance_id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.get_world_advance(id="<id>", advance_id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | World ID.                                                           |
| `advance_id`                                                        | *str*                                                               | :heavy_check_mark:                                                  | Clock advance operation ID.                                         |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ClockAdvance](../../models/clockadvance.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## cancel_world

Requests cancellation of a pending or active World build and returns the World without waiting for the build to stop. Get the World until its status leaves pending or building. The build can finish before cancellation takes effect. A World whose build has settled is returned unchanged.

### Example Usage

<!-- UsageSnippet language="python" operationID="cancel-world" method="post" path="/v1/worlds/{id}/cancel" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.cancel_world(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | World ID.                                                           |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.World](../../models/world.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_world_change

Returns a membership change and its outcome.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-world-change" method="get" path="/v1/worlds/{id}/changes/{change_id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.get_world_change(id="<id>", change_id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | World ID.                                                           |
| `change_id`                                                         | *str*                                                               | :heavy_check_mark:                                                  | Membership change ID.                                               |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.WorldChange](../../models/worldchange.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## add_world_simulation

Adds a ready Simulator to a ready, running, or stopped World as a new member, starting from its Simulator's default data, and returns the change. A running World starts the member at the World's current time; a stopped World creates it at its next start. Poll the change until it completes.

### Example Usage: bad_request_body

<!-- UsageSnippet language="python" operationID="add-world-simulation" method="post" path="/v1/worlds/{id}/simulations" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.add_world_simulation(id="<id>", idempotency_key="<value>", simulator="smr_01J8Z5X4K7M2N9P0Q1R2S3T4V5")

    # Handle response
    print(res)

```
### Example Usage: simulator_unknown

<!-- UsageSnippet language="python" operationID="add-world-simulation" method="post" path="/v1/worlds/{id}/simulations" example="simulator_unknown" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.add_world_simulation(id="<id>", idempotency_key="<value>", simulator="smr_01J8Z5X4K7M2N9P0Q1R2S3T4V5")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                    | Type                                                                                                                                         | Required                                                                                                                                     | Description                                                                                                                                  |
| -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                                                                                                                                         | *str*                                                                                                                                        | :heavy_check_mark:                                                                                                                           | World ID.                                                                                                                                    |
| `idempotency_key`                                                                                                                            | *str*                                                                                                                                        | :heavy_check_mark:                                                                                                                           | Stable key for this request. Reuse with the same body returns the same change; reuse with a different body returns 409 idempotency_conflict. |
| `simulator`                                                                                                                                  | *str*                                                                                                                                        | :heavy_check_mark:                                                                                                                           | ID of the ready Simulator to add as a new member.                                                                                            |
| `retries`                                                                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                             | :heavy_minus_sign:                                                                                                                           | Configuration to override the default retry behavior of the client.                                                                          |

### Response

**[models.WorldChange](../../models/worldchange.md)**

### Errors

| Error Type                                       | Status Code                                      | Content Type                                     |
| ------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------ |
| errors.Error                                     | 400, 401, 403, 404, 408, 409, 413, 415, 422, 429 | application/problem+json                         |
| errors.Error                                     | 500, 503                                         | application/problem+json                         |
| errors.ContinuousDefaultError                    | 4XX, 5XX                                         | \*/\*                                            |

## remove_world_simulation

Removes a member from a running or stopped World and returns the change. By default the member is detached and keeps running as a standalone Simulation; delete=true removes it and its saved state. A World keeps at least one member.

### Example Usage

<!-- UsageSnippet language="python" operationID="remove-world-simulation" method="delete" path="/v1/worlds/{id}/simulations/{simulation_id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.remove_world_simulation(id="<id>", simulation_id="<id>", idempotency_key="<value>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                             | Type                                                                                                                                                                  | Required                                                                                                                                                              | Description                                                                                                                                                           |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                                                                                                                                                                  | *str*                                                                                                                                                                 | :heavy_check_mark:                                                                                                                                                    | World ID.                                                                                                                                                             |
| `simulation_id`                                                                                                                                                       | *str*                                                                                                                                                                 | :heavy_check_mark:                                                                                                                                                    | ID of the member Simulation to remove.                                                                                                                                |
| `idempotency_key`                                                                                                                                                     | *str*                                                                                                                                                                 | :heavy_check_mark:                                                                                                                                                    | Stable key for this request. Reuse with the same member and delete value returns the same change; reuse with a different request returns 409 idempotency_conflict.    |
| `delete`                                                                                                                                                              | *Optional[bool]*                                                                                                                                                      | :heavy_minus_sign:                                                                                                                                                    | true stops and deletes the member and its saved state. false, the default, detaches it: it keeps its ID, endpoint, token, clock, and data as a standalone Simulation. |
| `retries`                                                                                                                                                             | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                      | :heavy_minus_sign:                                                                                                                                                    | Configuration to override the default retry behavior of the client.                                                                                                   |

### Response

**[models.WorldChange](../../models/worldchange.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409, 422       | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## start_world

Starts every Simulation in the World. The first start creates the Simulations; later starts restore them from saved state. The World must be ready or stopped, and the workspace must have room for all members under its active-Simulation limit. The response returns once the start is accepted, with status starting; read the World until it is running. A running or starting World is returned unchanged.

### Example Usage

<!-- UsageSnippet language="python" operationID="start-world" method="post" path="/v1/worlds/{id}/start" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.start_world(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                 | Type                                                                                                                                                                      | Required                                                                                                                                                                  | Description                                                                                                                                                               |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `id`                                                                                                                                                                      | *str*                                                                                                                                                                     | :heavy_check_mark:                                                                                                                                                        | World ID.                                                                                                                                                                 |
| `start_time`                                                                                                                                                              | [date](https://docs.python.org/3/library/datetime.html#date-objects)                                                                                                      | :heavy_minus_sign:                                                                                                                                                        | Simulated time for the first Start, in RFC 3339 format. Omission keeps the clock chosen at build. Saved business dates remain unchanged. Later starts preserve the clock. |
| `retries`                                                                                                                                                                 | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                          | :heavy_minus_sign:                                                                                                                                                        | Configuration to override the default retry behavior of the client.                                                                                                       |

### Response

**[models.World](../../models/world.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 403, 404, 408, 409, 413, 415, 429 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## stop_world

Stops a World and saves each Simulation state. You can start the World later from the saved states. The response returns once the stop is accepted, with status stopping; read the World until it is stopped. A stopped or stopping World is returned unchanged.

### Example Usage

<!-- UsageSnippet language="python" operationID="stop-world" method="post" path="/v1/worlds/{id}/stop" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.stop_world(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | World ID.                                                           |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.World](../../models/world.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 403, 404, 409            | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |