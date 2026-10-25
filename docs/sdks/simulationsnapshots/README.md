# SimulationSnapshots

## Overview

Save a running Simulation's state as a Snapshot, create Simulations from it, and rename and delete it.

### Available Operations

* [list_simulation_snapshots](#list_simulation_snapshots) - List Simulation Snapshots
* [create_simulation_snapshot](#create_simulation_snapshot) - Create Simulation Snapshot
* [delete_simulation_snapshot](#delete_simulation_snapshot) - Delete Simulation Snapshot
* [get_simulation_snapshot](#get_simulation_snapshot) - Get Simulation Snapshot
* [update_simulation_snapshot](#update_simulation_snapshot) - Update Simulation Snapshot

## list_simulation_snapshots

Returns the workspace's Snapshots, newest first. Filter by simulation_id for the Snapshots of one Simulation.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-simulation-snapshots" method="get" path="/v1/simulation-snapshots" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulation_snapshots.list_simulation_snapshots(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `simulation_id`                                                     | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Optional filter for the Snapshots saved from this Simulation.       |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size. Values below 1 use 50. Values above 200 use 200.         |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Opaque next_cursor value from a previous page.                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListSimulationSnapshotsResponseResponse](../../models/listsimulationsnapshotsresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_simulation_snapshot

Saves a running Simulation's current state and returns the Snapshot once the state is stored. Saving closes the Simulation's open event streams and WebSockets and does not change its state. Create Simulations from the Snapshot with snapshot_id; it outlives its Simulation. A stopped Simulation returns 409 simulation_stopped. Reuse the Idempotency-Key to retry safely.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-simulation-snapshot" method="post" path="/v1/simulation-snapshots" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulation_snapshots.create_simulation_snapshot(name="before-refund", simulation_id="sim_01J8Z5X4K7M2N9P0Q1R2S3T4V6", idempotency_key="<value>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                                                    | Type                                                                                                                                                                                                                                                         | Required                                                                                                                                                                                                                                                     | Description                                                                                                                                                                                                                                                  |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `name`                                                                                                                                                                                                                                                       | *str*                                                                                                                                                                                                                                                        | :heavy_check_mark:                                                                                                                                                                                                                                           | Snapshot name.                                                                                                                                                                                                                                               |
| `simulation_id`                                                                                                                                                                                                                                              | *str*                                                                                                                                                                                                                                                        | :heavy_check_mark:                                                                                                                                                                                                                                           | ID of the running Simulation to save.                                                                                                                                                                                                                        |
| `idempotency_key`                                                                                                                                                                                                                                            | *Optional[str]*                                                                                                                                                                                                                                              | :heavy_minus_sign:                                                                                                                                                                                                                                           | Optional key for retrying this create safely. A retry with the same key and request returns the original result with its status; the same key with a different request returns 409 idempotency_conflict. Without a key, each request creates a new resource. |
| `retries`                                                                                                                                                                                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                             | :heavy_minus_sign:                                                                                                                                                                                                                                           | Configuration to override the default retry behavior of the client.                                                                                                                                                                                          |

### Response

**[models.SimulationSnapshot](../../models/simulationsnapshot.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 403, 404, 408, 409, 413, 415, 422 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## delete_simulation_snapshot

Deletes a Snapshot and its saved state. Simulations created from it keep running, and their snapshot_id becomes null. While it is creating, the response is 409 snapshot_not_ready; while a Simulation is starting from it, 409 snapshot_in_use.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-simulation-snapshot" method="delete" path="/v1/simulation-snapshots/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.simulation_snapshots.delete_simulation_snapshot(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Snapshot ID.                                                        |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_simulation_snapshot

Returns a Snapshot.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-simulation-snapshot" method="get" path="/v1/simulation-snapshots/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulation_snapshots.get_simulation_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Snapshot ID.                                                        |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.SimulationSnapshot](../../models/simulationsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_simulation_snapshot

Renames a Snapshot. Omitted fields keep their current values.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-simulation-snapshot" method="patch" path="/v1/simulation-snapshots/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulation_snapshots.update_simulation_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | Snapshot ID.                                                        |
| `name`                                                              | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | New Snapshot name.                                                  |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.SimulationSnapshot](../../models/simulationsnapshot.md)**

### Errors

| Error Type                        | Status Code                       | Content Type                      |
| --------------------------------- | --------------------------------- | --------------------------------- |
| errors.Error                      | 400, 401, 404, 408, 413, 415, 422 | application/problem+json          |
| errors.Error                      | 500, 503                          | application/problem+json          |
| errors.ContinuousDefaultError     | 4XX, 5XX                          | \*/\*                             |