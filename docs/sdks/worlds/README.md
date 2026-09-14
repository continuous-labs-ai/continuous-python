# Worlds

## Overview

Build Worlds from one or more Simulators and start or stop their Simulations together.

### Available Operations

* [list_worlds](#list_worlds) - List Worlds
* [build_world](#build_world) - Build World
* [delete_world](#delete_world) - Delete World
* [get_world](#get_world) - Get World
* [advance_world_time](#advance_world_time) - Advance World Time
* [get_world_advance](#get_world_advance) - Get World Clock Advance
* [cancel_world_build](#cancel_world_build) - Cancel World Build
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

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size. Values below 1 use 50. Values above 200 use 200.         |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Opaque next_cursor value from a previous page.                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListWorldsResponse](../../models/listworldsresponse.md)**

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
    ], builder="claude", instructions="Use stable example data for each Simulator.", timeout_seconds=3600)

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
    ], builder="claude", instructions="Use stable example data for each Simulator.", timeout_seconds=3600)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                                                                   | Type                                                                                                                                                                                                                                                                        | Required                                                                                                                                                                                                                                                                    | Description                                                                                                                                                                                                                                                                 |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `simulators`                                                                                                                                                                                                                                                                | List[*str*]                                                                                                                                                                                                                                                                 | :heavy_check_mark:                                                                                                                                                                                                                                                          | Simulator IDs for the World.                                                                                                                                                                                                                                                |
| `builder`                                                                                                                                                                                                                                                                   | [Optional[models.BuildWorldRequestBuilder]](../../models/buildworldrequestbuilder.md)                                                                                                                                                                                       | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Model provider that builds starting data. Defaults to claude.                                                                                                                                                                                                               |
| `instructions`                                                                                                                                                                                                                                                              | *Optional[str]*                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Describe the initial data, scenario, and relationships. Populated Worlds support up to 8 selected Simulators and 1,000 starting records in total. Named record types replace their default data. At most 16,384 characters and 65,536 UTF-8 bytes. U+0000 is not permitted. |
| `name`                                                                                                                                                                                                                                                                      | *Optional[str]*                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Name for the World. Omission generates a name. The ID stays its identity, and names need not be unique.                                                                                                                                                                     |
| `start_time`                                                                                                                                                                                                                                                                | [date](https://docs.python.org/3/library/datetime.html#date-objects)                                                                                                                                                                                                        | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Simulated time the World starts at, in RFC 3339 format. Omission uses 2024-01-01T00:00:00Z. Saved starting data keeps its build dates.                                                                                                                                      |
| `timeout_seconds`                                                                                                                                                                                                                                                           | *Optional[int]*                                                                                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                                          | Time limit for generation and validation in seconds, from 1 to 43200. Defaults to 3600 (one hour). Excludes queue wait and finalization. Retries share the same deadline.                                                                                                   |
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

## advance_world_time

Fences all members, advances each local clock, and returns a durable operation. A partial failure keeps members fenced while the operation retries. The World clock changes after all members commit.

### Example Usage

<!-- UsageSnippet language="python" operationID="advance-world-time" method="post" path="/v1/worlds/{id}/advance-time" example="bad_request_body" -->
```python
from continuous import Continuous
from continuous.utils import parse_datetime
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.advance_world_time(id="<id>", idempotency_key="<value>", to=parse_datetime("2026-11-05T04:15:58.628Z"))

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

## get_world_advance

Returns durable progress for each member. Members remain fenced until the whole advance can finish.

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
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Simulation or World ID.                                             |
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

## cancel_world_build

Cancels an active World build. Repeated cancellation returns the current World.

### Example Usage

<!-- UsageSnippet language="python" operationID="cancel-world-build" method="post" path="/v1/worlds/{id}/cancel" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.worlds.cancel_world_build(id="<id>")

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

## start_world

Starts every Simulation in the World. The first start creates the Simulations; later starts restore them from saved state. The World must be ready or stopped, and the workspace must have room for all members under its active-Simulation limit. A running World is returned unchanged.

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

Stops a World and saves each Simulation state. You can start the World later from the saved states.

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