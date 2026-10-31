# McpboxSnapshots

## Overview

List, rename and delete the MCP Box Snapshots that Continuous publishes into your workspace. Each holds an MCP server's tools, built against an API Box Snapshot.

### Available Operations

* [list_mcpbox_snapshots](#list_mcpbox_snapshots) - List MCP Box Snapshots
* [create_mcpbox_snapshot](#create_mcpbox_snapshot) - Create MCP Box Snapshot
* [delete_mcpbox_snapshot](#delete_mcpbox_snapshot) - Delete MCP Box Snapshot
* [get_mcpbox_snapshot](#get_mcpbox_snapshot) - Get MCP Box Snapshot
* [update_mcpbox_snapshot](#update_mcpbox_snapshot) - Update MCP Box Snapshot
* [cancel_mcpbox_snapshot](#cancel_mcpbox_snapshot) - Cancel Snapshot build
* [resume_mcpbox_snapshot](#resume_mcpbox_snapshot) - Resume Snapshot build

## list_mcpbox_snapshots

Returns the workspace's MCP Box Snapshots, newest first.

### Example Usage

<!-- UsageSnippet language="python" operationID="list-mcpbox-snapshots" method="get" path="/v1/mcpbox-snapshots" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox_snapshots.list_mcpbox_snapshots(limit=50)

    while res is not None:
        # Handle items

        res = res.next()

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `limit`                                                             | *Optional[int]*                                                     | :heavy_minus_sign:                                                  | Page size. Values below 1 use 50. Values above 200 use 200.         |
| `cursor`                                                            | *Optional[str]*                                                     | :heavy_minus_sign:                                                  | Opaque next_cursor value from a previous page.                      |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.ListMcpboxSnapshotsResponseResponse](../../models/listmcpboxsnapshotsresponseresponse.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 422                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## create_mcpbox_snapshot

Starts an asynchronous build and returns the MCP Box Snapshot with status pending. Get the Snapshot until its status is ready, failed or canceled; MCP Boxes serve a ready one. The build makes an MCP server that answers from the API Box Snapshot it names, run in real time: with a credential, it copies the live MCP server at the credential's base_url; without one, the server its instructions or research name. Builds queue with Simulator builds and count toward the same quotas. Send multipart/form-data with a JSON part named request and optional file parts named document with vendor documentation.

### Example Usage

<!-- UsageSnippet language="python" operationID="create-mcpbox-snapshot" method="post" path="/v1/mcpbox-snapshots" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox_snapshots.create_mcpbox_snapshot(request={
        "apibox_snapshot_id": "<id>",
    })

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                                                                                                                                                                                                                                                                         | Type                                                                                                                                                                                                                                                                                                                                              | Required                                                                                                                                                                                                                                                                                                                                          | Description                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `request`                                                                                                                                                                                                                                                                                                                                         | [models.BuildMCPBoxSnapshotRequest](../../models/buildmcpboxsnapshotrequest.md)                                                                                                                                                                                                                                                                   | :heavy_check_mark:                                                                                                                                                                                                                                                                                                                                | N/A                                                                                                                                                                                                                                                                                                                                               |
| `idempotency_key`                                                                                                                                                                                                                                                                                                                                 | *Optional[str]*                                                                                                                                                                                                                                                                                                                                   | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Optional key for retrying this create safely. A retry with the same key and request returns the original result with its status; the same key with a different request returns 409 idempotency_conflict. Without a key, each request creates a new resource. The key is recorded before the build starts, so a retry never starts a second build. |
| `document`                                                                                                                                                                                                                                                                                                                                        | List[[models.CreateMcpboxSnapshotDocument](../../models/createmcpboxsnapshotdocument.md)]                                                                                                                                                                                                                                                         | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Vendor documentation the builder and reviewer read before they start, as repeated file parts: UTF-8 text (.md, .txt, .html, .json, .yaml, .xml, .csv) or PDF (.pdf). The documents share the request body limit.                                                                                                                                  |
| `retries`                                                                                                                                                                                                                                                                                                                                         | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                                                                                                                                                                                                                                                                                  | :heavy_minus_sign:                                                                                                                                                                                                                                                                                                                                | Configuration to override the default retry behavior of the client.                                                                                                                                                                                                                                                                               |

### Response

**[models.CreateMcpboxSnapshotResponse](../../models/createmcpboxsnapshotresponse.md)**

### Errors

| Error Type                             | Status Code                            | Content Type                           |
| -------------------------------------- | -------------------------------------- | -------------------------------------- |
| errors.Error                           | 400, 401, 404, 408, 409, 413, 415, 422 | application/problem+json               |
| errors.Error                           | 429                                    | application/problem+json               |
| errors.Error                           | 500                                    | application/problem+json               |
| errors.Error                           | 503                                    | application/problem+json               |
| errors.ContinuousDefaultError          | 4XX, 5XX                               | \*/\*                                  |

## delete_mcpbox_snapshot

Deletes an MCP Box Snapshot. A build that is still pending or building is canceled first. The API Box Snapshot it was built against stays. A Snapshot that MCP Boxes serve returns 409 mcpbox_snapshot_in_use; delete those MCP Boxes first.

### Example Usage

<!-- UsageSnippet language="python" operationID="delete-mcpbox-snapshot" method="delete" path="/v1/mcpbox-snapshots/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    c_client.mcpbox_snapshots.delete_mcpbox_snapshot(id="<id>")

    # Use the SDK ...

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## get_mcpbox_snapshot

Returns an MCP Box Snapshot of the workspace.

### Example Usage

<!-- UsageSnippet language="python" operationID="get-mcpbox-snapshot" method="get" path="/v1/mcpbox-snapshots/{id}" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox_snapshots.get_mcpbox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.MCPBoxSnapshot](../../models/mcpboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## update_mcpbox_snapshot

Renames an MCP Box Snapshot or replaces its metadata. Omitted fields keep their current values. Its tools do not change.

### Example Usage

<!-- UsageSnippet language="python" operationID="update-mcpbox-snapshot" method="patch" path="/v1/mcpbox-snapshots/{id}" example="bad_request_body" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox_snapshots.update_mcpbox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                                                    | Type                                                                                         | Required                                                                                     | Description                                                                                  |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- |
| `id`                                                                                         | *str*                                                                                        | :heavy_check_mark:                                                                           | MCP Box Snapshot ID.                                                                         |
| `metadata`                                                                                   | *Optional[Any]*                                                                              | :heavy_minus_sign:                                                                           | Replacement customer JSON metadata, up to 16 KiB and 64 nesting levels, or null to clear it. |
| `name`                                                                                       | *Optional[str]*                                                                              | :heavy_minus_sign:                                                                           | New Snapshot name.                                                                           |
| `retries`                                                                                    | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)                             | :heavy_minus_sign:                                                                           | Configuration to override the default retry behavior of the client.                          |

### Response

**[models.MCPBoxSnapshot](../../models/mcpboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 400, 401, 404, 408, 413, 415  | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## cancel_mcpbox_snapshot

Requests cancellation of a pending or active build and returns the Snapshot without waiting for the build to stop. Get the Snapshot until its status leaves pending or building. The build can finish before cancellation takes effect. A Snapshot whose build has settled is returned unchanged.

### Example Usage

<!-- UsageSnippet language="python" operationID="cancel-mcpbox-snapshot" method="post" path="/v1/mcpbox-snapshots/{id}/cancel" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox_snapshots.cancel_mcpbox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.MCPBoxSnapshot](../../models/mcpboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404                      | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |

## resume_mcpbox_snapshot

Resumes a build that waits at QA because its live credential was rejected, as build.waiting reports. Refresh the credential it names with PATCH /v1/credentials/{id} first: QA then runs again with its current value, and the build goes on to finalize. Returns the Snapshot, still building. The wait does not count toward the build's time limit: the build resumes with the time it had left when it began waiting. A build that is not waiting returns 409 simulator_not_waiting.

### Example Usage

<!-- UsageSnippet language="python" operationID="resume-mcpbox-snapshot" method="post" path="/v1/mcpbox-snapshots/{id}/resume" -->
```python
from continuous import Continuous
import os


with Continuous(
    api_key_auth=os.getenv("CONTINUOUS_API_KEY_AUTH", ""),
) as c_client:

    res = c_client.mcpbox_snapshots.resume_mcpbox_snapshot(id="<id>")

    # Handle response
    print(res)

```

### Parameters

| Parameter                                                           | Type                                                                | Required                                                            | Description                                                         |
| ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- | ------------------------------------------------------------------- |
| `id`                                                                | *str*                                                               | :heavy_check_mark:                                                  | MCP Box Snapshot ID.                                                |
| `retries`                                                           | [Optional[utils.RetryConfig]](../../models/utils/retryconfig.md)    | :heavy_minus_sign:                                                  | Configuration to override the default retry behavior of the client. |

### Response

**[models.MCPBoxSnapshot](../../models/mcpboxsnapshot.md)**

### Errors

| Error Type                    | Status Code                   | Content Type                  |
| ----------------------------- | ----------------------------- | ----------------------------- |
| errors.Error                  | 401, 404, 409                 | application/problem+json      |
| errors.Error                  | 500, 503                      | application/problem+json      |
| errors.ContinuousDefaultError | 4XX, 5XX                      | \*/\*                         |