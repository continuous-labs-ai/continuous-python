# SimulationSnapshots

## Overview

Save a running Simulation's state as a Snapshot, create Simulations from it, and delete it.

### Available Operations

* [list_simulation_snapshots](#list_simulation_snapshots) - List Simulation Snapshots
* [create_simulation_snapshot](#create_simulation_snapshot) - Create Simulation Snapshot
* [delete_simulation_snapshot](#delete_simulation_snapshot) - Delete Simulation Snapshot
* [get_simulation_snapshot](#get_simulation_snapshot) - Get Simulation Snapshot

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

Saves a running Simulation's current state, or its state after an earlier recorded step when you set at_step, and returns the Snapshot once the state is stored. Saving does not change the Simulation. Create Simulations from the Snapshot with snapshot_id; it outlives its Simulation. A stopped Simulation returns 409 simulation_stopped. Reuse the Idempotency-Key to retry safely.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-simulation-snapshot" method="post" path="/v1/simulation-snapshots" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.simulation_snapshots.create_simulation_snapshot(idempotency_key="<value>", name="before-refund", simulation_id="sim_01J8Z5X4K7M2N9P0Q1R2S3T4V6", at_step=42)

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                             | Type                                                                                  | Required                                                                              | Description                                                                           |
| ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| `idempotency_key`                                                                     | *str*                                                                                 | :heavy_check_mark:                                                                    | Stable key for this request. A retry with the same key returns the original Snapshot. |
| `name`                                                                                | *str*                                                                                 | :heavy_check_mark:                                                                    | Snapshot name.                                                                        |
| `simulation_id`                                                                       | *str*                                                                                 | :heavy_check_mark:                                                                    | ID of the running Simulation to save.                                                 |
| `at_step`                                                                             | *Optional[int]*                                                                       | :heavy_minus_sign:                                                                    | Completed step to save the state after. Omission saves the latest state.              |
| `retries`                                                                             | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                      | :heavy_minus_sign:                                                                    | Configuration to override the default retry behavior of the client.                   |

### Response

**[models.SimulationSnapshot](../../models/simulationsnapshot.md)**

### Errors

| Error Type                                  | Status Code                                 | Content Type                                |
| ------------------------------------------- | ------------------------------------------- | ------------------------------------------- |
| errors.Error                                | 400, 401, 403, 404, 408, 409, 413, 415, 422 | application/problem+json                    |
| errors.Error                                | 500, 503                                    | application/problem+json                    |
| errors.ContinuousDefaultError               | 4XX, 5XX                                    | \*/\*                                       |

## delete_simulation_snapshot

Deletes a Snapshot and its saved state. Simulations created from it keep running, and their snapshot_id becomes null.

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